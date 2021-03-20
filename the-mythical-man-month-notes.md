# Notes on *The Mythical Man-Month*

*The Mythical Man-Month: Essays on Software Engineering* (1975, rev. 1995) by Frederick P. Brooks, Jr. is a classic on software project management, team dynamics, and why “adding manpower to a late software project makes it later.” These notes are a running collection of ideas, quotes, and reflections as I read the book, viewed through the lens of **embedded software development**: long-lifecycle systems, hardware–software co-design, resource constraints, and the particular challenges of planning and coordinating firmware and systems work. Where Brooks’ examples often come from large mainframe-era projects (particularly OS/360), I’ll flag parallels and contrasts with embedded practice today.

---

## 1. The Tar Pit

### The Programming Systems Product
### The Joys and Woes of the Craft

I loved these, and I think we should be reminded of them more often: the Joys for the inspiration and the Woes for the challenge keeping us engaged.

## 2. The Mythical Man-Month

### Optimism
### The Man-Month
### Systems Test

On estimating time for a project:

If **the problem is adaptive**, and tinkering and R&D are expected, the first design will almost certainly be subpar.

Don't overthink the first attempt. Aim to produce a proof of concept quickly and fail fast, so there is time for a proper (adequate) redesign and implementation.

Avoid further redesigns unless requirements cannot be satisfied.

If **the problem is technical**, with a known solution, and the team already has the necessary skills, we should aim to get it right (adequate) the first time.

In that case, the proof-of-concept phase can be combined with design, often making the total time much shorter than for new product development.

In both cases, the design phase may take as long as the coding phase, even for technical problems.

Developers are not mind readers, so time must be spent documenting clearly what we intend to implement.

I agree that test can easily take half of the total time. This includes unit tests, integration tests, and fixing the findings.

Test preparation should run in parallel with implementation.

As Jack Ganssle notes in Embedded Muse, the later we find bugs, the more expensive they get, so developers should be able to test their code as early as possible.

### Gutless Estimating

*"An omelette, promised in two minutes, may appear to be progressing nicely. But when it has not set in two minutes, the customer has two choices—wait or eat it raw."*

*"We need to develop and publicize productivity figures, bug-incidence figures, estimating rules, and so on. The whole profession can only profit from sharing such data."*

### Regenerative Schedule Disaster

*"Adding manpower to a late software project makes it later."*

## 3. The Surgical Team

### The Problem

*"[..] the sheer number of minds to be coordinated affects the cost of the effort, for a major part of the cost is communication and correcting the ill efects of miscomunication (system debugging)."*

*"[...] one wants the system to be built with as few minds as possible."*

### Mills's Proposal

This is something I have been thinking about for a long time, and this book gives it the formality I never sat down before to define. While some of the roles feel anachronic, they immediately spark in my mind what the equivalent would look like today.

The following roles are not necessarily tied to a single person or different people.

**Surgeon (chief programmer):** *"He personally defines the functional and performance specifications, designs the program, codes it, tests it, and writes its documentation. [...]"*

- I disagree with this definition of the role.
- I do think every project needs a technical leader, but this person must not be the main developer.
- They must be capable of writing code and may write proof-of-concept pieces to validate design ideas.
- They are ultimately responsible for the project documentation, but they do not need to write all of it.
- I call this role the Lead.

**Copilot:** *"[...] His main function is to share in the design as a thinker, discussant, and evaluator. [...]"

The more I read this description the more it sounds like how I use AI (Artificial Intelligence) the most:

- *"The surgeon tries ideas on him, but is not bound by his advice."*
- *"He knows all the code intimately."*
- *"He may even write code, but he is not responsible for any part of the code."*

AI needs direction, though. In complex systems this role could be supported by a person as well, someone that can take initiative on behalf of the lead. For the same reason, there is something irreplaceable about connecting with other people (at least today), but this can be filled by periodically meeting with peers in the same role, not necessarily assigned to the same project.

Some other lines like *"represents his team in discussions of function and interface with other teams"* may not apply to AI (today), but sounds like a collateral that can be covered by another person (keep reading the next role, and remember this in chapter 7).

**Administrator:** *"The surgeon is boss, and should have the last word on personnel, raises, space, and so on, but he must spend almost none of his time on these matters. [...]"*

In reality, surgeons are clinical leaders in the OR, but compensation and resource allocation belong to hospital administration. I disagree with Mills’s description of this role.

What I think matters instead is the manager role:
- Keeps track of the schedule and milestones.
- Provides the team with the inputs they need to reach their goals.
- Ensures the output satisfies the customer (final or internal).
- May be a skilled programmer, but the primary responsibility is coordination and facilitation so the team can focus on development.
- Represents the team in discussions with other teams.

Typical responsibilities:
- Staffing, space, and other resources for the project.
- Gathering requirements.
- Setting the schedule.
- Defining milestones.

**Editor:** *"[...] takes the draft or dictated manuscript produced by the surgeon and criticizes it, reworks it, provides it with references and bibliography, [...]."

The book was written in a time when word processing and document management wasn't as ubiquitous, accessible and powerful. I think as a full time role may be anachronic, but I do believe its responsibilities shall lay on the lead, since they are ultimately responsible for the documentation, maybe shared with the developers at the lead's discretion.

**Two secretaries:** *"[...] will handle project correspondence [...]."

Outdated roles for such a small team due to the tools at our disposal today.

**Program clerk:** *"[...] maintaining technical records [...] computer input goes to the clerk, who logs and keys it [...] making all the computer runs visible to all team members and identifying all programs and data as team property [...]."

The responsibilities of this role are performed today by development tools such as version control systems (Git), and task and test management platforms (Jira, Trello, ...).

**Toolsmith:** *"[...] file-editing, text-editing, and interactive debugging services [...] must be available with unquestionably satisfactory response and reliability. [...] responsible for ensuring this adequacy of the basic service and for constructing, maintaining, and upgrading special tools. [...] will often construct specialized utilities [...]."

The *DevOps* is responsible for ensuring the build system satisfies the requirements of the team, deploying and maintaining quality tools such as static analysis, style checkers, automated testing, etc., as well as release management tools, all of them often triggered by the version control system.

It's an essential and underrated role and hard to hire for in the embedded world. It's often covered by the lead and developers, but this keeps them from focusing and spending time on what they do best, often with suboptimal results.

**Tester:** *"an adversary who devises system test cases from the functional specs, and an assistant who devises test data for the day-by-day debugging."

I wouldn't have put it better. Essential. As mentioned before, the sooner code is tested, the better!

DevOps and Tester shall coordinate to integrate testing into the build and CI systems.

**Language Lawyer:* *"[...] can find neat and efficient way to use the language to do difficult obscure, or tricky things. [...]"

This sounds a lot like what Google and AI can do, at least the "difficult" part. If you have to do something obscure or tricky... think again, there may be a better way. Today's computing power and compiler optimizations allow us to focus on readability and maintainability over raw efficiency most of the time.


**

### How It Works

I like where it says ***"the system is product of one mind."*** I 100% agree with this. A great leader will listen to the team, but decisions shall be made by a single person.

Multiple people with equal authority must talk out all their differences which makes projects longer, and arguments tend to come back multiple times.

Even if a leader may make the wrong decision from time to time, it is still better than endless arguments and no clear vision. (Assuming they are properly skilled for the role.)

### Scaling Up

Even when scaling up to multiple "surgical teams", the author insists *"the entire system also must have conceptual integrity, and that requires a system architect to design it all, from the top down."* 

## 4. Aristocracy, Democracy, and System Design

### Conceptual Integrity

The author mentions how some cathedrals show a mix of styles due to the multiple generations of builders. Makes me think of Sagrada Familia, which with over its 140+ years of work (and still going), shows the different styles of multiple architects, and the unique interpretation they made of Gaudi's original vision for the cathedral.

In contrast, the author mentions the integrity of the design of Reims Cathedral, and how eight generations of builders had to sacrify some of their ideas *"so that the whole might be of pure design."*

Proceeds then to make a clear statement for software system design: *"Conceptual integrity is the most important consideration in system design. It is better to have a system omit certain anomalous features and improvements, but to reflect one set of design ideas, than to have one that contains many good but independent and uncoordinated ideas."*

### Achieving Conceptual Integrity

***"Simplicity and straightforwardness** proceed from conceptual integrity."*

### Aristocracy and Democracy

Separation between design and implementation. I have always said software projects cannot be run like a democracy. A great leader will know when to listen to their team, but decisions have to be made efficiently.

### What Does the Implementer Do While Waiting?

Developers can start working as soon as they have an idea of what needs to be implemented. With frequent communication with the architect, they can get work done while the latter finishes the design of the system.

## 5. The Second-System Effect

### Interactive Discipline for the Architect

Important takeaways for the architect:

* When it comes to implementation details, the architect suggests, not dictates.
* Must always be prepared to suggest a way of implementing anything specified, and prepared to accept any other way that meets the objectives.
* Deal quietly and privately in such suggestions.
* Forego credit for suggested improvements.

### Self-Discipline—The Second-System Effect

The author here wisely asserts that the second system is the most dangerous system anyone can make, because the tendency will be to over-design it using all the ideas that didn't make it into the first one.

This second system does not need to be a new one. It could be the willingness of the architect and developers to rewrite the first one.

## 6. Passing the Word

### Written Specifications—the Manual

I disagree with the author, and I believe the architect must in fact describe "what the user cannot see". Maintainabilty and reusability of software are of extreme importance, and I believe they cannot be achieved without a single cohesive vision of the codebase.

### Formal Definitions
### Direct Incorporation
### Exemplars and Executable Specifications

## 7. Why Did the Tower of Babel Fail?

In big software projects, the core failure is communication breakdown, not lack of technical skill. It’s a reminder that “who talks to whom, about what, and how often” is as important as the architecture diagram.

To address the issue, a tree-like organizational structure is proposed. I see each subtree to be compatible with the "surgical team" described in chapter 3.

In this structure, communication links are well defined. Each node communicates with the node above it, nodes at the same level, and nodes below it. There's no "everyone talks to everyone" when it comes to making decisions.

## 8. Calling the Shot

Metrics shown here remind us that *"if you want to go fast, go alone, but if you want to go far, go together."*

## 9. Ten Pounds in a Five-Pound Sack

This section talks about optimizing for shared resources, in this case size and speed. Even though the examples in the book may not be relatable to many today, there are still considerations for the software architect to address in how each module shares the space and CPU available.

For example, when using an RTOS, one shall be mindful of the stack usage of the functions they write. Creating big local buffers is a common pitfall that sometimes doesn't have a straightforward solution.

Thread priorities is another way different modules may share CPU time and interfere with each other. When not in a preemptive system, execution time of each task should be clearly controlled at the system level.

I can think of a few more like access to peripherals, non-volatile storage, etc.

## 10. The Documentary Hypothesis

I like what I call the "docs as code" philosophy. This method stems from the idea that up-to-date quality documentation is just as important as the code itself:

- Docs live in the same location as code, written if possible in the same IDE.
- Docs are part of the code review process.
- Docs shall be written in a plain-text format, easing the review process with common diff tools.
- Use doc generator tools like Sphinx or MkDocs for human-friendly renderings.
- Integrate doc generation into the build system and CI.

## 11. Plan to Throw One Away

For technical problems, where a solution is known, NO.

For adaptive problems, YES. Make faster initial decisions by knowing they are not final.

When breaking down a problem of a given type into smaller ones, some may become adaptive and some technical regardless of the parent problem type. Same as above applies to each subproblem.

When planning to "throw one away" let the developers know what the plan is:
- It may help them make faster decisions knowing they are just prototyping.
- Avoid creating doubt about the quality of their work.

## 12. Sharp Tools

Robust build system, package ecosystem, language support... these are common in any modern programming language.

However, it is common to start coding features and give a lower priority to other essential tools such as having good mocks for testing, a HIL (hardware-in-the-loop) system, robust CI, or a good HAL (hardware abstraction layer).

## 13. The Whole and the Parts

Developers *"will happily invent their way through the gaps and obscurities"*. Clear requirements and programming guidelines are essential to guide them through these.

*"Build plenty of scaffolding"* or, as mentionend before, create good mocks for unit testing. But also have a robust logging system in place, even if logs are not going to be visible in production code, and use system monitoring tools like stack canaries, OS profiler routines, debugging helpers, etc.

*"Control changes"*: today using version control may be a given, but we shall remember every binary shared out with another team shall be traceable back to a specific commit.

## 14. Hatching a Catastrophe

*"How does a project get to be a year late? ... One day at a time"*

- Clear milestones are essential. Well defined, testable.
- I question whether PERT is the right tool today.


## 15. The Other Face

Software documentation usually lacks prose. I believe good documentation shall speak to the reader as if the developer was talking to them, not just listing a set of items they could easily see by themselves in the code.

I disagree with the author in the usefulness of flowcharts.
- Flowcharts describing code statement by statement are useless, yes.
- Flowcharts describing a program flow as abstracted human-readable steps are great.

Write docstrings and be generous with descriptions.

Make docs part of the deliverable and review process.

## 16. No Silver Bullet—Essence and Accident in Software Engineering

### Notes

## 17. "No Silver Bullet" Refined

### Notes
