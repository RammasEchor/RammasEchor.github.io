# Week 8 - Building something from scratch (3)

## 25 May 2021 - 31 May 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## React and the VDOM

We have the DOM (Document Object Model). This is the UI for our application. Every time there is a change in the state of the application, the DOM gets updated to represent that change. But frequently manipulating the DOM affect performance, because you need to draw everything from scratch.

The DOM is represented as a tree data structure; the updated element and it's children have to be re-drawn to update the UI. The more components you have, the more expensive the DOM updates are.

### Virtual DOM

This is a representation of the DOM; every time something in our app is updates, the VDOM gets updated instead of the DOM. Then, the VDOM compares the changes before and after the update, and calculates the best possible method to only make these changes to the DOM. This reduces the cost and time of updating the DOM.

In React, every piece is a component; and each component has a state. When the state of a component changes, React updates the VDOM. The VDOM is compared (diffing) with before and after states, and then React updates only those objects in the real DOM.

## Formik and FastFields

Formik is a javascript library used for creating easy forms. The problem here is that we don't want to create an easy form in our project, we want a form with at least 50 (fifty) fields.

Now, Formik has a component called *FastField*. This *FastField* has a property that allows it to use *shouldComponentUpdate()* from React.

*shouldComponentUpdate()* lets React know that a component's output is not affected by the current change in state or props. This is only used for performance optimization; Since the form we want to use has 50 fields, this is not optional.

Because React renders components when there is a change of state, or at least checks for changes, validating one field means changing the top level *Formik* object that all of those fields also use. This *at least* forces to compare states of the VDOM, and if we want to check every time the value is changed, the form becomes extremely slow.

Using *FastField* works for us because, apart form very few fields, each field does **not** depend one from another. *FastField* updates its component only if the values it uses change, not if the *bag* holding those values (the *Formik* object) changes.
