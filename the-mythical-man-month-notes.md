# Notes on *The Mythical Man-Month* (Embedded Lens)

*The Mythical Man-Month: Essays on Software Engineering* (1975, rev. 1995) by Frederick P. Brooks, Jr. is a classic on software project management, team dynamics, and why “adding manpower to a late software project makes it later.” These notes are a running collection of ideas, quotes, and reflections as I read the book, viewed through the lens of **embedded software development**: long-lifecycle systems, hardware–software co-design, resource constraints, and the particular challenges of planning and coordinating firmware and systems work. Where Brooks’ examples often come from large mainframe-era projects (e.g., OS/360), I’ll flag parallels and contrasts with embedded practice today.

---

## 1. The Tar Pit

### The Programming Systems Product
### The Joys and Woes of the Craft

I loved these, and I think we should be reminded of them more often: the Joys for the inspiration and the Woes for the engaging challenge.

## 2. The Mythical Man-Month

### Optimism
### The Man-Month
### Systems Test

On estimating time for a project:

Assuming the problem to solve is adaptive, were some tinkering and R&D is expected, the first design will almost certainly turn to be subpar. Don't get hung up overthinking it. The goal must be to spin a proof of concept promptly and to fail fast, allowing more time for a proper (and by proper I mean adequate) redesign and implementation. No further redesigns unless requirements cannot be satisfied.

If the problem is inherently technical and the team already has the skills to take it to completion, we shall aim at getting it right (read adequate) the first time, and the proof-of-concept phase can be combined with the design, with a total time a lot shorter than for new product development.

The design phase in both cases could very well take as long as the coding phase, even for technical problems. Implementers aren't mind readers, and time shall be spent documenting clearly what we ought to implement.

I agree that in every case the test phase could take half of the time. This includes unit testing, integration testing, and the time spent fixing findings. Test perparation shall run in parallel to the implementation phase. Jack Ganssle mentioned in his newsletter Embedded Muse how bugs get a lot more expensive the later we find them. The earlier the implementers can test their code, the better.

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

This is something I have been thinking about for a long time, and this book gives it the formality I never sat down before to define. While some of the roles feel anachronic, they immediately spark in my mind what the equivalent would look like today. However, this team organization is not my favorite and the book describes one that I like better in chapter 7 *"Why Did the Tower of Babel Fail?"*.

**Surgeon (chief programmer):** *"He personally defines the functional and performance specifications, designs the program, codes it, tests it, and writes its documentation. [...]"*

While I do not agree with this definition of the role, I do think it is important to have a role for a technical leader in every project. This person's role, however, should not be that of the developer. They should be capable of writing code if needed and highly skilled on it, and may write some proof-of-concept pieces to test design ideas, but should refrain themselves from doing most of the programming work. They are also ultimately reponsible for the project documentation, but do not need to write all of it.

I call this the **Lead**, and depending on size and complexity they may support more than one project at a time, but no more than 2 or 3.

**Copilot:** *"[...] His main function is to share in the design as a thinker, discussant, and evaluator. [...]"

The more I read this description the more it sounds like how I use AI (Artificial Intelligence) the most:

- *"The surgeon tries ideas on him, but is not bound by his advice."*
- *"He knows all the code intimately."*
- *"He may even write code, but he is not responsible for any part of the code."*

AI needs direction, though. In complex scenarios, this role could be supported by a person as well, someone that can take initiative on behalf of the lead. For the same reason, there is something irreplaceable about connecting with other humans (at least today), but this can be filled by periodically meeting with peers in the same role and not assigned to the project.

Some other lines like *"represents his team in discussions of function and interface with other teams"* may not apply to AI (today), but sounds like a collateral that can be covered by another person (keep reading).

**Administrator:** *"The surgeon is boss, and should have the last word on personel, raises, space, and so on, but he must spend almost none of his time on these matters. [...]"*

The reality is that, while surgeons are the clinical leaders in the operating room, administrative decisions regarding compensation and resource allocation are typically handled by hospital administration. So I do not agree with Mills' description of this role.

However, I do agree on the relevance of the *manager*, whose role is to provide the team with the inputs they need to fulfill their goals, and ensure the output satisfies the customer (final or internal). While the manager is the ultimate boss, and may very well be a skill programmer, their primary role is to coordinate and facilitate the work so the team can focus on development. I believe this is the role that should also represent the team in discussions with other teams.

- Staffing, space, etc. for the project.
- Set the schedule.
- Define what needs to be done, gather requirements, but not necessarily to write them (more on this later).

A manager may serve more than one team at a time, and/or be supported by a project manager.

**Editor:** *"[...] takes the draft or dictated manuscript produced by the surgeon and criticizes it, reworks it, provides it with references and bibliography, [...]."

The book was written in a time when word processing and document management wasn't as ubiqutuous, accessible and powerful. I think as a full time role may be anachronic, but I do believe its responsibilities shall lay on the lead, since they are ultimately responsible for the documentation, maybe shared with the developers at the lead's discretion.

**Two secretaries:** *"[...] will handle project correspondence [...]."

Extinct roles for such a small team due to the tools at our disposal today.

**Program clerk:** *"[...] maintaining technical records [...] computer input goes to the clerk, who logs and keys it [...] making all the computer runs visible to all team members and identifying all programs and data as team property [...]."

The responsibilities of this role are performed today by development tools such as version control systems (Git), and task and test management platforms (Jira, Trello, ...).

**Toolsmith:** *"[...] file-editing, text-editing, and interactive debugging services [...] must be available with unquestionably satisfactory response and reliability. [...] responsible for ensuring this adequacy ofd the basic service and for constructing, maintaining, and upgrading special tools. [... will often construct specialized utilities [...]."

I believe this role is so important and its importance often underrated. The *DevOps* is responsible for ensuring the build system satisfies the requirements of the team, deploying and maintaining quality tools such as static analysis, style checkers, automated testing, etc., as well as release management tools, all of them often triggered by the version control system (GitHub Actions, Bitbucket Pipelines, etc.).

It's an essential and underrated role and hard to hire for in the embedded world. It's often covered by the lead and developers, but this keeps them from focusing and spending time on what they do best, often with mediocre results.

**Tester:** *"an adversary who devises system test cases from the functional specs, and an assistant who devises test data for the day-by-day debugging."

I wouldn't have put it better. Essential. As mentioned before, the sooner code is tested, the better!

**Language Lawyer:* *"[...] can find neat and efficient way to use the language to do difficult obscure, or tricky things. [...]"

This sounds a lot like what Google and AI can do. However, difficult, yes, but if you have to do something obscure or tricky... think again, there may be a better way. Today's computing power allows us to focus on readability and maintainability over efficiency in most settings.



**

### How It Works
### Scaling Up

## 4. Aristocracy, Democracy, and System Design

### Conceptual Integrity
### Achieving Conceptual Integrity
### Aristocracy and Democracy
### What Does the Implementer Do While Waiting?

## 5. The Second-System Effect

### Interactive Discipline for the Architect
### Self-Discipline—The Second-System Effect

## 6. Passing the Word

### Written Specifications—the Manual
### Formal Definitions
### Direct Incorporation
### Exemplars and Executable Specifications

## 7. Why Did the Tower of Babel Fail?

### Notes

## 8. Calling the Shot

### Notes

## 9. Ten Pounds in a Five-Pound Sack

### Notes

## 10. The Documentary Hypothesis

### Notes

## 11. Plan to Throw One Away

### Notes

## 12. Sharp Tools

### Notes

## 13. The Whole and the Parts

### Notes

## 14. Hatching a Catastrophe

### Notes

## 15. The Other Face

### Notes

## 16. No Silver Bullet—Essence and Accident in Software Engineering

### Notes

## 17. "No Silver Bullet" Refined

### Notes
