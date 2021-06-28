# Week 11 - Open source / Read others people's code (3)

## 22 Jun 2021 - 28 Jun 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## *Mongo DB Go driver* contribution

### Remove unused typesafe BSON API

[Link to issue](https://jira.mongodb.org/browse/GODRIVER-1953)

### Context

There is a package (a collection of code) called `bsonx` that contains some proof of concept types used in version `1.0` of the driver. Since version `1.1`, those types were replaced by a package called `bsoncore` when a refactor occurred. These types should be removed as they are not longer being used or useful.

I think that there is some backwards compatibility shenanigans going on, because some files use the package `bsonx`, but `bsonx` itself calls some methods from `bsoncore`.

### Status

Right now I created a draft pull request ([Link to draft](https://github.com/mongodb/mongo-go-driver/pull/692)) with some questions about the issue, but so far nobody has responded.

My approach was to refactor the straggler cases of `bsonx` to `bson` or `bsoncore`; since the packages are fairly similar this seemed like a good approach.

I tried to reach the maintainers via the recommended channels in the `CONTRIBUTING.md` readme, but no one answered to my comments, so I created the draft pull request in my attempt to receive feedback.

## Topics for the interview

This is what I learned investigating the topics. I put some *when to use what* about each struct, but, drawing from what I learned so far in the Academy, I think that *readability* and *maintainability* triumphs over any use case. Only when the speed is needed we should use a complex struct.

### Arrays

Collection of items stored at a contiguous memory locations. Data is accessed via indexes. It cannot grow in size, or shrink.

- Allow random access to elements
- Have better cache locality, which is access to elements within relatively close storage locations. This means that this operation may be optimized via caching, prefetching, predicting.

Mathematically modeled, an array has two operations:

- `get(A, I)`: Get the *I* element from the array *A*
- `set(A, I, V)`: Set the value *V* in the array *A* with index *I*

Those operations mean that each element behaves like a variable and that storing a value in one element does not affect the value of any other element.

#### Dope vectors

This is the array's *descriptor*: A record that contains the dimension *d* (number of indices needed to specify an element), the base address *B*, and the increments *c1, c2, c3...*. This dope vector is a complete handle for the array.

A handle is an abstract reference to a resource, and it is used to manipulate that resource.

### Why use an array?

An array has indexation complexity of `Θ(1)`. This means that the access to one element (given that you know the index) is independent of the array's size.

A *dynamic* array may be resized, but the operation is costly. The only difference they have with normal arrays is that they expand, and when they expand, they essentially double their current size, to account for future resizes.

In my understanding, you may choose to use an array over any other struct when:

- Data will be accessed randomly
- The size of the data may not change often
- The data will be accessed sequentially: This is less dependent on the array than in the OS/microprocessor optimization for sequential memory accesses.

### Linked lists

### Some thoughts on algorithm complexity

1. How often will the program be used? If only once, or a few times
    do we care about run time? Will it take longer to code than to run
    for the few times it is used?
2. Will it only be used on small inputs, or large inputs.
3. An efficient algorithm might require careful coding, be difficult
    to implement, difficult to understand, and difficult to maintain.
    Can we afford those expenses?

## Conclusions

