# Month 3 - Open source / Read others people's code

## 13 Jul 2021 - 09 Aug 2021

---

## Cracking the code interview

### The interview process

- Talk out loud throughout the problem, and explain your thought process. Show everyone that you are capable of analyzing a problem, and proposing a solution.
- Do not sullen about a question you were not able to answer; most probably it was hard for everyone. Interviewers assess you relative to other candidates.

### Behind the scenes

This chapter is a walkthrough of a few interview process in tech companies (Microsoft, Amazon, Google, Apple, Facebook, and Palantir).

While some companies value some skills more than others, the main point from this chapter is to be prepared; each interview may have some things that others don't take into consideration, but that does not mean that you should disregard those topics.

### Special Situations

There is differences in applying for a tester position than for a management position. For example, a tester may not be faced with *customer focus* questions, but a manager definitely will.

There is some tips for interviewers that I may use in constructing my mock interview:

- Ask medium and hard problems.
- Look for questions with multiple hurdles.
- Use hard questions, no obscure knowledge.
- Avoid scary questions (eg. Math, Probability, Low-level knowledge, etc).
- Offer positive reinforcement.
- Coach candidates:
  - Brute force algorithms are okey for starting a problem
  - Steer the conversation towards what you want to see
  - Guide them through examples

### Before the interview

- Get the right experience: Take big project classes, get an internship, start something for fun in your free time.
- Write some great resume: Short, concise, and focused on what the job requires.

### Behavioral questions

Are asked to know about you personality, to understand your resume more deeply.

Tips:

- Gather info about each component of your resume, so you can talk about them in detail. A table like this is useful:

| Common Questions | Project 1 | Project 2 | Project 1 |
|------|------|------|------|
|Challenges|
|Mistakes/Failures|
|Enjoyed|
|Leadership|
|Conflicts|
|What You'd Do Differently|

- Answer truthfully about your weaknesses, but don't stop there! Emphasize how you work to overcome it.
- Ask some questions to the interviewer. Personally, I liked the ones about *passion*: What I can learn here?
- Know technical details about your projects.
  - Challenging components
  - Central role
  - Technical depth you master

- Be specific: give out the facts
- Limit the details: state the key points
- Focus on yourself, not on your team
- Give structured answers
  - Nugget first: Start with a very short overview of the answer.
  - S.A.R.: Situation, Action, Result(Measurable is preferable).

- Tell about yourself in a structure:
  - Current role (Headline)
  - College
  - Post College and Onwards
  - Current Role (Details)
  - Outside of work
  - Wrap Up

- Sprinkle in Shows of Success

### Big O

Is the concept of asymptotic runtime. Your algorithm (sorting, multiplication, etc) may take some amount of time to complete. Usually this is expressed as an equation. For example, for each element, where the number of elements is `N`, you may construct and algorithm that sums all elements. Since this algorithm has to *visit* each element (to know its value), it takes `N` time. This means that if you have 10 elements, you will spend 10 time units. If there is 50, you will spend 50 time units. This is a lineal relationship, and the big O notation for this algorithm is `O(N)`.

But however the input size changes, you *will not* surpass this ceiling. If you put 1000 elements, it will no take 1001 units of time to complete. Big O notation is the ceiling asymptote for an algorithm.

In the industry, its meaning is more close to *this algorithm takes this time* instead of *this algorithm will not surpass this time*.

This runtime is called time complexity; and there is a term for space: *space complexity*. It is the same, but in terms of the space that it will use.

Big O just describes the rate of increase for a given input.

### Technical questions

- Prepare solving exercises, like coding problems.
- Absolute, must have knowledge:

| Data Structs | Algorithms | Concepts |
|-|-|-|
|Linked Lists| Breadth-First Search | Bit Manipulation|
|Trees, Tries, & Graphs | Depth-First Search | Memory (Stack vs Heap)|
|Stacks & Queues|Binary Search|Recursion|
|Heaps|Merge Sort|Dynamic Programming|
|Vectors/Array Lists| Quick Sort| Big O Time % Space|
|**Hash Tables** (Very Important!)|

- Powers of two table.
- Walking through a problem:
  - Listen: Make sure you understand the problem
  - Example: Draw one large, specific, and not a special case
  - State the solution in brute force
  - Optimize
  - Walk through (Test the solution)
  - Implement: Code it
  - Test

- Interviews are supposed to be difficult.
- Optimize:
  - Look for Bottlenecks, Unnecessary work, and duplicated work.
  - Do it yourself: How do you may solve the problem? (you, not the computer)
  - Simplify and generalize
  - Base case and build
  - Data Structure Brainstorm

- Try to get the *Best Conceivable Runtime*
- You don't need to get every answer right
- Admit if you already know the problem
- Clean code
- Use data structs generously
- Code reuse

### The offer and beyond

- Decline an offer in good terms; don't burn bridges
- Handle rejection with grace
- Evaluate the offer:
  - Salary
  - Perks
  - Location
  - Bonuses and stock options

- Check if it aligns with your career development
- Negotiate the offer
- On the job:
  - Set a timeline
  - Build strong relationships
  - Ask for what you want
  - Keep interviewing

### Arrays and strings

#### Hash table

- We save the *key* and the *value* in the *bucket*
- A good implementation has indexation time complexity of `O(1)`
- If there is a lot of collisions, this may creep up to `O(N)`
- Resizable arrays or ArrayLists provide `O(1)` access, but the resizing takes `O(N)`. This resizing happens so rarely that the amortized time for insertion is `O(1)`.
- Check how strings are copied, or concatenated in your language.

### Linked lists

- Data structure that represents a sequence of nodes. Each node points to the next node in the linked list. In a double linked list, each node points to both the next and previous node.

### Stacks and queues

- Stack: LIFO
- Queue: FIFO

### Trees and graphs

