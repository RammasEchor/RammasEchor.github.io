# How does a consumer gets a message in Apache Kafka?

## 08 Jun 2021 - 14 Jun 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

The first question have to do with Apache Kafka. To answer this, I read the code, and the answer is based only on what I read.

## Ultra quick and short intro to Apache Kafka

Kafka provides a service of *event streaming*.

> "Event streaming is the digital equivalent of the human body's central nervous system. It is the technological foundation for the 'always-on' world where businesses are increasingly software-defined and automated, and where the user of software is more software." *From kafka: <https://kafka.apache.org/intro>*

This may be translated to: Capture data from event sources in the form of streams of events. Then, storing, manipulating, processing, and reacting to these streams.

Kafka has three key capabilities:

- Publish and subscribe to streams of events
- Store streams
- Process streams

An *event* records the fact that something happened in the world. An event has a key, value, and a timestamp:

```` json
Event key: "Luis"
Event value: "Ate a hamburger"
Event timestamp: "Jun 8, 2021 at 8:29pm"
````

**Producers** are those clients that publish events to Kafka, and **consumers** are those that subscribe to these events. Events are organized by *topics*.

## Before the questions

I cloned the open source code locally, and started reading the code. Kafka has a quick-start demo, which uses the producer and consumer. **The comments were added by me based on what I understood**:

```` java
// Already we can see that Kafka packages all the source code used in here.
package kafka.examples;

import org.apache.kafka.common.errors.TimeoutException;

import java.util.Optional;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.TimeUnit;

public class KafkaConsumerProducerDemo {
    public static void main(String[] args) throws InterruptedException {
        // Async operation if there is no args OR if there is not an arg called sync
        boolean isAsync = args.length == 0 || !args[0].trim().equalsIgnoreCase("sync");
        // This is a kind of lock; each call to latch.countDown() is syncronized between the consumer and the producer.
        CountDownLatch latch = new CountDownLatch(2);

        Producer producerThread = new Producer(KafkaProperties.TOPIC, isAsync, null, false, 10000, -1, latch);
        producerThread.start();

        // This two lines is what I will be answering below
        Consumer consumerThread = new Consumer(KafkaProperties.TOPIC, "DemoConsumer", Optional.empty(), false, 10000, latch);
        consumerThread.start();

        // Timeout for the demo
        if (!latch.await(5, TimeUnit.MINUTES)) {
            throw new TimeoutException("Timeout after 5 minutes waiting for demo producer and consumer to finish");
        }

        consumerThread.shutdown();
        System.out.println("All finished!");
    }
}
````

## First question: How does a consumer gets a message?

A `KafkaConsumer` implements the `Consumer` interface, which in turn extends `Closeable`:

```` java
public class KafkaConsumer<K, V> implements Consumer<K, V> {
    ...
public interface Consumer<K, V> extends Closeable {
````

In `Consumer.java` (which is the *example* implementation) we have this:

```` java
public Consumer(final String topic,
                    final String groupId,
                    final Optional<String> instanceId,
                    final boolean readCommitted,
                    final int numMessageToConsume,
                    final CountDownLatch latch) {
````

This let us map each argument in the demo to what they really mean:

```java
Consumer consumerThread = new Consumer(
    KafkaProperties.TOPIC,  // Topic to subscribe to.
    "DemoConsumer",         // Name of the group of consumers
    Optional.empty(),       // Instance Id
    false,                  // "Controls how to read messages written transactionally." Since this is false, it will return all events, even those aborted (not commited)
    10000,                  // Number of messages to consume
    CountDownLatch          // When this reaches 0, the program ends. Basically, we initialize in 2, so when the producer and the consumer countDown() each once, we reach 0, and know that those threads ended.
);
```

This is the `Consumer` used in the demo, and for each parameter, we create a property. For example:

 ```` java
Properties props = new Properties();
...
 props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
 ````

Then, we create a new consumer from `KafkaConsumer` with those properties as args:

```java
private final KafkaConsumer<Integer, String> consumer;
...
consumer = new KafkaConsumer<>(props);
```

In this example implementation file, we have two main methods:

```java
// We subscribe to the given list of topics.
consumer.subscribe(Collections.singletonList(this.topic));
// We poll the events from each topic.
ConsumerRecords<Integer, String> records = consumer.poll(Duration.ofSeconds(1));
```

Those methods do all the work; lets see how they look in the inside. After we are sure that we use `KafkaConsumer` we can dive into `KafkaConsumer.java` and know how it asks for an event. In this file we have `poll()`:

```java
private ConsumerRecords<K, V> poll(final Timer timer, final boolean includeMetadataInTimeout) {
    ...
    final Map<TopicPartition, List<ConsumerRecord<K, V>>> records = pollForFetches(timer);
    ...
    return this.interceptors.onConsume(new ConsumerRecords<>(records));
}  
```

Now, we call another function to fetch events for us, which in turn calls an object called `Fetcher` to, you guessed it, fetch the records:

```java
private Map<TopicPartition, List<ConsumerRecord<K, V>>> pollForFetches(Timer timer) {
...
fetcher.sendFetches();
...
client.poll(pollTimer, () -> {
            return !fetcher.hasAvailableFetches();
});
...
return fetcher.fetchedRecords();
```

This sends us to `Fetcher.java`:

```java
public synchronized int sendFetches() {
    ...
    // Future for the fetches that may happen.
    RequestFuture<ClientResponse> future = client.send(fetchTarget, request);
    ...
    // Callback function for the future.
    future.addListener(new RequestFutureListener<ClientResponse>() {
        public void onSuccess(ClientResponse resp) {
            ...
            // Create a set from the events
            Set<TopicPartition> partitions = new HashSet<>(response.responseData().keySet());
            ...
            // and process it to create partitions and events.
            for (Map.Entry<TopicPartition, FetchResponseData.PartitionData> entry :     response.responseData().entrySet()) {
                TopicPartition partition = entry.getKey();
                FetchRequest.PartitionData requestData = data.sessionPartitions().get(partition);

                FetchResponseData.PartitionData partitionData = entry.getValue();
            }
            ...
            // We add to a global queue all the events we recieved.
            completedFetches.add(new CompletedFetch(partition, partitionData,
                                            metricAggregator, batches, fetchOffset, responseVersion));
        }
    }
}
```

So, to really answer the question, here are the steps:

- We create a `Consumer`, which in turn has a `KafkaConsumer`, and we pass the topics to subscribe to.
- We subscribe to the topics, and poll for the events. `KafkaConsumer` asks `Fetcher` for events that we may already have. If there is no events, we ask `Fetcher` to send a fetch.
- `Fetcher` asks `ConsumerNetworkClient` to send a request for events, and this request creates a `future`.
- `Fetcher` adds a callback for this `future`, that process the response and sets up the information received in a concurrent queue; `completedFetches`.
- The, we ask `Fetcher` for the fetched records, which in turn it retrieves it from `completedFetches`.
- After this, `KafkaConsumer` gets the events, and returns it to `Consumer`.
- `Consumer` returns the `ConsumerRecords`.
- Now we may work with the fetched records (events).
