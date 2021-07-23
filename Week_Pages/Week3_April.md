# Week 3 - Fancy Topics

## 20 Apr 2021 - 26 Apr 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

This week I learned how to work in a test-driven environment with Maven and Java. I also learned how to create a little user-based recommender, and how to apply this knowledge to an assignment for a Movie Recommender.

## A user-based recommender

I learned how to use Mahout in Java for a recommender
that takes users and their opinion in a product, and
then, when asked for recommendations for a user, searches for users with similar tastes and recommends products that the other users had enjoyed. This little tutorial will help me to solve the assignment of the Movie Recommender.

## Test driven development

Tips for writing code:

1. Think what you want to do and how to test it
2. Write a small test and just enough code to fail it
3. Write just enough code to pass it
4. Refactor the code and see if it still passes
5. If not, fix it. If yes, return to step 1.

Code written in a test-driven environment tends to be
highly reusable. Test code tends to be easy; just a class constructor and a set of assertions. Writing the test code first makes writing the hard part easy. **Design Patterns** should be added incrementally. Prevents scope creep.

Obviously, you may fall into writing tests that doesn't really represent the real requirements. This is where *PairProgramming* shines. Another pair of eyes can spot some things that you overlooked.

Of course, there are disadvantages;

- It may cause more problems to solve (you are not sure if the tests are testing the right thing)
- More work upfront
- End up with overly concrete code that only passes the tests

My takeaway here is to try it. If it reduces the time it takes to deliver, nice!

## Unit Test

Some notions about unit testing are that each test focuses on a small piece of code, a unit; unit tests are expected to be fast.

### Solitary or sociable?

There is two ways to test an object that calls other objects:

- Solitary: Emulate the response from other objects. May fail because those other objects, and not the one you're testing
- Sociable: Make the object call those other objects. You are not testing the "real" environment.

Solitary may be useful when testing with remote servers, due to the server not working as expected, and time savings. Another academy, Alberto Chávez, told me about how his mentors told him that if they use aws servers, they bill the team, so using them in the tests are not a good idea.

### Speed

Another notion is that unit tests are fast, and able to run frequently.
If you need to test a lot of things, maybe trade the depth of the testing, and test only the code you're working on. Then, you run more infrequently tests that cover more ground, but take more time.

## XUnit

From this I learned a little about the origins of XUnit; a family of testing frameworks, from which JUnit is the first to be known. Apparently, someone called Kent Beck was into automated testing, and which better way to test code than with more code? He started making each team build their own testing framework. Then, during a flight with Erich Gamma, they developed the first version of JUnit.

## Mocks Aren't Stubs

This lecture was very interesting, and very well explained. First, there is a misunderstanding between testing techniques:

### Regular Tests

You set up your real classes and test their functionality by means of testing their state after some method calls. This is called **state verification**.

### Test with mock objects

One, or more of the objects being tested (of course, at least one is the one being tested) is a mock. This means that it has no inner workings of its own, and the test is based in the behavior of the object being tested in relation with the mock object.

Focus on the word *behavior*; the mock object checks that the object being tested made the correct calls. This is called **behavior verification**.

### Why are we here

The first case we saw that we can use the real implementation of objects that the object being tested needs to, well, test it. In the second part, we used a mock object in place of those objects. The term "Test Double" is used as an umbrella term for any kind of object used in place of the real object for testing purposes. There are five (5) types:

- Dummy: Passed in parameters but not used
- Fake: Working implementation but with shortcuts
- **Stubs**: Canned answers to calls
- Spies: Stubs that records information about the calls
- **Mocks**: Programmed with expectation about the calls that they receive. Only mocks do behavior verification.

### When to use a mock

Two types of test-driven development:

- Classical: Real objects if possible.
- Mockist: Always use mocks.

You may choose one style over the other based on context. It is simple to use the real object? Classic. It is awkward to use? Mock. There are other considerations:

- Driving Test-Driven Development: It may be useful to get the test started in an object to continue developing it.
- Fixture setup: Mocks are useful because you don't need to wait for the other objects to be complete.
- Test isolation: Mocks ensure that the error comes form the object being tested, not from the objects that it calls.
- Implementation: Mockist test are more bound to the internal implementation of calls; a refactor or a change in implementation can break the test.
- Design style: Classical and Mockist each affect differently the design decisions.

My takeaway is to try, and see which method works best for you.

## Java Build Tools for Dependency Management

As programs use more and more dependencies, tools are used to manage those dependencies. But this tools also introduce more complexity:

- Conflicts between the dependencies
- Reliance on a remote repository

### How they came to be

Throw a jar file which contains the library in the lib folder, or download the library when in the target machine, and pray that the url doesn't change.

Doing this manually was cumbersome and prone to errors.

### Give me the good stuff

Dependencies use a similar syntax between tools. They all have the following elements:

- group-id: Where does it come from?
- artifact-id: Name of the dependency
- version: Which version to use

For example, to add guava in maven we use:

```` xml
<dependency>
   <groupId>com.google.guava</groupId>
   <artifactId>guava</artifactId>
   <version>15.0</version>
</dependency>
````

We can specify a range of versions, but it is not always recommended because they rely on the correct numbering in the repository, and using another version may break the code.

### Transitive dependencies

If two modules use different versions of the same dependency, each tool determine which version to use.

## Missing semester: Vim

Vim is designed around the notion that programmers spent their time reading, navigating, and making small edits. Because this, Vim has multiple operating modes:

- Normal: Moving around and making edits.
- Insert: Inserting text.
- Replace: What do you think? *Deletion of entire project*? No no, this is for replacing text.
- Visual: Selection chunks of text.
- Command-line: running a command.

### Basics

#### Inserting text

Vim starts in normal mode when you open a file. From normal mode to insert mode press `i`. In insert mode, vim behaves like any text editor.

#### Buffers, tabs and windows

- Buffers: Set of open files by vim
- Tabs and windows: Each vim session has a number of tabs, which in turn has a number of windows. Each window shows one buffer.

#### Command line

From normal mode, enter command mode pressing `:`. Then, you can press the name of the command:

- `:q` quit
- `:w` write (save)
- `:wq` save and quit
- `:e {filename}` open for editing
- `:ls` show open buffers
- `:help {topic}` open help

#### Vim's interface is a programming language

Keystrokes are commands, and these commands compose: that means they add each other. There are a lot of examples, and I tried each of them. I bookmarked this page to return to it when I start to use vim more.

## Missing semester: Git

Version control systems are used to track changes to source code, or other collections of files and folders. There are a lot of VCS, but git is the standard.

### How git works

Git models the history of a collection of files and folders within some top level directory as a series of *snapshots*:

- Blob: A file
- Tree: Directory

A snapshot is the top level tree. Each snapshot is that tree in a point in time. In the repository history, each snapshot has a set of parents; the preceding snapshots. Each point in a history is a *commit*. Each commit has an author, parent(s), message, and the snapshot.

In git, where our tree is in the commit history is a reference called HEAD.

Git doesn't create a snapshot of the entire tree. It takes what is it in the *staging area* and creates a snapshot with it.

There were a lot of commands, but I focused on the exercises at the end of the reading. With this reading I feel more confident using git.

## Pale blue dot

To put in context of the immensity of space the human home world tragedies, it really seems pointless not to treat each other with kindness. I would love to walk on another planet.

## The future of programming in 1973

How may a computer-enthusiast predict the future from 1973? We had Moore's law predicting the future trends of transistor size, and, more importantly, people resisting change. When assembler came out, people thought that it was unusable, that why anyone should write anything more than machine code?. When fortran came out, they resisted the change from assembler.

The future may show direct data manipulation, because, if there is a 1962 prototype of software directly manipulating data, why not in the future? There is prototypes about programs and languages that tells the computer what we want, instead of a series of steps to reach a goal. That should be ubiquitous in the future, right? It is much more easier!

Parallel programming should evolve beyond one processor per memory, and beyond locks and thread. A model that fits the hardware.

My takeaway is that people resist change. We took a direction, and we just take it to it's limits. Now, with Moore's law declining, we can't just keep doing that for diminishing returns. If you think you know what you're doing, you stop searching for more ways to do it.

## Inventing on principle

Creators need a connection with what they're making, to know what does the changes do. A lot of creating is discovering what does this do, how does this work with other things.

The video shows us a very peculiar tool. It allows you to see immediate changes in something that the code is creating, but we don't need to compile again and again. That is the point, that any delay in seeing the changes hinders our ability to create, because we may not try something because it may take too much time for us to see the results.

I think this was my favorite video; a lot of times I draw on a whiteboard the data structures and then simulate their state in a point in time to construct an algorithm.

## Machine learning

Create algorithms that can define, or "learn", the rules for us for a problem. So, for example, if we want to classify a photo of something, we may write conditions, but it is far easier find patterns in different photos of the same object, and NOT find patterns in photos without the object.

A dataset is a lot characteristics of an object with a label saying *what that object is*. We train machine learning algorithms with one set, and then test them with sets that they never seen before.

For machine learning, it is very important the dataset that you're giving to the algorithm, because from there it infers the rules. Characteristics in a dataset are very useful if they are characteristic of the labels and they are not correlated between them.

The neighbor classification algorithm checks the nearest points from a point, where each point is an object, and then classifies that object as the same type of the nearest objects.

Tensor Flow is incredibly useful for datasets that consists entirely of images; it saves you from extracting each characteristic from an object in a image.

## Moonshot thinking

Moonshot is where there is a clearly defined problem. There has to be a proposal to solve the problem, and a scientific reason behind that proposal.

It is often easier to make something 10x better than 10% better. That is because if we try to make something 10x better, were taking risks, and taking risks is seen as expensive and a waste of time. But if you're working in that 10x, then you can get better people because it is more interesting to work in, and then you make more progress and learn more, and the circle continues.

If you reward fail and learn, people tend to be more honest, and more prone to participate with new ideas. Making this fail and learn process part of that interesting thing that makes the world 10x better draws people to the project, and makes them want that project to succeed.

## Inside Google X

Their job at Google X is to find a problem that is a problem for millions of people, and find a breakthrough solution. The process of innovation is expensive and time consuming. Even this guys at Google X can only pursue a small set of moonshots. So, they try to fail quickly. It may be almost as good to reach a no in an idea than a yes. The point is to try and learn, and build over that knowledge.

## The best presentation of your life

All presentations are important. In a company, presentations are the way that causes most impact in people. But presenting exposes you, and it is totally valid if you're afraid of slips. You want your audience to think, and to feel.

Making them think is to make them work a little to work up with you, but not to make it seem like a study session. There are some tips:

- Pique their interest:
  - Give something useful for them
  - Explain at the beginning why it is useful
  - Why they should believe you

- Minus is more:
  - We base our preferences in how much we understand

- Give order to ideas:
  - Give a general explanation at the beginning
  - Divide in chunks the information
  - Give structure to concepts

- Associate: How the concepts bind to known things

To make them feel, you want them to participate. Show them that every participation is valid. Choose someone to ask, don't just leave the question hanging. Relate to the present, to the room. Emit the emotions that you want to generate in the audience.

## What got you here won't get you there

The more successful we are, the more delusional. There are some bad habits that successful people make:

- Winning too much: When you can't win, you get angry
- Adding too much value: The need to add something to others ideas.
- Telling the world how smart we are: We can't stop bragging.
- Pass too much judgment: Judge everything.

Sometimes the problem is not what we are not doing, it is what are we doing, or how are we doing it. Stopping a bad habit and changing it to a good one is very difficult.

There is value in listening, and in trying to help another person. Don't just dismiss their ideas, or try to one-up their experiences. Just listen, and say thank you. This works as well in the office as in home.

## Quantum computers

The problem in our journey to reach the fastest processor passes through shrinking the components of it. Because this, when a transistor is very, very small, it may start stop working as a reliable switch. This is causes by electrons just skipping the barrier that is supposed to stop them in a phenomenon called *quantum tunneling*. Maybe we can solve this, but maybe it is time to search for a completely different approach. Quantum computing takes advantage of qbits; bits that can be in a state called superposition. This means that each qbit can be either `1` or `0`, and it is not known until it is measured. This loosely means that those qbits contains all the possible responses at the same time. This could exponentially speed up complex computations and also make current encryption methods obsolete.

## Google and NASA's quantum computer

 The intro is amazingly well made, and it really makes you invested in what the rest of the video has to say.

My takeaway is that quantum computers are much better at optimization problems; this means a problem where we need to search all possibilities for us to reach the best solution.

## Dr. Quantum: The observer and the duality of a wave particle

What the heck?

Just kidding... maybe. The double slit experiment involves shooting electrons into a plate; this plate has two slits. If we shoot matter, like some salt grains, the salt that passes through the slits makes a two band pattern. When we put the plate in water, the waves created that pass through the slits create a multi-band pattern. What I understood is that electrons, when we are shooting them through the plate, behave like a wave when there is no measuring device, but like matter when there is one.

## Measure for Measure

Using the same experiment mentioned above, we try to understand the connection between waves and particles.

### Waves and probability

A wave is just a representation of where a particle most probably is. So, if we see the electron beam as a beam of waves, and each wave tells us the probability of the electron being in one region, we can create the multi-band pattern.

### Quantum algorithm and it's discontents

A wave may show that an electron may probably be in one place. But what if it shows that it is probably it is in two places at once? What would happen in the quantum algorithm, that tries to predict where a particle will be based on it's history?

I thought that I was maybe keeping up, but I really don't understand this part. I know they're talking about four models, but I don't know how each model attempts to answer the question of why an electron behaves like a wave.

## The Quantum Conspiracy

This is about the differences between what you read about quantum mechanics and what the actual truth is.

How do we know that what we measure corresponds to actual reality?:

- Make experiments more than once, and get consistent results

This occurs because reality it is accurately reflected. Or is it? Turns out we can demonstrate that not really.

If we measure one slit of the two slit experiments, we may think that we cheated the problem. After all, is we doesn't measure anything, that means the electron passed through the other slit. But there is something, something that is at both places at once, that when we measure *anything* the electrons stop behaving like a wave. If two particles are entangled, when an aspect of a particle is measured, the other particle changes in response, even when separated by large distances.

I may need to revisit this two last videos; they are over an hour long and I didn't understand very much anything in them.

## Conclusions

I learned a lot, mainly about Moonshot thinking because it was the underlying theme for various videos; I learned that it is good to take risks, to try to make the world a better place, and that that way of thinking attracts valuable people. I learned about Vim and Git that I didn't know, and I also learned a lot about unit testing. Using Maven with Java showed me how easy is to manage dependencies, and the readings from Martin Fowler gave very useful information about test driven development concepts. I wouldn't say that I learned about quantum mechanics and so; I would say that I learned what I don't know.

### Back to main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)
