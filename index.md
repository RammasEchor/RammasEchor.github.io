# Week 11 - Open source / Read others people's code (3)

## 22 Jun 2021 - 28 Jun 2021

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

#### Why use an array?

An array has indexation complexity of `Θ(1)`. This means that the access to one element (given that you know the index) is independent of the array's size.

A *dynamic* array may be resized, but the operation is costly. The only difference they have with normal arrays is that they expand, and when they expand, they essentially double their current size, to account for future resizes.

In my understanding, you may choose to use an array over any other struct when:

- Data will be accessed randomly
- The size of the data may not change often
- The data will be accessed sequentially: This is less dependent on the array than in the OS/microprocessor optimization for sequential memory accesses.

### Linked lists

Is a linear collection of data elements whose order is not given by their physical placement in memory (unlike an array). Instead, each element points to the next. It can be resized.

- Does not need a contiguous block of memory
- Insertion and removal of elements from any position is possible
- Accessing an element requires iterating from the first element to the desired one through all the elements between them. This means any form of indexing is not efficient

The *handler* to the simplest list points to the initial element. The end of the list may be obtained when the current element `next` pointer is `null`. Handles now have more overhead, for example, `size` and `type`.

#### When to use linked lists?

- Data is not usually accessed randomly
- Frequent insertion and deletion of elements
- Memory is not capable of big sequential chunks
- Sequential access to data (although arrays are much better for this)

### Sets

Abstract data type that can store unique values, without any particular order. Rather than retrieving a specific element from a set, one typically tests a value for membership in a set.

#### When to use sets

- When you need to test if a element or value belongs to something

Sets are representations of *finite sets*.

### Heaps

Is a tree-like structure that satisfies the *heap property*: The parent of a node has always a value greater/equal to it's children. This means that there is no ordering between siblings or cousins; only the between the nodes and it's parents, grandparents, etc.

#### When to use a heap

- When creating a priority queue. This ensures that a task needed for other tasks is done first, no matter the order of those other tasks.

Keep in mind that the ordering only applies to parents and children.

### Hash table

Data structure that can map keys to values via a *hash function*. A hash function is a function that can map arbitrary sized inputs to a fixed sized output.

This an implementation of an associative array: A data type composed of a collection of pairs (key, value). It takes the key as an input to a hash function. The output tells the hash table in which *bucket*, or slot it may set, or get, the corresponding value *and its key*.

This mapping may result in collisions, where the hashing function outputs the same value for two different keys, but there there are many solutions to this. The most simple is to advance one slot until it finds one that is empty.

Hashed tables allow for fast indexation of items (like arrays), while keeping the ability of resizing (*rehashing*) like linked lists. Note that this rehashing is not always efficient or fast.

#### When to use hash tables

- Storing key-value pairs
- When access speed is important, and data size changes are minimal
- Lookup tables (tend to be more efficient than trees)

### Trees

Simulates a hierarchical tree structure, with a root value and subtrees of children with a parent node, represented as a set of linked nodes.

A tree has:

- *Nodes*: each element in the tree
- *Child nodes*: Nodes may have children
- *Parent node*:  Nodes always have a parent (except the *root node*). Child nodes with the same parent are *sibling* nodes
- *Root*: The topmost node

A tree may implement some sort of ordering for the nodes, or may not. For example, a *heap* orders the nodes in terms of parents and children, but not in terms of siblings or cousins. A *binary search tree* (binary means that nodes have at most two children) orders its elements in such a way that the right child node is always greater/lesser than the left child. Then, each insertion follows the path to its ordered place.

There are many ways to search in a tree; two of the most common are:

- *Breadth-first search*: Searches all nodes in the same level first.
- *Depth-first search*: Explores as far as possible each branch before backtracking.

#### When to use trees

- Good indexing, insertions and deletion times: `Θ(log n)`
- Good all rounder

If the abstraction is good, readability is not a problem.

### Some thoughts on algorithm complexity

1. How often will the program be used? If only once, or a few times
    do we care about run time? Will it take longer to code than to run
    for the few times it is used?
2. Will it only be used on small inputs, or large inputs.
3. An efficient algorithm might require careful coding, be difficult
    to implement, difficult to understand, and difficult to maintain.
    Can we afford those expenses?

## Conclusions

Working in the Golang issue showed me that not all code is good; I don't mean this in the performance context, but in the *readability* context. I was drawn to Go since it's selling point was a focus on readability and simplicity, but in the issue I worked the variable names often didn't mean anything, and method names worse. But this is totally subjective to me; maybe with more experience under my belt I would be able to make sense of it faster. One of the principal types is `bsoncore.D`. I do think they had a reason to call it that, but to me it was not clear until I read the documentation and the code.

Reviewing the data structures topic, I relearned a lot of things that I had forgotten about. I don't know why, but I enjoy learning about data structures and software design patterns. Like if I read about it, it magically makes me a better developer, ha. Moreover, I read about clean code, and that is what prompt me to ramble about code.

## All blogs

| Blog | Info |
| --- | --- |
| [Week 1 - Innovation and hard\smart work](/Week_Pages/Week1_April.md) | 05 Apr 2021 - 12 Apr 2021 |
| [Week 2 - Polyglot Programming](/Week_Pages/Week2_April.md) | 13 Apr 2021 - 19 Apr 2021 |
| [Week 3 - Fancy Topics](/Week_Pages/Week3_April.md) | 20 Apr 2021 - 26 Apr 2021 |
| [Week 4 - It's all about science](/Week_Pages/Week4_April.md) | 27 Apr 2021 - 03 May 2021 |
| [Pretotypes](/Pretotypes/Pretotypes_April2021.md) | My pretotypes for week 4 (*It's all about science*) |
| [Month 1 - Reset Phase](/Month_Pages/Month1_April.md) | 05 Apr 2021 - 03 May 2021 |
| [Week 6 - Building something from scratch (1)](/Week_Pages/Week6_May.md) | 11 May 2021 - 17 May 2021 |
| [Week 7 - Building something from scratch (2)](/Week_Pages/Week7_May.md) | 18 May 2021 - 24 May 2021 |
| [Week 8 - Building something from scratch (3)](/Week_Pages/Week8_May.md) | 25 May 2021 - 31 May 2021 |
| [Week 9 - Building something from scratch (4)](/Week_Pages/Week9_Jun.md) | 01 Jun 2021 - 07 Jun 2021 |
| [Month 2 - Building something from scratch](/Month_Pages/Month2_May.md) | 11 May 2021 - 07 Jun 2021 |
| [Week 10 - Open source / Read others people's code (1)](/Week_Pages/Week10_Jun.md) | 08 Jun 2021 - 14 Jun 2021 |
| [Week 11 - Open source / Read others people's code (2)](/Week_Pages/Week11_Jun.md) | 15 Jun 2021 - 21 Jun 2021 |
| [Week 12 - Open source / Read others people's code (3)](/Week_Pages/Week12_Jun.md) | 22 Jun 2021 - 28 Jun 2021 |
