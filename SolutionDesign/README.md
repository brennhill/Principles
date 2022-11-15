# Solution Design
When companies build a new system, they often "dive in" and just start building.
It's actually MUCH more efficient to spend a little time doing some up-front design work.
However, when teams do upfront design, they rarely consider all the aspects they should.

This document is meant to be a helpful checklist of things to consider and document before you "dive in" to implementation.
If you do this, you'll almost certainly save a lot of time in the end.

**The following template should be used when planning new systems or large upgrades.**

# Design Writeup Template
## 1 System Goals
What are you trying to achieve? How do you know you are successful
## 2 How do we measure customer success
What are the people using this system/API trying to achieve?  Have you considered their needs?  How can we be sure our solution is easy for them to use and effective before we spend time building it?
## 3 Estimated system ROI
Building things costs money and time. Where do we get our ROI?
- Improved TTM? How much?
- Can we sell this?
- Improved customer retention?
- Internal ease of use/efficiency gains?

The point is to document the intended value creation and expected costs *up front*.
## 4 Solutions considered
Don't consider just one solution. Consider a few and look at the ROI.  Sometimes cheap, fast, and dirty is great.  Sometimes it's a horrible idea.

Choose carefully.
## 5 Solution Selected + Rationale
Document why you chose the solution. This is not to CYA it's for retros and to force honesty.

## 6 C4 architecture + other helpful diagrams
Use the C4 architecture to document your overall solution architecture.  
You can add additional diagrams if you really feel it helps.

I also suggest sequence diagrams as well (see item 9)

## 7 Public API or Behavior
If this is a backend system, link out to a proposed API *before building*.
Is this easy to use? What about authorization (see below)

## 8 Authentication and authorization, rate limiting, system protection
How is this system secured?  
How will you protect against abuse?

Don't write "we won't".

## 9 Key Sequence Diagrams
For key flows, create some sequence diagrams. These are helpful for development, QA, debugging, etc.

## 10 Data models
Larger companies often suffer from an explosion of similar but not the same data models.
For interoperability's sake, don't re-invent the wheel if possible.

#### a. New Data models
If you have a new data model, what is it.

#### b. Re-used data models
What other data models are you using from across the company?
Were there competing standards? Better to call it out here.

#### c. ISO standards utilized, considered, rejected
Did you even look to see if there's a documented standard? Bet you didn't.

Go check. Revise the above as needed.  Please use the standards for everyone's sanity unless you have a *super good reason* not to.
Lazyness is not a super good reason.

## 11 How to verify correct behavior?
Again, think about how you'll test your system *as a whole* to verify correctness.
Nobody likes 3am calls because something blew up.  Shift left, starting now.

## 12 List of Key external systems and dependencies
External means everything that isn't this system, inside your company or not.
### Strategy to minimize and encapsulate.
Every dependency is a potential bug when they upgrade, change something, go down, etc.
Have a plan for how you'll deal with external dependencies and services going haywire.

Make reviewing that plan part of code review.

## 13 Key consequences
Every system has consequences. Don't lie to yourself.
### a. Vendor or system lock in?
### b. Technology specific behavior
### c. Unique or rare skillsets needed for development or support
### d. Regulations? Others?
## 14 Anticipated Evolution considered
The only constant is change.  If your system works, do you expect a lot of scale?
I've only listed scale here but you could consider ongoing regulations (privacy, financial, etc).
A little thinking here can go a long long way.
### Scale
#### Plan for 10x current load
#### Plan for 100x current load

## 15 Service lifecycle management
The only constant is change. You will upgrade, deprecate, alter, or have others depend on your service.
How will you handle this thing AFTER it gets deployed.  Thinking this through NOW makes everyone's life easier LATER.
### a. plan for versioning?
### b. How will we flag service upgrades, deprecation, etc, to end users?
### c. Intended SLO/SLA for service
### d. Intended behavior outside of SLO
### e. Replacement lead time
### f. Plan for operations and monitoring
### g. Key contacts