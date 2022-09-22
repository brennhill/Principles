# Brenn’s Software Engineering Principles

# Preamble

This is a collection of principles, guidelines, and suggestions I’ve compiled as my default “Tech standards playbook”. It’s oriented to standard webapps, SaaS applications, and similar. If you are doing a hardware startup or something that’s deep in security or something outside the area for which this is intended then parts of this document may not apply.

  

I write this in part to help out the large number of non-tech founders I’ve met who are trying to hire a CTO or VPoE and don’t know how to choose or how to evaluate them. This gives them something they can use as a sounding board/conversation starter.

  

That said, I offer this for free, you get what you pay for.

# Culture Standards

1.  Communicate early and often.
    
2.  Don’t assume others are on the same page. Over-communicate and explain.
    
3.  Self-organize, collaborate, be proactive in resolving your own blockers and helping others to do so. All for one, one for all. There is no such thing as “not my job” in a startup. In a bigger company, if you can’t solve a problem then help find the person who can.
    
4.  If you don’t know what to do, feel stuck, are out of tasks, or are worried/nervous about anything regarding the tech, the product, or company, please speak up.  
      
    

## Set and Communicate Shared Values

Let’s start with defining why we need to establish a set of shared values. Each industry and team faces different risks and each company is in a different part of its lifecycle. The values that need to be maintained in a highly regulated or life and death industry (like healthcare) are very different from what you should value in a fast-paced internet startup where fortunes may be won or lost but lives will not be.

  

These values are for those sorts of companies, which are the locations I generally work (along with most others in the internet industry). Highly regulated would be different (more testing, more formal validation, and scheduled penetration tests, etc).

### My Personal Values as a Dev and Team Lead

  

-   Be experimental, fail fast, fail cheap. The best teacher is the market. It’s better to put something out there and get feedback than try and polish something to perfection against an imaginary customer.  
      
    
-   Focus on learning. Everyone on the team should not only be growing as an engineer, but also in their understanding of the whole business.  
      
    
-   Become cross-functional. We do not want to be siloed or too specialist. Being excellent at one thing (FE/React, BE, etc) is fantastic, don’t be afraid to mentor others or take on things outside your specialty. We never know what challenges we will face so we need to prepare ourselves for anything. We do that by growing our skills in as many aspects of technology as we can.  
      
    
-   Good developers ship. In the end, shipping working software is the most important thing. We hold ourselves to the goal of always improving how fast we can get our code to production while maintaining acceptable code quality.
    

  

### My Personal Values as a Tech Executive

-   People come first. Almost every problem I’ve seen at the organizational scale is a people issue first, a process issue second, and a tech issue third if at all. Get the right people in the right place to succeed, and the tech will usually be taken care of. Take care of your people and let them handle the rest.  
      
    
-   Build Trust, Earn Trust. Trust is the foundation of relationships. Relationships are necessary between people to be effective. People come first. Ergo, communicate honestly at all times. Mistakes are OK if owned and learned from. Say what you know, what you think, what you believe, and be willing to admit what you don’t know. Be direct in communications. No gossip, no passive aggressive BS. I hold myself to this standard and must be a role model for the team.
    

  

-   Align incentives, Use the whole person. Organizations hire engineers to be smart, creative, problem solvers. Then they shield them from decisions which require smart and creative solutions just because those decisions don’t involve writing code. People are more than cogs, use and develop the full power of the human capability for creative thought. Align the development of the people with the development and success of the organization.
    

  

-   Do the big things right, don’t sweat small stuff. The world is ruled by Pareto's principle: 20% of what we do drives 80% of the results. Focus on getting that 20% done 120%, then work on the rest. Corollary: Anything that isn't really important probably shouldn’t be done at all. Typically, this means most meetings. Most meetings should be emails, most emails should be quick messages, and half the time even those get resolved before anyone needs to get involved. Killing waste is usually more impactful than trying to optimize.
    

  

Those are for me personally as a tech leader. You can/should have your own and be able to articulate them and defend why they matter to you and your company.

# Writing Scalable Software: The 12 Factor App Model

The 12 factor app model (12factor.net) is an established approach to writing software that scales. We should obey these principles whenever possible. When not possible, we should take a pause and ask ourselves what we are doing wrong, fix it, and go back to the 12 factor model. While following the 12 factor model does not guarantee software will scale, breaking these rules is a good way to guarantee that it will not scale. So follow the rules ;)

## [I. Codebase](https://12factor.net/codebase)

One codebase tracked in revision control, many deploys.

## [II. Dependencies](https://12factor.net/dependencies)

Explicitly declare and isolate dependencies

## [III. Config](https://12factor.net/config)

Store config in the environment using environment variables.

## [IV. Backing services](https://12factor.net/backing-services)

Treat backing services as attached resources

## [V. Build, release, run](https://12factor.net/build-release-run)

Strictly separate build and run stages

## [VI. Processes](https://12factor.net/processes)

Execute the app as one or more stateless processes

## [VII. Port binding](https://12factor.net/port-binding)

Export services via port binding

## [VIII. Concurrency](https://12factor.net/concurrency)

Scale out via the process model

## [IX. Disposability](https://12factor.net/disposability)

Maximize robustness with fast startup and graceful shutdown

## [X. Dev/prod parity](https://12factor.net/dev-prod-parity)

Keep development, staging, and production as similar as possible

## [XI. Logs](https://12factor.net/logs)

Treat logs as event streams

## [XII. Admin processes](https://12factor.net/admin-processes)

Run admin/management tasks as one-off processes

## Writing Measurable Software

Note: Once you have a working MVP, you want to consider an additional 7 factors

[https://www.ibm.com/cloud/blog/7-missing-factors-from-12-factor-applications](https://www.ibm.com/cloud/blog/7-missing-factors-from-12-factor-applications)

  
  

# API Principles

## I. Understand your Data in APIs.

What I’m about to write is a VERY general suggestion, but I’ve seen this be applicable so often that I’m just gonna throw it out there. It’s a great place to start thinking about data and fixing this alone has resulted in 10x application performance when properly using an edge cache as I suggest below.

  

### Data Segmentation

Data should be segmented into one of three types: Static, Dynamic, and Personal.

  

API CALLS SHOULD INCLUDE ONLY ONE TYPE OF DATA.

  

-   STATIC data is data that once created rarely or never changes or changes rarely (daily, every few hours). Example: product photos and description.  
      
    
-   DYNAMIC data is data that is known to change regularly - every few seconds or every minute but the value is global. Typically fetched from a database table. Example: Available inventory, shipping time updates, etc.
    

  

-   PERSONAL data is data that may or may not change regularly but is specific and unique (and likely private) to a user. Example: shopping carts, user profile, order histories.
    

## II. Design for Caching

Data should be cached appropriately to its type (see below).

  

-   Static data should be cached either indefinitely or with a very long (hours) TTL. Ideally, the content will only update with a PUSH from source systems telling the cache that new data is available.  
      
    
-   Dynamic data should be updated at a known frequency (e.g. 10s). Thought should be given to the nature of the data whether a persistent (update on notification) or transient (expires after a timer) cache should be set.  
      
    
-   Unique/Personal data should be cached at the client side whenever possible. Exceptions to this rule should be for security or legal reasons, in which case data should be stored in whichever means is most appropriate. For instance, a user logs in and their profile loads. Cache in the browser or mobile app as long as possible to avoid future API calls.
    

  

For Static and Dynamic data, Edge Caching (e.g CDN) should be implemented transparently at the infrastructure level. Ideally neither client nor server is aware of the existence of the cache. This typically means using a CDN or API gateway/reverse-proxy service that encapsulates this behavior. We do this because multiple levels of cache can create all sorts of race conditions and invalidation bugs that are hard to debug or reproduce. By keeping things at the infrastructure level, it’s typically easy to tune the system. Only if this layer is not enough should additional cache layers be considered.

  

Once you are at the scale where a well placed and utilized edge cache is enough, you should have a substantial tech team and they can make solid decisions going forward.

# Coding Standards

## I. Build In Security

Don’t create your own security. Be aware and watchful for violations of the OWASP top 10:

-   [https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
    
-   Never trust data from client applications - internal and external. All data should be validated/escaped from all sources. If performance is an issue, increase the strictness of the requirements and demand clear client requests. For any web rendering: Use a template library that auto-escapes data values.
    
-   Never execute raw queries, always use an ORM package that has been heavily tested and validated to execute all queries.
    
-   Production databases should be secured with passwords, IP locked, hidden behind a VPC, and minimal permissions given for app behavior.
    
-   Backups of production data should be done daily and tested daily. Backups must be outside of the running production system. A good approach is to backup and load into a staging environment (which also needs to be properly secured). This ensures a hot backup and testing against real data.
    
-   Use scanning tools like [Sonar](https://www.sonarqube.org/) and [Snyk](https://snyk.io/) to auto-detect some security risks. Sonar is free.
    

## II. Code Quality

### Write Clean Code

An enormous amount has been written about clean code. It will not be rehashed in full here. We just want to focus on a few key principles:

  

-   Automated Testability
    
-   Encapsulation of complexity (SOLID, FP principles, etc etc)
    
-   Code is for people just as much as it is for computers.
    

  

Have engineers read the following articles:

-   [https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/)
    
-   [https://blog.cleancoder.com/uncle-bob/2017/03/03/TDD-Harms-Architecture.html](https://blog.cleancoder.com/uncle-bob/2017/03/03/TDD-Harms-Architecture.html)
    
-   [https://chris.beams.io/posts/git-commit/](https://chris.beams.io/posts/git-commit/)
    
-   [https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)
    

  

You don’t have to agree with everything in these articles. The goal is to expand awareness in the space.  
  

Key principles to maintain code quality:  
  

-   Use a code style linter. Every language has a tool or tools for this. Pick popular defaults if you aren’t sure about anything.
    
-   Use an automatic style formatter if there isn’t a language-standard style (e.g. Golang). Use a set of popular common defaults across all projects in that language. Don’t argue over this, a clear choice that you commit to is better than no choice and wasting time.
    
-   Automated code coverage percentage for tests should always go up. The goal should generally be 90% code coverage. This is not just “unit test” coverage, but overall code coverage by automated tools. There are ways for devs to cheat this. Make sure you know how to check for this or find someone who can.
    
-   Integration tests and e2e tests and BDD style tests provide more business value and test more code per line than unit tests. Use those tests as a first choice and use unit tests for tricky logic that needs to have all edge cases carefully covered. NOTE: this is not the case for industries like fintech which have huge penalties/problems with even “minor” misbehavior. In these cases you want extensive coverage at every level possible to ensure maximum robustness and defense against accidental/unexpected behavior.  
      
    

If you just HAVE to rush it, low-level tests should include at least one “happy path” test and one “error/unhappy path” test, to force the definition of failure not just success.

### Use the updated “[Joel Test](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/)” as a gut check

How do you score on this list. You want to be 12/12. If you are below 9/12, you probably have an issue.

  

1.  Do you use source control? (Should be using Git and github, gitlab, etc)
    
2.  Can you make a build in one step? (This should be locally and CI/CD)
    
3.  Do you make builds at least daily, preferably many times a day?
    
4.  Do you have a bug database/tracking?
    
5.  Do you fix bugs before writing new code?
    
6.  Do you have an up-to-date schedule?
    
7.  Do you have a spec?
    
8.  Do programmers have quiet working conditions?
    
9.  Do you use the best tools money can buy? (Good laptops, Cloud tech)
    
10.  Do you have testers?
    
11.  Do new candidates write code during their interview?
    
12.  Do you do hallway usability testing?
    

## III. Code reviews

All code should have at least one peer review before being merged to the master branch. Put some effort into this as a part of culture. Good code reviews improve quality, help onboarding, and lower the overall technical risk of the team and company.

## IV. Architecture

Most typical web applications have a similar web architecture, consisting of a CDN, a set of UI rendering servers (optional), API servers, file storage, a database of some type, and a set of admin/worker/scheduled jobs.

  

A good primer on system architecture concepts:

[https://github.com/donnemartin/system-design-primer](https://github.com/donnemartin/system-design-primer)

  

VideoBlocks produced a relatively generic web architecture diagram that I like:

[https://engineering.videoblocks.com/web-architecture-101-a3224e126947](https://engineering.videoblocks.com/web-architecture-101-a3224e126947)

  

This is a good baseline reference of what a production web architecture looks like. If you are building a software as a service application, the architecture should be at least somewhat similar in most cases.

Last but not least, this is an excellent (but long) article on large-scale web services:

[https://medium.com/swlh/recipe-to-build-large-scale-web-apps-f38b20ab727f](https://medium.com/swlh/recipe-to-build-large-scale-web-apps-f38b20ab727f)

### REST APIs

By default, use REST APIs. They are well understood and supported and it’s easier to find people who are skilled at rest vs GraphQL vs gRPC.

  

When architecting features, give a look to the REST Cookbook:

[http://restcookbook.com/](http://restcookbook.com/)

  

## V. Code Deployment and QA Process

### Current process

As code passes from development to production, it will pass the following stages:

1.  Local Computer - a developer works on a ticket on their machine.
    
2.  CI/Dev server - A developer can push to a dev branch to check their code running in the cloud and verify interaction with any google/cloud services.
    
3.  Staging - After verification by the developer that their code works on the CI server, they should merge request to master for code review. After code review, the merged code will deploy to staging for manual and AQA.
    
4.  Production. By tag on master branch only.
    

  

The code architecture should allow an engineer to run the whole system (or at least enough of the whole system to validate behavior) on their own machine. Developer machines should either be very close to production (e.g. all running linux) or a diversity of OS’s and machine builds to help find tricky code issues through sheer variance. The best choice will depend on your problem domain.

  

NOTE: This is changing since I wrote this, with cloud-native development options becoming available but I’ve not used them enough to recommend anything at this point.

  

### QA Options

The quality of your software is a combo of how well written the code is and how well tested it is. At scale, QA is always important, and because of the effort involved, we want as much of that QA to be automated as possible. There’s a difference between “feature works” and “feature works well”. If you use humans for QA, they need to be opinionated about what counts as “feature works well” and help police usability.

  

Testing you want to do:

-   Unit tests (every language has a unit testing library)
    
-   API tests ([https://github.com/pact-foundation](https://github.com/pact-foundation))
    
-   End to End BDD style tests, (cucumber)
    
-   Manual tests (to catch weird stuff, glitches, bad usability, etc)
    

  

Depending on the nature of the feature/project, some or all of these processes should be involved. It’s important that the team determine how much testing should be unit tests (tricky logic with lots of edge cases is a good example) and how much should be API tests or BDD tests. As code becomes more stable, there should be more and more of all of the above to ensure that updates don’t break older functionality.

## V. What is “Done”?

First, understand the difference between the “definition of done” and “acceptance criteria”. Acceptance criteria are specific to a task/feature/ticket/etc. The definition of done are a set of standards that apply by default to ALL work regardless of what that work is for.

  

Here’s my default:

-   Test code coverage must be at least as high as before (preferably higher). By default, the target should be 90% across branches/lines/etc.
    
-   CI/CD build passes all unit, integration, API, and performance checks and deploys properly without issues.
    
-   Static analysis checks do not flag any substantive problems.
    
-   The code has been peer reviewed.
    
-   Acceptance criteria in the ticket is met and validated by Manual QA.
    
-   The code obeys 12 factor app principles (for any web related code), or other architectural principles that the team agrees are more appropriate.
    

# Git Branching and Merging

Don’t overcomplicate this. Gitflow is popular but it’s author specifically says he did not have modern webapps in mind when he wrote it. Don’t overcomplicate. He recommends github flow.

  

[https://guides.github.com/introduction/flow/](https://guides.github.com/introduction/flow/)

  

I don’t like it, some people love it. I prefer trunk based development, but you need the right team to make that work. My suggestion is to hire the right team, but if you can’t or don’t know how, use the cludge that is gitflow.

  

# Team Performance Metrics

Measurement of performance is at the TEAM level. All for one, one for all. Because software is a team sport and so are startups, we either all succeed together or not at all.

  

There are 4 metrics proven to be oriented with team success:

  

-   Cycle Time - How long it takes to put a change into production (see “What is Done?”) once code is completed for a feature (completely defined as submitted for manual QA). Our target is < 4hrs. Meaning that once code is ready for QA, it will be evaluated by a tester, tested for load and other functional and non-functional requirements, and deployed into production in half a day. Ideal is under 2 hours.  
      
    
-   Lead Time - how long it takes to go from business idea to delivered software. Lead time can itself be divided into two:
    

-   Requirements definition time
    
-   Requirements -> Feature time.
    

The development team is primarily responsible for the second aspect of lead time. Once we get acceptable requirements, it’s up to us to find the fastest way to deliver working software. There is no target, we just want to continually improve this number.  
  

-   QA fail and Open rates - How many issues fail final QA and how many defects are reported by production. Again, no firm target, but the trend should be downward.  
      
    
-   Velocity - How many units of functionality we produce per unit time. This is not an objective measure but ties into the benefit perceived by the business (business velocity) and the complexity of the technical implementation (technical velocity). Maximum velocity comes from maximizing business impact with as little technical effort as possible.
    

  

# Tech Org Metrics

In addition to the dev team metrics above, [there 4 metrics](https://www.leanix.net/en/wiki/vsm/dora-metrics) associated with overall technical organization success.

-   Deployment frequency (how often you put code into production)
    
-   Lead time (just like above)
    
-   Mean time to restore failure (if things break in production, how long to restore your system)
    
-   Change failure rate (how often does code need to be rolled back)
    

If you are hiring a manager/executive in charge of technology, ask about these metrics. If they don’t know what you are talking about, don’t hire them.
