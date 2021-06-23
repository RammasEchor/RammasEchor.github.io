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
