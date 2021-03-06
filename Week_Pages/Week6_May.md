# Week 6 - Building something from scratch (1)

## 11 May 2021 - 17 May 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## Requirements and User Stories

To try to define a full and detailed set of requirements too early in a project is not ideal. The business environment changes, new requirements and opportunities arise, and as the project progresses, the team understands better the needs of the product owner.

### What is a requirement?

Is a service, function, or feature that a user needs. Requirements is an element that must be present to meet the needs of the intended users. For example, "An engine" is not a requirement, is a solution. A requirement would be "A means of propulsion".

### User stories

Is a requirement expressed from the perspective of the end user. This way of expressing requirements has become the most popular way of expressing requirements in Agile because:

- It focuses on the perspective of the user, and how it may be impacted by the solution.
- Defines the requirement in a language that had meaning for that role
- Clarifies the true reason for that requirement
- It helps to define higher level requirements without too much detail.

The format of a user story is as follows:

- **As a ...**: As a user, administrator, etc; a role in the system.
- **I need ...**: Functionality
- **So that ...***: Why do I need that?

For example, *As a Marketing Director, I need to improve customer service so we can retain our customers*.

This user stories may be written on cards: The front of the card has the user stories, and the back of the card has the acceptance criteria.

## Tips from mentors for conversations with clients

- Don't assume; ask and confirm everything (There is no dumb questions)
- Each process has a set of questions that need to be answered:
  - What does it (the process) do?
  - Why?
  - For whom? (Age, skills, etc.)
  - When?
  - How it is done in practice? (How they do it right now?)

- Objectives and client's expectative: MUST KNOW!
- Write everything, or better yet, RECORD THE SESSION.
- What problem may be solved with the system?
- Try to understand the business.
- Be careful not to expand the scope of the project. Time is important.
- COMMUNICATE: Problems, progress, everything.

## Node.js and Express (specifically middleware)

Express is a routing and middleware web framework; an express application is essentially a series of middleware function calls. **Middleware** functions are functions that have access to the *request object*, the *response object*, and the next middleware function. Middleware functions can perform the following tasks:

- Execute any code
- Make changes to the requests and the response objects
- **End the request response cycle**
- Call the next middleware function in the stack

If the current middleware function does not end the request response cycle, it must call the next middleware function; otherwise the request will be left hanging.

This middleware functions are mounted in a `path`. For example, if it mounted in `/public/`, it will handle the methods for that path (given that is has code for that method.)
For example:

        app.get('/public/:id', function (req, res, next) {
          res.send('USER')
        })

Will handle `get` methods for the route `/public/:id`.

## Agile Practices

### Pair programming

Pair programming is pairing with someone and working in the same problem, on the same code, side by side. Is like driving in a rally; there is a driver, and there is a navigator. This roles should often change in the session. The driver has the mouse and keyboard, and the navigator is considering the strategic direction, the next steps and potential problems, and if this is the simplest way to do and how may they improve it. This technique works best with similar level of knowledge and expertise between the pair.

### Continuous integration

This refers to integrating the system components continuously, instead of creating them separately and integrating them at the end. In agile, integration is done early (as in day one) and often (each day new code is integrated to the system). This technique reduces problems, bugs, and risks, and helps deliver a defect free solution.

### Frequent small releases

The act of releasing small, but frequently versions of the system allows us to receive feedback, incorporate that feedback into future releases, and we have a quick system to show the client.

### Definition of done

In Agile teams, this we need to agree in a definition of *done*. Maybe it doesn't need more work to go live, or maybe it doesn't need more work when is documented, but essentially means what the product owner is happy with. The definition of done is important to accurately measure the progress of their work. But a history may have little done checkpoints along the development.

### Burn Up charts

Burn Up charts highlight risks, and helps to track progress agains predictions, and express clear facts and numbers. This puts points into the features, and the project scope is the sum of those points. For each release, we track how many points we reached, and we compare those against the predicted ones needed to deliver the project. If we realize that we are falling behind, we may reach out for a solution; for example, we may reduce the scope of the project, delay the completion date, or increase the resources. There are two types: ones that track the progress daily, and the ones that track the process for each release.

### Test driven development

This is a development process; you write a test for the simplest piece of functionality, then, write just enough code to pass that test. After this is completed, you refactor that code to meet specification. This has the following advantages:

- Ensures quality by focusing on requirements before writing the code
- Keeps the code clear, simple and testable by splitting it logically
- Provides documentation on how the system works
- Suite of regression tests
- Enables rapid changes: Because of the tests, it is easier to understand what the code does, and test that any changes or additions do not break the tests.

### Automated testing

By providing early and continuous feedback, automated testing helps reduce stress and repetitive manual effort. It also enables quick change, because you have regression tests to verify that all is good.

### Planning poker

Activity that integrates all team members, and guides them to share their thoughts, concerns, and contribute. Is as follows: you take one task, and each member rates this task in terms of difficulty. Then, based on the average for this task, they rate other tasks.

### Retrospectives after iterations

After each iteration (release), the team may pause and reflect on how they are working together, how they can improve, and what issues need clarification. This is run by an external facilitator. We may answer some questions:

- What did work well?
- What did not work well?
- What still puzzles me?
- And most importantly, what we learned: What we will do differently in the next iteration?

### Sustainable pace

A sustainable pace maintains the quality and value for the customer. Working at a sustainable pace ensures Agile teams have time to plan, think, rest, and deliver effectively.

### Success sliders

For this technique, the sponsor and product owner need to be involved. We create four sliders for scope, cost, time, and quality of the project. The top of the slider means "This characteristic is fixed". The bottom means "Totally flexible". The kick here is that no slider may be at the same level as another.

After working in the project for a while, the team may realize that some of the scope may not be delivered in time; it is important to communicate with the product owner so there may be a negotiation.

### Prioritization (MoSCoW)

To prioritize a histories, it is important to have the input of the product owner. The MoSCoW technique works as follows:

- We divide the histories in categories:
  - M: Must Have this attribute (Basic functionality)
  - S: Should Have
  - C: Could Have
  - W: Would Have (In the future)

### Showcases

Demonstrate the completed work to product owners, sponsors, and other stakeholders to get feedback, and to course correct if changes need to be made. This meetings show the work that is completed, and the work that is not yet completed. The product owner get a feel of the actual deliverables and the risks of the project.

### Stand ups

This is a recurrent meeting where each team member keeps updated each other with:

- What did I complete yesterday?
- What do I plan to complete today?
- What blockers do I face in completing my work done today?

No more than 15 minutes for a small team. They're a brief daily meeting to share progress update, and any obstacles to that progress.

### Big visible charts

As the name implies, everything related to the work is done in charts visible to anyone clear to see and understand. The written ones work better than the computer generated ones.

### Social Contracts

A social contract is a set of social agreements that each team member must follow to have a happy, sane relationships with one another and the client, and to encourage cooperative behavior. For example, "Be on time" and "Don't dismiss other people opinions". This holds up each team member accountable in a safe way.

### Story cards

I touched this topic above, but I am going to repeat it here as part of the techniques of Agile development.
Agile teams capture requirements from a perspective of the user. This are called user stories, and written on cards. They follow this format:

- **As a ...**: As a user, administrator, etc; a role in the system.
- **I need ...**: Functionality
- **So that ...***: Why do I need that?

A story cards allows you to record a story of what the user wants, and why. This is not all the information, this is just to identify the requirement.

## The black swan: Impact of the highly improbable

The idea presented in the beginning is that we make complex things for commodity; for example, a kindle. BUt with this complexity comes fragility. Anything fragile may eventually break. For example, in nature, things tend to find an equilibrium to survive; this means to be robust, and adaptable. Random events hit you harder when you lack this robustness.

The problem here is that humans, what we create, is not careful crafted over the course of millions of years; what we do is complex and fragile. Organizations, the stock market, bridges, every one of this example is complex and fragile. We are vulnerable because we rely too much in technology, for example, we try to forecast the economy as it was a robust system, but in reality it is incredibly fragile. We end up seeing the tipping points after they happen, but it is extremely difficult to recognize them when they happen.

This fragility also extends to social systems; for example, the way people ungroup in suburbia in the U.S. vs the way people in Russia know each other because a housing crisis. Because this, when suddenly there is no energy, the Russian group has more probabilities to survive. Of course, this all sound like a post-apocalyptic scenario, but it just an example.

My takeaway from this is that we must strive for robustness, for systems that are adaptable and not incredibly dependent on little things. Also, as a government, stop bailing companies in crisis, because this causes the social system to grow even more complex and fragile.

## The optimism bias

This is a tendency to overestimate our likelihood of experiencing good events in our lives and underestimate the likelihood of experiencing bad events, even if we are pessimistic about things in general. Most of us put ourselves above average. This is because if this is true, we may be the ones that will receive the promotion, the raise, the opportunity. Maybe the truth in happiness is to lower our expectations, and to be pleasantly surprised when something good happens.

But happiness is more prevalent in people with high expectations even when failure comes, because it helps us to interpret the failure in a way that encourage to be better. Anticipation makes us happy. Like when people prefer Friday than Sunday.

There is something inside us that prevents us to be more realistic, even when presented with facts that reinforce that reality. But optimism bias also has it's pitfalls, like overestimating what may occur to us.

My takeaway is that is good to hold on this optimism bias, but be prepared for those things you don't believe will happen to you. After all, it doesn't hurt to, right?

## How do we heal medicine?

Medicine right now has a problem: the cost. This has to do with history of medicine; when there was not much a doctor could do, the most appreciated values were self-sufficient and independent, autonomous. Now, it is different. Because now each one of the medical personnel has a specialization, a specific job to do, combined with that culture, generates loner cowboys, instead of what we need: crew pits (like nascar) for each patient. We want the best drugs, the best specialists, the best components, but we often don't think how they work together. What if you build a car from only the best components? Do they work together seamlessly? Working as a pit crew, as a system, has two advantages; you know exactly where in the system the problems are, and you know how to devise a solution for it.

Checklists work great! They provide a guide of what to check before doing something, a little recipe that makes sure you are prepared for the unexpected.

Checklists made for hospitals not only did they make them check everything beforehand; they forced medical personnel to evaluate a different set of values than those old values: Humility, discipline, teamwork. Complexity requires group success.

## Anti fragility

How do we recognize something robust? Well, lets start by recognizing something that is fragile: something that breaks when rattled is fragile, something that remains the same is called robust, and something anti-fragile actually gets better with the rattling, like our body with vaccinations, or work out. Anti-fragility means becoming better through your struggles. There are two things that may break something anti-fragile: enough stress and not enough recovery time.
