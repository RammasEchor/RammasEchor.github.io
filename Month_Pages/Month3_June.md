# Month 3 - Open source / Read others people's code

## 08 Jun 2021 - 12 Jul 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## Status of my open source contributions as today 12-Jul-2020

### 1. npm contribution

#### Status

##### Closed, not merged

[Context](/Week_Pages/Week11_Jun.md)

The issue was marked as open and with some thumbs up from the maintainers, but the direct fix was deemed a regression. From the feedback I understood why what I did was not acceptable, and why to solve this issue completely the project would need a rework in the way it outputs errors.

[Link to issue](https://github.com/npm/cli/issues/2740)

[Link to pull request](https://github.com/npm/cli/pull/3437)

### 2. mongodb Go driver contribution

#### Status

##### Suspended, waiting for a response

[Context](../Week_Pages/Week12_Jun.md)

I tried to ask some questions about my approach to the issue, but no one responded. I hit a wall when refactoring, and I could not move forward without some answers.

[Link to issue](https://jira.mongodb.org/browse/GODRIVER-1953)

[Link to draft pull request](https://github.com/mongodb/mongo-go-driver/pull/692)

### 3. openseadragon contribution

#### Status

##### Merged

[Context](../Week_Pages/Week13_Jun.md)

This issue stems from callbacks trying to work with elements for which it's own callbacks haven't told about the change yet. Since those elements haven't received the changes, they output an error. Most of my work involved recreating the error in a consistent manner, and then I just changed the way callbacks were processed.

My approach to the issue was deemed acceptable by the maintainers, and my work was merged.

[Link to issue](https://github.com/openseadragon/openseadragon/issues/1843)

[Link to pull request](https://github.com/openseadragon/openseadragon/pull/2005)

### 4. openseadragon contribution

#### Status

##### Pull Request Created

[Context](../Week_Pages/Week14_Jul.md)

This feature has already code that does what is needed; it works if I just copy and and paste the code inside a switch, but this is a bad code practice: better to avoid code duplication. I read about closures and the object this in javascript to help me understand a way to solve it; also, about the bind() function. The maintainers asked me to fix something related to the issue in my pull request, so that's what I have been doing

[Link to issue](https://github.com/openseadragon/openseadragon/issues/1718)

[Link to pull request](https://github.com/openseadragon/openseadragon/pull/2007)

### 5. openseadragon contribution

#### Status

##### Pull Request Created

[Context](../Week_Pages/Week14_Jul.md)

You may notice that the issue and the pull request are in different open source projects. This is because the root cause was not in the original project, but in a library that uses the original project. This fix seemed simple enough, but I did have to dive fairly deep into the code to realize that it was an issue of the other project

[Link to issue](https://github.com/openseadragon/openseadragon/issues/1831)

[Link to pull request](https://github.com/cuberis/openseadragon-curtain-sync/pull/9)

## Conclusions

This phase I learned a lot about big codebases, how to navigate them, how to read them, and I got a feel about the environments used in big projects. I would have loved to work on more projects with my secondary tech stack, but there were very little of them, and was difficult for me to find open source projects with good communication. Nonetheless, this phase helped me in developing communication skills with people I don't know beforehand, and more importantly, receiving feedback as *feedback*; not as criticism, or as an insult, but as advice. I really liked this phase, because it makes you put yourself into the spotlight. It really helps with removing those feelings of *what if they laugh at me* or *what if they think I'm stupid*, and makes you see yourself as what you are: a beginner that may make mistakes, but that learns from them.
