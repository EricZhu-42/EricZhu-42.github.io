---
title: The Pragmatic Programmer, Chapter 1&2
date: 2019-02-21
tags: [Reading Notes]
---
# The Pragmatic Programmer
## 1. A Pragmatic Philosophy
### The Cat Ate My Source Code
* Take responsibilities for yourself and your actions, being honest and direct
* Provide options instead of lame excuses
* Be ready for others' errors
* Admit that you need help when necessary

### Software Entropy
* **Don't live with broken windows, fix them as soon as they are discovered**
* If time is insufficient, leave a comment at least
* Neglect accelerates the rot faster than any other factor

<!-- more --> 

### Stone Soup and Boiled Frogs
* **Be a catalyst for change. Show them a glimpse of the future and you'll get them to rally around**
* Remember the big picture. Constantly review what's happening around you.

### Good-Enough Software
* Write software that's good enough for users, future maintainers, your own peace of mind. 
* **The scope and quality of the system should be part of the early requirement**
* Great software today is often preferable to perfect software tomorrow. They can NEVER be perfect.

### Your Knowledge Portfolio
* **Invest in your knowledge portfolio regularly. Learn at least one new language every year**
* Diversity matters. Learn technical and non-technical knowledge.
* Balance between high-risk technologies and low-risk ones
* Be ready to learn emerging technology before it gets popular.  
* Stay current and get wired

### Communicate! 
* Know what you want to say
* **It's both WHAY YOU SAY and THE WAY YOU SAY IT**
* Know the audience

## 2. A Pragmatic Approach
### The  Evils of Duplication
* DRY principle: Don't Repeat Yourself
* If duplication is unavoidable, make the process active by using automatic methods
* Make comments independent from detailed code
* Use header files to document interface issues
* **Normalize data by using accessor functions to read/write and localizing impacts**
* Shortcuts make for longer delays. Avoid impatient Duplication
* Encourage active and frequent communications between developers. Appoint a member as the project librarian if possible

### Orthogonality
* Orthogonality signifies independence and decoupling.
* **Eliminate effects between unrelated things**
* Orthogonality increases productivity and reduces risk
* An orthogonal team is more efficient

### Reversibility
* There are no final decisions. ALL decisions are temporary in real circumstances
* Make architecture flexible to get ready for a change in platform, language, design pattern, etc

### Tracer Bullets
* Use tracer bullets like printing variables to test components of the system
* **Tracer code is not disposable because a project is never fininshed**
* Prototype generates disposable code while tracer code is lean but complete. They are different development styles

### Prototypes and Post-it Notes
* Prototypes are designed to test some specific aspects of a project
* Prototypes can be built on different materials, like workflow or application logic for dynamic things and whiteboard for user interface
* Prototypes are designed to answer just a few questions. They are much cheaper and faster to develop
* **The value of prototype lies both in the code produced and the lesson learned**
* Things can be ignored when building a prototype
    * Correctness of data
    * Completeness of function
    * Robustness (Error checking)
    * Pretty and standard style
* When prototype is inclined to be based on, turn to tracer bullet approach instead

### Domain Languages
* **Program close to the problem domain**
* Use mini-language to help create artifacts (related to metaprogramming)

### Estimating
* Estimate to avoid surprises and optimize efficiently 
* Iterate the schedule with the code
* Make time estimate cautiously

Link: [Mindmap (as PDF format)](/files/notes/The_Pragmatic_Programmer_chapter_1&2.pdf)







