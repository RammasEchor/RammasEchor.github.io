# Week 2 - Polyglot Programming

## 13 Apr 2021 - 19 Apr 2021

---

This week I learned a lot, my mentors gave some readings, and the videos
and the readings had a lot of topic to absorb.

## MVC

Model View Controller is a software design pattern, commonly used for developing user interfaces that divides
the program logic into three interconnected elements.

- Model: App central structure
- View: Representation of information
-Controller: Accepts the inputs

## Node

Javascript runtime. What I understand is like an API for the OS, so if any PC has Node.js installed,
it can run any program that uses those calls to the system.

## Software design patterns

- Creational Design Patterns: Handling object creational mechanisms
  - Constructor pattern: It is the way new objects are created
  - Factory: Create objects that are similar but not equal
  - Prototype: We take the structure of an object and use it to create new objects
  - Singleton: Only one instance of a class can exist.

- Structural design patterns: Add new functionalities to existing ones
  - Adapter: Interface of one class is translated into another
  - Composite: Used to create tree-like structures
  - Decorator: Adds functionality to a class dynamically
  - Facade: Unified and simpler interface that hides complexity of subsystems
  - Flyweight: Used for efficiency and memory conservation (example: you just instantiate one type of object. If asked for another, you return the existing one)
  - Proxy: Placeholder for access to an object, usually used when the object should not have a responsibility, but we want to add anyways. The proxy calls this
  - functionality if we need it. If not, just calls the object.

## The myth of the genius programmer

The myth here refers to that one guy that is capable of creating a super world-shocking application by himself
and without failing a lot first. This behavior of hiding our errors stems from insecurity; it is hard to admit
mistakes. This is unproductive and doesn't help you learn; you want to cast your code outside and receive feedback.

How to avoid this:

- Lose the ego: Criticism is not evil
- Embrace failure: Everybody fails; you even learn more failing
- Iterate quickly: Fail more and learn more
- Be a small fish: Try to work with someone who knows more than you, so you can learn more.
- Be influenced: Try to listen to others peoples ideas.
- Practice is Key: Failures tend to be smaller over time, and success larger  
- Sometimes you can use technology to solve sociological problems.
  - Requiring a sign in before commenting on code may put you in contact with someone

When working on collaborative projects, people tend to join too late or too early;
if the project is 95% finished, well, there is nothing for them to do. If
the project is al 5%, and a lot of people take interest, almost always the
project stalls because the can't agree on what to do:

- Try to have a little code and objectives for your project before
telling people about it.
- Document failures; Don't make the same mistake twice.

## Perf matters: Some techniques

With the tools
that we have today to measure performance, there is no excuse for your website
to be slow in mobile devices.

This is the performance tuning loop:

- Gather data form users
- Insight: What does this data mean?
- Action: What you can do to increase performance?

There is three (3) pillars of web performance:

- Network:
  - Critical path: Resolution of all the dependencies needed to get pixels on
  screen.
- Rendering: Reduce number of paints.
- Compute: Reduce execution time.
  - Request Animations Frames (rAF): You can push back operations if you
  do a lot of things in a frame, and mis subsequently frames.

## Variable Length Codes: Compressor Head

### Part 1: Why and how it works?

Data compression is used to avoid flooding the network; the information
that flows through the internet every day, every second, could easily
stall the entire internet if not compressed.
Variable length codes changes ASCII and assigns a number of bits according
to the occurring percentage of a character in a stream. If a character appears
a lot of times, the code assigned is very short, like a single zero.
If the probability of a character appearing on a stream is higher than
the other characters, we need less bits per character to represent that stream in average, so the number of bits in the stream lowers if we use
variable length code. You need to make sure to assign shortest codes to
the most frequent symbols.

The encoding part is easy; just put the bit corresponding to each character
in the stream. The decoding part is more difficult; you need to read ahead
because you don't know the current character and thereof the length of bits
to read. What if `A` is `1` but `B` is `10`? Your code needs to assign new
bit codes that *doesn't* start with previous codes.

The Huffman tree generates VLC for a set of chars by means of a binary tree.

### Part 2: The LZ Reign

We use symbol grouping to group (duh) adjacent symbols into a even more compressed stream. We can even group entire strings together. Entropy in this
context means how many bits in average we need to represent the message when
compressed, and so if we group entire strings, the entropy increases (which is not ideal). We want to find the balance between the longest chain and the
minimum entropy. The *LZ* family is the most used algorithm for this task,
and therefore for compression. This algorithm spans a lot of variants, and
it is really the backbone of the modern compression world.

### Part 3: The Markov Model

Instead of assigning bits to characters, we assign symbols to the
transitions between chars (states). This leads into how we need a table
to encode properly, but sending the table over the internet so the
receiver can decode is a lot of overhead. Remember, the thing here is
how to send the smallest stream over the internet. We compress the stream
in such a way that the act of decoding the stream gives us the table for
the remaining of the stream. This needs to strike a balance between the size
of the table and the memory available in the system that is decompressing.

## The Art of Organizational Manipulation

Companies are made from people. You need to learn how to navigate them.

### The Ideal Company

The job of a manager is to remove roadblocks so your team can do work.
You can question things, and you're treated the way you ask to be treated.
You can take risks, and it is even encouraged to.

### The Reality

A bad manager prevents you from taking any risks, failure is terrible, and
ignores low performers. Unrealistic expectations are always there, and they
don't trust you.

### How to navigate

Do the thing that you think needs to be done, and ask for forgiveness later.
Don't try to do it your way al the time; choose your battles. Say "Let's try
this". Try to use favors to your advantage. Build a community with your coworkers.

Ask for something in the form of three (3) bullet points and a call for action.

## Programming Well with Others: Social Skills for Geeks

We study about the latest technologies but not how to talk to people. Don't
hide code; without feedback you'll never learn. The culture of your team is
very important for a good working environment.

Hear the users; make them want to tell you everything. Try not to overwhelm
them with information.

## Developer expertise: How beginners slip with their own feet

We always had tool to write software, but the bug's keep appearing and
appearing. This is not the tools fault; we the people keep creating those bugs. A good team leader asks their team how the training is coming along, and
if it's working. Expertise means that you have enough knowledge and experience to intuitively
know something; this is the objective of learning.

There are five (5) skill stages:

- Novice: Beginner in the field.
- Advanced beginner: Start trying new things
- Competent: Troubleshoot problems
- Proficient: Know how it is all connected
- Expert: Has intuition and knows how to use it

## The Best Programming language

The best programming language is the one that adapts best to your situation.
You may choose a programming language based on:

- Productivity
- Performance
- Reliability
- Portability
- Community
- Familiarity

## Perfection is an unrealistic goal

Good enough is almost always better than perfect. After all, we are all human,
and everyone is different; we learn, progress, fail, work in different ways, and we need to take care of us. Multitasking is not very good if we are doing
critical tasks.

## The power of agile mindset

Pushes us to better ourselves, because agile mindsets grow, learn, challenge
themselves and learn from failure.

## Collaboration, Bonobos and the Brain

Our brain is always making subconscious decisions (like breathing. **HA**,
*now you're breathing manually*). As former primates, we are wired to work
with a reduced (10) group of people. Communication is important in a team,

## Everything is a remix

No idea is new; we learn by copying others do something. By combining ideas,
new creative things can occurs.

## Who do you trust

Collaboration need trust and respect between the ones collaborating,

## Shell tools and scripting

### Assign variables

- `foo=bar`

### Strings

- `` : Literal strings
- "" : Strings that can format values inside

### Reserved var inside bash scripts

- `$0` : Name of the script
- `$1` - `$9` : Args
- `$@` : All args
- `$?` : Last return
- `$$` : PID of script
- `!!` : Last command
- `;`: Separate commands

### Various utilities

- Boolean logic: `&&`,`||`,`true`,`false`
- Output into a variable: `foo=$(command)`
- Pipe into: `< command`
- Conditionals:

      if condition ; do
        something
      if

- Loops:

      for somthing in things; do
        something
      done

-Filename expansion: `?`, `*`

## Conclusions

I learned about the importance of people in the software engineering field,
that we are not alone, and by creating something and receiving feedback about that we can learn more than to try to present something already perfect. I learned about the ways we can thrive in a professional habit,and I learned that I can learn a new programming language quickly.

I learned a little about the tools my mentor are using for their job.

### Back to main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)
