# Week 10 - Open source / Read others people's code (1)

## 08 Jun 2021 - 14 Jun 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

Hi! This week in Academy we kicked off the phase *Open source / Read others people's code*. This week I answered four questions about open source projects by reading their code. I also learned about some general guidelines about contributing to open source projects. Here is what I learned this week:

## How to contribute to open source

This is a quick lecture about general guidelines.

### Why contribute to open source?

There are a variety of reasons why contributing to open source is a good idea;

- Improve software you rely on
- Improve your skills
- Find mentors and teach others
- All your open source is public, which means that you may show as a demonstration of what you can do.

### What it means to contribute?

You don't have to contribute code; in fact, it is often the other parts of the project (documentation, translations) that are the most neglected.

But code is also fine.

### Anatomy of a open source project

Every open source community is different, but many open source projects follow a similar organizational structure:

- Author: Creator of the project
- Owner: Not always the same as the author, is the person with administrative ownership over the organization or repository
- Maintainers: Contributors that drive the vision and managing the organizational aspects of the project
- Contributors: Everyone who has contributed something back to the project
- Community members: People who use the project

A project also has documentation:

- License: *Open source license*
- README: Instruction manual that welcome new community members
- CONTRIBUTING: This helps people to find ways to contribute to the project
- Code of conduct: Rules for participants
- Other documentation

And finally, open source projects may use tools to organize themselves:

- Issue tracker
- Pull requests
- Forums or mailing lists
- Chat channel

### Finding a project to contribute on

There is a fairly long list of recommendations to check when searching for a open source project. Summary is listed below:

- Meet the definition of open source
- Actively accepts contributions
- Is welcoming

### How to submit a contribution

#### Communicate effectively

Before opening an issue of pull request, keep some points in mind to help come across your ideas effectively:

- Give context: Make sure the other person knows what you are talking about without making them search for it if it is not necessary.
- Homework: Show that you tried to understand what you are asking.
- Keep requests short and direct: Be concise, and don't waste peoples time.
- Keep all communications public: Don't reach privately, what you are asking may help more people in the future.
- It is okay to ask questions.
- Respect community decisions.
- **Keep it classy**

#### Gather context

Check to make sure your idea hasn't been already discussed. Search for a few key terms. Before opening an issue or pull request, check the projects contributing docs to see how things are done.

### After the submit

You may not receive a response, tasked to rework the contribution, get rejected, and better, get accepted. This is normal, and don't get discouraged if it happens to you. Remember, whatever happens, keep it classy and respect the decision being made.

## Apache Kafka

The first two questions have to do with Apache Kafka. To answer this questions, I read the code, and created a diagram to help visualize the answer. This answer is based only on the code.

### Ultra quick and short intro to Apache Kafka

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

### Before the questions

Kafka has a quick-start demo, which uses the producer and consumer. **The comments were added by me based on what I understood**:

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

        // This four lines is what I will be answering below
        Producer producerThread = new Producer(KafkaProperties.TOPIC, isAsync, null, false, 10000, -1, latch);
        producerThread.start();

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

### First question: How does a consumer gets a message?

