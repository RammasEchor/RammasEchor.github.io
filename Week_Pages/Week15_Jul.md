# Week 15 - My Personal Brand (1)

## 13 Jul 2021 - 19 Jul 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## Topics for the interview

### Databases: SQL vs No SQL

*Structured Query Language*  is a language used for communicating commonly with a relational database. Relational databases were developed with a focus on reducing data duplication, as storage was a premium in the 1970's. This type of databases tend to have rigid tabular schemas. A characteristic is that they use *joins* frequently; because they focus on no duplication data, you often join two tables together to get the complete data you need. If the tables are really big, it takes time to sift through each record.

No SQL databases don't really follow this table approach; instead, they focus on speed and flexibility. A No SQL database is any non-relational database. A rule of thumb is that *data that is accessed together should be stored together*. Queries then do not require joins, so they are very fast. This also causes that No SQL databases duplicate data and consume more storage.

Types of No SQL databases:

- Document: Store data in documents similar to JSON objects.
- Key-value: Each item contains keys and values. A value can typically only be retrieved by referencing its key.
- Wide-column: Store data in rows, tables, and dynamic columns; rows are not required to have the same columns.
- Graph: Store data in nodes and edges. The edges are the relationship between nodes.

### Agile

Some key points about the Agile manifesto:

- Early and continuous delivery of valuable software.
- Welcome changing requirements.
- Convey information face-to-face.
- Sustainable development.
- At regular intervals, the team reflect on how to become more effective.

#### SCRUM

SCRUM is not a *methodology*. It replaces a programmed algorithmic approach with a heuristic one. A keyword of SCRUM is *adapt*.

It starts with a *product owner* that represents stakeholders and customers. This product owner manages the backlog; a prioritized list with all the work needed for the product.

Work is done by a self-organizing team during a sprint (a period of one to four weeks). During sprint planning, a sprint backlog based on the sprint goal is created. With the definition of done in place, the team work together to deliver value.

Once a day, the development team meets for 15 minutes to inspect and adapt their progress towards the sprint goal.

The SCRUM master is the person in charge of enacting SCRUM. It helps everyone in understanding SCRUM.

As work is completed, the team inspect and adapt empirically based on progress. This means just to time ourselves based on how much time it have take us to reach this point. The sprint review is a meeting at the end of the sprint with all the stakeholders to review the results. At the end, there is the sprint retrospective, where they evaluate how they have worked, and how to improve.

SCRUM Values:

- Courage: Do the right thing and work on tough problems
- Focus: On the work of the sprint and the goals of the SCRUM Team
- Commitment: Personally commit to achieve the goals of the SCRUM team
- Respect: Each other.
- Openness: Be open about all the work and the challenges.

Some SCRUM concepts:

- Done: Formal description of the state of a backlog item when it meets the quality measures required for the product.
- Increment: When a item from the backlog meets the definition of Done, an Increment is born. It adds value.

#### KANBAN

Kanban is a workflow management method. You annotate in a public board three columns: *Requested*, *In progress*, and *Done*. This board serves as a real time information repository, highlighting bottlenecks within the system, and anything else that might interrupt smooth working practices.

To use the board effectively, you need to understand what it takes to get an item from a request to a deliverable product. Recognizing workflows will set the path to continuous improvement by making well observed and necessary changes.

One of the key features is the *Work in progress limit*. This means that only a number of active items may be in progress at any given time. Switching a team's focus halfway will harm the process, and multitasking generates waste and inefficiency.

## Hackerrank interview preparation

I have been solving some exercises, and It really had helped me to fill in some little gaps in respect of my knowledge about my primary stack (javascript).

## Duck Typing

For example, when you pass an object to a function, you may call a method on this object:

```js
void func(e)    {
    e.quack();
}
```

Javascript has no way of knowing if `e` even has that method, so it just **tries it out with what is given**. The operation does not **formally specify** the requirements that its operands have to meet.

## How to: Work at Google - Example coding/Engineering Interview

- Repeat the problem to the interviewer to make sure you understood it right. Inputs, outputs, anything: there is no dumb questions.
- Really, repeat the questions, and ask even the most simple things. Maybe you assumed that you may not repeat numbers, and maybe it is possible and you just shoot yourself in the foot.
- State the brute force solution first.
- Practice coding in paper to ease yourself into writing code in the whiteboard.
- Talk about your thought process. Talk, and explain your ideas, explain what you are doing, everything.
- Outline the solution out loud before start coding.
- Test the code in real time.
- Think about edge cases.

## Google Coding Interview With A Competitive Programmer

This is an example of how not to loose your cool while presented with problems that you cannot solve. Try to do your best! Remember that for very hard problems, the interviewer is evaluating how you behave in the face of challenges, not looking for a perfect solution.

## Conclusions

This week I reviewed a lot of topics I had forgotten, and learned some new things! The first time I was those topics I hadn't have the experience I have now to understand them, so I saw them in a new light. Reading about SCRUM and KANBAN helped understand better the motivations behind adopting each workflow, and how self-organization is essential for doing a good job. Interviewers grade your communication and problem solving skills as much, or more, than knowledge.
