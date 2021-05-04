# Week 4 - It's all about science

## 27 Apr 2021 - 03 May 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## What the heck is the event loop anyway?

I'm going to put here some concepts that I investigated to understand better this video:

- Runtime: This is the execution of a program. Is everything that is loaded in memory and belongs to the program.
- Call stack: The stack of functions that we called so far; when you get the stack trace when debugging, this is it. A log of what functions were called until this point, and, of course, it is a stack, so if a function called another function, it is "nested" in a way in the stack call; you always return to the function that is below in the stack. (Ha, this appeared later in the video and better explained than wikipedia)

Javascript is a single threaded, asynchronous, concurrent, non-blocking language, with a call stack, an event loop, a callback queue. It is a single treaded runtime; it has only one call stack. This means that it can only do one thing at a time. So, if a function takes time to complete, Javascript will wait (*block*) until that function completes (*returns*). Network requests may take seconds, or they may never complete. This is a problem, because our program can keep blocking indefinitely. This also means that the user interface will also block until those requests complete. It can't run any other code.

This is solved by asynchronous callbacks. Asynchronous callbacks don't run immediately, but we can't push into the stack (because it will run immediately). So, we have the V8 runtime, in which Chrome runs, and the browser also adds this web API's. This are effectively threads that we can make calls to, and those pieces of the browser are aware of this concurrency.

Little side note: Node works like this, but instead of web API's calls, we call C++ API's to thread.

Those threads work in the background, and when they finish what they had to do, they do a *callback*; we passed to them a function, something to do with the result after they finish, and push that function into the *task queue* or *callback queue*.

The **event loop** job is to look at the stack, and to look at the callback queue. If the stack is empty, it pushes the first thing in the callback queue, and effectively runs that.

## The pretotyping manifesto

Most new ideas fail, even if they are very well executed. Make sure you're building the right thing before you build it right.

An idea doesn't mean anything if is not executed. Innovators do this, they concrete and idea, and see where it leads them. This is better, because this products are data, and data is much more valuable than just the idea.

Learn how to fail fast. This let's you learn fast, and statistically, you will end up with more successes. This is what it means to create a prototype: to create something that anything but resembles what you're trying to do, and see if you, or the people, use it.

### My pretotyping

I'm going to test three (3) ideas, and show you how I used the pretotyping technique. I create a page for all three experiments, and you can check out them here:

[My pretotypes](/Pretotypes/Pretotypes_April2021.html)

But here is a fast description of them:

#### Long distance fire starter

Now, bear with me; you can't burn down your ex's house. This is a way to lit up your water heater via a network. This means remote start.

#### Super-leash

This leash is very long; it lets your dog run around almost as if had no leash.

#### Kindle mount

I wanted to see how feasible would be a mount for you to read your Kindle while doing something else with your hands, like driving. Just kidding, like washing the dishes.

## Programming the Universe

The universe is always computing something. This is because every atom carries bits of information with them. Every time one particle collides with another, it changes, and this changes that bit of information. At a most basic level, the universe computes. So, quantum computing asks those particles for what they are already doing.

We give meaning to information. We can see particles as particles and waves at the same time. A wave just tells us the probability of a particle being on one place. So, because some waves can tell us that this particle can be in two places at once, we can do two operations at once, or three, or one million operations at the same time. This makes quantum computers reduce extremely complex problems to a paralleling programming problem.

## Quantum Machine Learning

I think this is the most complex video from the weekly assignments. To convert machine learning algorithms to it's quantum machine learning algorithms counterparts, we may map each operation that has to do with vectors, with matrices, with linear transformations, because we know how to do those things at the quantum mechanical level. And this enables us to represent information as qbits, and each qbit is in a superposition; this means that is in all states at once. The speed up is that quantum computers can do a lot of operations at the same time; they can manipulate cˆ2ˆ*n* using only *n* qbits. This is what makes this algorithms much more better in quantum computers: the speed up.

## Why each one of us should have our own black box?

The aviation industry learns a lot from it's mistakes. When a plane crashes, they go and retrieve the black boxes from a plane, they analyze them and say "What we can do so this doesn't happen again?". This is a growth focused culture.

Vice versa, the medical field is focused on talent. You don't reveal, and even hide, your mistakes because of the fear of being seen as untalented. This is a high blame culture.

Learn from your mistakes; the worst thing would be to slip on the same rock from before.

## Richard Feynman: The Great Explainer

Richard Feynman was a Nobel Prize winner for his contributions to a better understanding the Quantum Electrodynamic Theory; this theory attempts to explain how light interacts with matter at the smallest possible level. This also could explain how everything worked. He created the Feynman diagrams; a time diagram that had the equations corresponding to each moment in time for a particle collision. This was the work that awarded him a Nobel Prize.

## Computing the theory of everything

Wolfram Alpha is a project that attempts to respond to questions by *computing* them. This means that it doesn't just responds something that someone wrote before, but delves into a huge amount of information and tries to retrieve the answer by creating relations between the information. This also has the advantage of being real time questions.

If we can compute any answer, that means that we can just compute a set of rules for universes, and search for the universe that evolves to be the same as the one we're in; this could gives us insight on what the universe may be based on.

## Feynman and computation

Little stories about how Richard Feynman helped the computing field.

### Parallelism

He devised a way to have multiple computations going at the same when computing the implosions of the atomic bomb. He essentially invented a technique of parallel computing called pipelining.

### Conference of Slovnick

He had methods for speeding up computations; he computed in one night what it took Slovnick six months.

### Quantum computing

Simulating nature on classical computers is impractical, so he devised the idea of a quantum computer.

## Reminiscing about Richard Feynman

The presenter, Danny Hillis, tells us about he got to know Richard Feynman, and about his philosophy about people:
> When you know, and make an impression on people, a little bit of you rubs on them. This is how one stays with them after death.

## Feynman on scientific method

We look for a new law like this:

- Guess it
- Compute the consequences of the guess
- Compare those computations results with nature, with experiments

If it doesn't correspond with the experiments, then the guess is not right. Keep in mind that experiments that may be done in the future may disprove the guess.
Ideas really don't have places on the scientific method: Concrete formulation of those ideas are what we can work with.

## Tools for continuous integration at Google scale

In a standard continuous build system, you create some changes, submit those changes, and test over those changes. But at Google scale, changes happen too fast. So, Google uses testing on each change, to pinpoint exactly what does a change break. This may seem expensive,but Google uses a dependency tree checker to only test what may be affected.

This approach helps with iteration times of the development cycle, lowers compute costs, and identifies what does broke what precisely. But it also has drawbacks; uses a lot of compute power to compute each dependency tree after each change.

So, this may be integrated with more tools, like a presubmit tool that tests some things before submitting your code.

### Flaky tests

They are tests that are not reliable to give a consistent result. You may fix them, hide them, or track and provide some kind of metric to fix it. Sometimes it is more expensive to fix this tests than to just ignore them.

## The release process for Chrome iOS

It is very different to develop a browser for a OS that you own, than to trying to squeeze in a market dominated by another browser. For example:

- Restricted OS API calls; Safari can use all the API calls that it wants. Chrome is not so lucky.
- Support for the devices; More easier in Chrome iOS than for Chrome Android.

The release process is as follows:

- Use the beta programs internally, because Apple didn't allowed this type of apps.
- Start feature approvals by Apple: May need to wait two (2) weeks.
- Once approved, push it to the App Store.

They use automated testing with bots on development channels so to streamline the development process:

- Unit tests
- End to end tests: User-like usage, like opening a browser, searching for something, and closing it again.
- Performance tests
- Screenshot tests: To check how the changes changed, if changed, the display of webpages.

## I don't test often, but when I do, I do it in production

Netflix has a really big and complex distributed system; this causes exhaustive testing to be almost impossible. There are some things that can be done to mitigate failure, like Exception handling, redundancy. But with the size of Netflix systems, this is not enough. They created the Simian Army; a type of testing that injects errors directly into the production environment. For example, the Chaos monkey terminates instances on the servers randomly. Now, this is a problem that can happen fairly often, and because it has been tested, now it barely causes an inconvenience. The latency monkey, that simulates a degradation on services.

Also, they use analytics tools to check the usage patterns of code, and focus testing by prioritizing frequently executed paths with low test coverage, and remove unused code.

## Test coverage at Google

What is code coverage? This is a code review that uses tools like web based testing, that let's you test code via the web, and you may start it before your commute and check the results at home. Then, there is the mandatory code review by peers, helped by automated testing. Reviewers see the status of the tests too, so they can accept or reject the code based on those results.

## Testing user experience

To make engineers write tests you need to excite them about them. Also, and this is more difficult, making them fix the bugs that get uncovered in the tests. For this, Google implemented a type of integration with the compiler; every time it sees a pattern, something that may trigger an error, it enforces this inside the compiler. This are called suggested fixes.

A complete representation of the results of a test lets the engineer find the root cause more quickly. The presenter is trying to create a standard for a schema for test results. This would be easier to work with, since every testing tool would have the same output.

## Chaos engineering

Chaos engineering is the field that breaks things in order to make them more resilient. To find the failures before they become something bigger.

To create a plan to break your system,  you need some principles:

- Plan an experiment
- Contain the blast radius: Execute the smallest test that will teach you something.
- Scale or squash: If you don't find problems, keep scaling the blast until you find one.

Chaos engineering lets you vaccinate your system against those things that you may not even think they may occur. There are some fallacies that new engineers to distributed systems fall into:

- Reliable network
- Zero latency
- Infinite Bandwidth
- Secure network
- Unmovable topology
- One administrator
- Zero-cost transport
- Homogenous network: Every machine in a network is the same

When planning what to break, follow this guidelines. Experiment first on:

- Known things that you understand
- Known things that you don't understand
- Things you understand but not know
- Things you don't know and don't understand

To create an experiment, just ask yourself "What could go wrong", formulate an hypothesis of what would happen. Then, after creating a roll back plan,do that, and measure the impact of that experiment. Fix the bugs that your experiments uncovered.

## Breaking things at Netflix

Anti-fragility is to be capable to adapt. Something that actually that gets better on the face of change. This can be tested with some breaking, like vaccinating. You get a little sick so you can get immune to what you're vaccinating against.

Fit is a tool used to control the blast radius of the Simian Army, because the monkeys worked really well; so well that the engineers were wary to let the monkeys loose because of the fallout.

Netflix uses injection of errors into requests from real users to Netflix. Of course, this need to strike a balance between testing and degrading the user experience, something that no one wants.

## Conclusions

I learned about various techniques big companies use for testing, like the Simian Army, and Fit. I learned that frequent testing leads to a good product, and a product with little down time. I learned that sometimes the usual testing techniques are not enough, or very, very expensive, and that there are a lot of creative solutions. I learned about the scientific method, and why is it important to have a base to start from. This connects with the idea from the first week; "An idea is worth nothing; it is all in the execution". In the scientific method, you concrete that idea into a formulation, and test it. I learned about the event loop and Javascript, how is it that it work asynchronously. I learned about Richard Feynman, and his extraordinary life.

### Back to main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)
