# Software Engineering Code

Software Engineering is the systematic application of engineering approaches to the development of software. Thousands of books are written about programming, management, practices and other things. There are engineering organizations that could use some healthy principles for building the organizations, but it is not easy to find the engineering approaches in a written form. 

This project started from recording some ideas and conclusions the author came or tested during his experience of working on different software projects. With years, it became obvious that thoughts should be written for 2 reasons - I will not keep forgetting them and some people could find them interesting to apply for the software development or using the practices in the engineering organization.

The project consists of articles/paragraphs of thoughts, principles and some practices. The intention is to have each article brings one concise idea. Not all of them relate to software development only, but they were found to be useful there as well. 

## Questions
### How can I use the document?
Read it from start to end or partially and think about the statements. If you find it useful you can discuss it with your team members to use it practically. It is better than nothing to have it recorded, at least if you agree with a statement you can always refer to the pargraph number as an argument in a dialog with somebody...

### How do you want the rules to be enforced?
This is not the rules. Rules must be enforced and they suppose that somebody controls their execution. These paragraphs are written with the goal to initiate a thinking process for establishing some practices. You can follow them if you like and establish a process to obey some of them, but it is not a matter of the document.

### The paragraphs have much text and hard to read.
I am sorry about it. The first paragraphs of each article contains the idea. All the rests are for the clarification purposes only. 

### I got the idea, can I contribute?
Sure you can. Feel free to create a PR with the change and we will discuss it. The list of the contributors will be in [this file.](AUTHORS.md)

### I have some arguments, how can I tell you about them?
Open a PR or an issue and let's exchange by opinions there. 

## §1. The quality of a source code
The source code should be maintainable: the good source code is friendly for changes and can be changed easily for either bug fixing, introducing new functionalities, or making some improvements. 

If the changes in a source code are painful, hard to test, or the result of the changes is hardly predicted, the source code is not maintainable and is not friendly for the changes.

The source code maintainability sounds more important than the source code's correctness. This is because if the source code is maintainable but not correct, it can be fixed due to the code being friendly for the changes. Things could become more complicated when the code is correct but not open for the new changes. In this case, introducing changes can be hard, but this is what normally is needed for alive products - the changes. 

## §2. Ability to write unit tests is an indicator of the source code maintainability
If one can easily cover a source code by unit tests, most probably the source code has good quality and is maintainable.

If somebody has some difficulties writing unit tests for a source code, or the source code requires special integration tests to be tested, this is the obvious sign that the source code has been misdesigned, and this is an indicator to rewrite or consider rewriting the source code ASAP.

## §3. The unimportance of programming language
The source code quality is not related to the programming language the source code is written. 

Good and bad source code can be written in any programming language. Programming languages are just tools. Blaming them is like saying that hammers with red-colored handles are much better than green ones or vice versa.

## §4. The source code quality is caused by the system design
The source code maintainability is caused by the object model and abstractions design used for modeling the process covered by the source code. 

If the abstraction model is not designed properly, or it has significant deviations from the modeled process, contains excess or lack of some objects, this will affect the source code quality and its maintainability. Always design the object model and try to build proper abstractions used in a source code.

The things like "spaghetti code", "labyrinths of branches", etc. are the effects of poor design.

## §5. Design patterns learning path
Writing the source code is a good way to understand the design patterns.

Some people find books and articles about any design patterns boring and uninteresting, but everyone who writes source code sooner or later comes up to understanding the main design patterns.

## §6. Design patterns revelation
Most of the systems built by humans use the same design patterns.

The software system is not an exception. The source code for a single application and the big distributed systems use the same design patterns for developing the system architectures.

## §7. Using a debugger for a source code tells about the complexity of the code
If a debugger is needed to understand the source code behavior - most probably, the source code has maintainability problems.

The debugger is a great tool to investigate some problems, but the fact of using a debugger can be a sign of the source code complexity and inability to understand it with minimal effort. There is a class of applications where a debugger could be the main tool to work with the source code, but in general, in the software development world, the following is true: if you use a debugger, most probably you don’t understand how the source code works and most probably the source code should be revised.

## §8. Ability to run the source code on a local developer machine
If it is impossible to run all or major parts of the source code locally with minimal configuration and external dependencies, most probably the source code is immature and it needs to be revised.

Use the local environment to run the source code. If the environment doesn’t exist, the environment must be created. In the majority of applications, the source code must be run in local environments for debugging and testing purposes. 

It doesn't matter what kind of software is developed - cloud service, distributed system, embedded automotive system, etc. If there is no ability to test the source code partially or in whole in the local environment, something is wrong with the source code.

If the system is not designed properly for testing a general algorithm like sorting a dedicated development environment is needed.

## §9. Application logs are essential
The application logs are the main tool for investigating problems for the major application classes. 

Writing application logs is important. If the source code doesn’t produce the application logs in the amount to understand most of the scenarios and use cases, it could significantly affect the technical support of the application.

Always write the application logs for your application in a way that a reader can understand how the application was used and which scenarios were run.

## §10. Application logs should be human readable
It is a good idea to write the application log messages in a plain, human-readable language in a way to understand easily what the function was run on.

A reader must easily understand the scenario which was run by the group of messages from the application logs.

## §11. Structure of a log message
Use the same pattern for writing all log messages. 

For instance, every log entry can contain the following fields:
- The timestamp.
- The Log Level
- The component which, which produces the message
- The message itself.

The message, for example, can help the following format: `<what exactly happens> <in which circumstances> <what is the result>`

For example, consider 2 messages:
Good: `“read from file=report.txt, size=25Kb: lines=1735 read for time=0.12ms”`
Bad: `“read file from the disk”`

The first message complies with the proposed format, and it is more informative than the second one, which actually tells almost nothing, but the fact of some reading.

## §12. Use log levels with a purpose
It is proposed to use the following log levels: `INFO`, `DEBUG`, `WARN` and `ERROR`. 

`INFO` - use the message level that explains what functions* are used. Through these messages, a reader should be able to understand what functions are executed and the use cases can be reconstructed. Ideally, every `INFO` message can explain a function, its parameters, and the result.

`DEBUG` - use the level to add more information to the context of the functionality. These messages should not contribute to understanding the use-case (`INFO` messages do), but they can contain some internal details about the function. The rule of thumb - the `DEBUG` messages are for the developer, who tries to understand the internals of the execution. The INFO messages are for everybody who wants to understand what scenarios were run and in which conditions.

`WARN` - use the log level for the messages when some error, exception, or non-standard deviation from the normal execution flow took place and when the application can handle it and continue to execute the function. Use the level for recoverable errors.

`ERROR` - use the message level when the error, exception, or deviation from the standard flow took place and when the application could not perform the function and interrupted its execution. It is not a fatal situation, but this is a situation when the source code doesn’t suppose some recovery on the request function level and interrupts the function.

*“functions” in the paragraph are some application/product functionality, but not literally a function in programming language terms. 

## §13. Writing application logs is an art
Writing good-quality application logs is a challenge. 

Due to the informality of the process, it is hard to test or formally check the application logs. Practice writing application log messages and always read the application log for test runs and test cases to reveal the quality of the application log messages.

## §14. Stack trace in the application logs
Stack trace for a use case should be printed once in the application logs. 

Use the rule: print the stack trace at the final point where an exception or the error is finally handled and no further actions will be done to the exceptional case. Do not print the stack trace in the application log in the middle of the process of handling the exceptional case or an error.

Writing stack traces multiple times or in the middle of the handling process produces garbage in the application logs, and it affects its readability.

## §15. Avoid printing log messages in not final error handlers
If the error is not handled finally and it still can be propagated through another call stack in the application, consider not printing a log message, but using the other objects to collect information about the error. This may not be 100% true for all the cases, but in the majority of the cases, it makes sense. 

For example, in the languages with exception support, it makes sense to gather some parameters in the object that will be re-thrown. 

If a source code function returns an error, which causes the caller interruption so the caller is not going to continue, gather the information about the error and return an object with the information upper by the call stack. Let the code, which is finally handling the situation, print the log message with all the gathered information by the stack trace. 

## §16. Layers of errors
Design the application source code in a way that internal components errors are always handled internally. The user-level errors should be part of the user interface or the API, and they should be designed independently of the internal components' behavior and errors.

Errors and exceptional cases are essential parts of any processing, so they can happen in different parts of programming systems and be handled there as well. Software normally consists of different levels of abstractions and some errors can be handled on a level and some errors should be propagated to upper more-abstract levels for taking decisions about them there. 

The highest abstract level is the application user interface or an API. The principle calls to separate the class of errors exposed to the end user and the internal errors which happen on the application components.

Don’t expose internal component errors to the application API or to the user. The class of API or user errors should be separated from the internal component errors. The errors, exposed to the user, should be designed the same way as other system functions. They should be a part of the user interface, and they should not contain any implementation specific. Always design the user errors class and find a way of translating internal components errors to the first class. Never expose internal errors, or stack traces to the end user. 

## §17. A system liveness
A software system is alive until it is possible to make changes to it. The software system which does not accept any changes is a dead system.

## §18. Nonlinear team scalability
Increasing the size of a software engineering team in a short period of time doesn't contribute to the team's performance, but the opposite can slow it down significantly or paralyze it all.

The short period of time here is weeks and even months. The team growth should be managed with care, with a pace the new members don’t affect the whole team's performance significantly.

## §19. Complex systems are built from very small and simple blocks
Big and complex systems can be decomposed to small and simple sub-systems and eventually to small programming components. 

Never try to design the complex system from scratch. Better to have a vision and the high-level design, which is open for the changes. By delivering and testing different pieces and components in a time the plan can be changed.

### §19.1 Complexity in the software development
In software development, complexity can be defined as a characteristic that depends on a number of components, their connections, branches, etc.

In general, complexity characterizes the behavior of a system or model whose components interact in multiple ways and follow local rules, leading to nonlinearity, randomness, collective dynamics, hierarchy, and emergence. For designing and building software, we need to reduce the number of dependencies on every software component. Generally speaking, we have to reduce the number of components and connections between them, which can be achieved via building abstractions. 

## §20. Source code granularity/decomposition
Source code should be delivered in a way that any independent piece (component)  of it can be written in several hours, maximum 2 days by an engineer. 

By “delivered” in the context it means that the source code is written and tested via unit tests.

## §21. Incremental small and concise changes
Source code of any system should be written by adding small, but complete changes. The good pace is to add any component to the source code base as soon as the component is ready (from several hours up to 2 days).

The error-prone approach is to attempt to deliver a feature in one shot. Avoid keeping the new source code detached from the common codebase. Trying to test the more complex functionality without testing its components independently first will be followed by a longer development/debug cycle. 

## §22. Delivering changes to the main codebase
Deliver changes to the main codebase as soon as the change is ready (written and tested) with the following conditions:
* The change doesn’t break the main codebase build.
* All unit tests are passed.

## §23. The product is always ready to run from the source code
The main codebase always contains a ready-to-run complete product. 

It means the following:
* If the source code is written in compilable language the source code is compiled and linked successfully
* All unit tests are passed
* The executable file(s) or the source code can be run in the required environment. 

The source code can contain some components for the not-ready features yet. These components can simply be not exposed to the user, but the source code most probably will contain such code all the time. 

## §24. Distinguish unit- and other types of tests (integration, load, etc.)
Unit tests are parts of the source code, they must be delivered together with the tested components.

The unit tests don’t need special setups, environments, or external dependencies. 

If for testing some special functionality an external component is needed and the functionality can't be substituted by a mock object, most probably it is an integration test.

## §25. Unit-testing is a part of the delivery process
For compilable languages, the running unit test cycle should be a part of delivering the executable file - compilation and linking. For interpreted language, the running unit-test suite can be part of the source code delivery for executing in an environment.

## §26. The unit-testing cycle should run quickly
The running unit-test cycle should take from several seconds up to several minutes.

The unit-tests run cycle should take minutes in very rare exceptional cases. 

## §27. Non-unit tests are running against an environment
Run non-unit-test suites like integration, load, regression, etc., against an environment where the executables are deployed.

Unit-testing is a part of delivering an executable or a source code to be ready to execute. Other tests should be run against the environments where the executables or the source code run.

## §28. Design of the process of product delivery
Unit-testing is a part of delivering an executable or a source code to be ready to execute. Other tests should be run against the environments where the executables or the source code run.

Product delivery is a system that should be designed and expressed in any human-readable form to understand its structure. It has its own abstractions and procedures (pipelines) running over the abstractions. 

To deliver a software product to production for use by customers, multiple things should be done - the source code should be written, an executable artifact should be delivered (built from the source code), and its components should be tested on their correctness (unit-tested). Then the delivered artifacts should be tested so that they work together as expected (integration tests), and the functionality satisfies customer requirements and needs (regression, load, and functional tests are run). The tested executables can form the product that can be delivered, but the procedures should be described and scheduled by taking into account the things like - service interruptions, migrations, roll-back procedures, etc. 

Like any other system, the product delivery can be designed, and it consists of some abstract objects like - source code, executable artifacts, release candidates, tests suites, etc. And some procedures applied to the objects or pipelines like - actions done to deliver an executable, what should be done to test the executables, how the executable goes to production, etc. 

Find a way by describing the abstractions and formalizing pipelines to design your product delivery system for your business area. Describe requirements for every step and expected work-flows. 

## §29. Automate executable environment creation
Automate creating an environment for running your product.

Either you need to test your product locally, run it on different operating systems, or with different dependencies, or write the automation script, which checks and satisfies the dependencies to create the execution environment. This automation script will be the source of the environment design required to run your product. 

Using multiple scripts, or different scripts for different conditions is an error-prone approach. Having a manual process for running products locally, and different scripts for testing and production will produce different dependencies and conditions for running your code.  

## §30. Acceptance rule
The person who has a problem or question should accept the resolution.

The rule is not 100% true, but in the majority of the cases it helps to establish the roles. For example, in a google doc people can raise questions in the form of comments. The comments can be resolved after some discussion. The rule says - the person who raises the comment has to resolve the conversation (literally click the button “resolve the conversation”).

### §30.1 A code change responsibility
The author of the change is responsible for the change and its delivery.

For example, the code reviewers are responsible for review the change and its approval, but the author is responsible to submit and bring the changes into the main code base. In other words, nobody else, but the author of the change is responsible for the change merge.
Another example could be a ticket submitted to fix a defect in the ticket control system (JIRA for example). The rule says - the person who submits the ticket should accept the ticket resolution and close it. 

## §31. System capabilities
Define and declare your system capabilities.

On a component level a developer can define some characteristics like the algorithm complexity and memory/resources complexity. On a functional module level a developer can define the limitations in appropriate units for the module e.g. expected number of queries per second, the latency. On an application level there are some core metrics that indicate the application status and its performance etc. 

### §31.1 Measure and report the production system capabilities
A system in production should report its core metrics. 

Build a dashboard or any real-time report which shows the status of your systems, components and applications on different levels. Make the dashboard be easily accessible to everyone who monitors and is involved in the production support. 


## §32. Application metrics in production
Developers of the application are responsible for the metrics collected for the application.

A couple decades ago developers were responsible for writing the source code, which was normally tested by other people (QA org), who considered the system as a black box. Today’s standard is that the developers are responsible not only for writing the source code, but they also have to cover the source code by unit-tests. But developers very often are involved in the resolution of production issues, where for the monitoring of the application are often responsible other people - technical operations personnel, dev-ops etc. - the people who normally don’t know about the application implementation. It is not uncommon, that when a production issue happens a developer, who is awakened by a pager-duty call, has zero ideas what did happen and why. The developers don’t own what is reported on the monitoring dashboards and why the metric is there.

The principle says - developers should be responsible for the source code development, the unit-tests for the source code and for metrics reported on the production dashboards for the application. The developers should define what kind of metrics should be collected to help them to resolve production questions easily. Defining the monitoring and supporting it  in a relevant state should be under the source code developers responsibility. 

## §33. Interface and Implementation
Explicitly separate interfaces of the software components from their implementation.

Every software component has an interface even if it is not declared explicitly. Because the implementation of the component can be changed more often than its interface, declare the interface and put dependencies on the interface instead of the implementation. Describe the interface and its contract in a form of the component documentations either as a part of the source code or as a documentation to you programing components. 

For example, public functions can describe the input and output parameters and what the function does. Document publicly defined functions describing corner case scenarios and exceptional cases. For classes, modules and components define the abstraction with the contract and put dependencies on the abstractions, but not onto their implementations. Any application, also has the interface it supports and the set of agreements, explain and describe them in the application documentation etc. 

### §33.1 Dependencies on the interface
Any external components should depend (use) on other component interfaces, but not their implementations.

If your code invokes a function, it should rely on the contract the function supports, but not how the function is implemented. For example, a sort function can be implemented with the different algorithms, but the contract is the same - order a collection of elements. 

Another example is an application API, which actually describes the interface of the application and the contract of the functionality the application supports. Put dependencies on the API, but not on the internals of the application. Never rely on some details that are not explicitly described by the API. Implement the contract and rely on it.

### §33.2 API is an interface
An API is nothing more than an interface to an application.

Despite the API acronym being Application Program Interface, some people tend to mistificate the importance of API and its role in software development. As it said before an API is just an interface to an application. Any software component on different levels tends to have an interface and its implementation. The API is nothing but the interface to the application implementation. 

Don’t confuse a protocol which allows the API with the API itself, because API is a contract and in general it doesn’t matter which protocol implements the API - HTTP, gRPC etc.

## §34. SW Engineers paradox
Engineers with the same cognitive skills have different delivery velocities. 

This phenomenon is often underestimated and it causes misunderstandings and confusion. The reality is that different people can deliver quality source code with dramatically different speeds. The speeds can be different by hundreds of percentages. The software development paradox here is that the planning of deliverables cannot be estimated by statistics based on the speed of a myphical programmer, but it should take into account the performance of each individual who is involved in the delivery process.

The problem here is that during planning software development managers tend to use techniques similar to performing physical jobs like digging trenches. There an average adult is able to perform a job for X hours and the difference will be probably +/- 30%. This is not true for software development where different engineers are able to deliver the source code with the time difference of hundreds percentages (+/- 500+%). What can deliver engineer A for X hours, can be delivered by engineer B for 5X hours. And it is the norm in the industry.

## §35. YAGNI for the source code
Do not add functionality until deemed necessary, but improve the code maintainability with any change.

Always implement things when you actually need them, never when you just foresee that you need them, but strive to improve the code maintainability with any change. The original YAGNI (You aren’t gonna need it) principle is often misinterpreted and it turns out that people start to minimize changes in the source code disregarding its maintainability. Yes, deliver only what is needed, but with the necessary amount of the changes that doesn’t affect the code maintainability. This literally means that if only one line of the code must be changed, but it will affect the source code, so another 100 lines to make the code more friendly for the future changes - make the 101 lines change.

## §36. YAGNI for other things
Change something  to address a real problem, but not just for good things.

### §36.1 Don’t use Story Points if you don’t need them
Story Point is a unit  for estimating velocity of a team by delivering similar tasks. Story points is not a universal tool for planning software projects, so if you don’t have a certain problem which can be resolved by introducing this type of estimates, don’t push the team to provide estimates in story points.  

## §37. Eliminating performance review mystery
If the performance review should take place it is good if there is no cycle, but the performance can be assessed on every day, but not once per a quarter, half-, one-year etc. 

Some engineering organizations try to incorporate a performance review with the goal to assess performance of their employee once per quarter, half- or one-year. The following typical problems take place in such kind of environments:
1. Employees often are not communicated properly, so they don’t clearly understand what exactly and how will affect the final score. They can apply a lot of effort to have a good score, but surprisingly can receive a bad one at the end of the cycle.
2. The employees try to hack the system and their goal now is to have a good performance review which could be not related directly to the company goals. 

To avoid these kinds of problems it proposes to find a way how the performance of any engineer can be assessed instantly at any moment. If any engineer could clearly understand how a single task contributes to their performance, they can see a path to succeed. Make the performance metric clear to engineers. Performance should be a continuous function, but not points taken once per cycle. Forming a clear guidance for the performance function trends and measuring the performance based on day-to-day activity will allow to eliminate the stress, uncertainty and false goals. Build the process where every engineer understands their score at any particular moment.

### §37.1. Descriptive feedback on a regular basis
Descriptive feedback given on the regular basis helps people to understand how their efforts are aligned with the project goals. 

Descriptive feedback is specific information, in the form of written comments or verbal conversations, that help the learner understand what she or he needs to do in order to improve. Descriptive feedback given to an employee helps the employee to understand what is aligned and what is not with the vector of the project development. Such kind of feedback is more effective than an assessment of the job results. It contributes to intrinsic motivation.

## §38. Nothing is done in spare time
Any activity should be planned and nothing has 0 cost. 

Nothing comes from nowhere, so the needed information costs something. Planning and prioritization of the tasks and engineering activities helps to sort things out and don’t waste time on unnecessary or less important actions.

## §39. Build a backlog of engineering tasks
Record all tasks in a backlog and keep it prioritized.

Having a backlog of tasks allows the engineering teams to see the scope and pick the most important task from the backlog as soon as they are ready to work on the next one. 

## §40. Product engineering transparency
Technical and organizational parts of a product development should be transparent and easy to understand. 

This is a very important general principle which means that any aspect of a software product development should be easily learned by newcomers without significant efforts. It is common in software development organizations that a person or a group of persons have secret knowledge which is distributed by word of mouth. Very often somebody who is new to the system is told - “I recommend you talk to Jack, he is the best source of knowledge about this”. Avoid it. This knowledge is not scaled and the situation is favorable to create local, sometimes not scalable, solutions.

Build an organization, where new members can easily gain product knowledge with minimum mentoring, special meetings, talks with gurus etc.

## §41. Requirements come first
Requirements come first and the solution is created to satisfy the requirements. 

### §41.1 Avoid the situation when existing solutions affect the initial requirements
The existing tool should not affect the initial requirements. If the tool doesn’t allow you to satisfy the requirements, look for another tool/solution. 

It is very common when the product is built by a set of available tools, so the tools dictate the requirements of the product. This is not healthy because instead of satisfying the product requirements the solution sticks to the ability of the tools to do something, but not what is initially needed. It could be ok for a POC (proof of concept), but it is not going to work in the long term, because instead of searching for the solution, the solution is dictated by the set of available tools. In most cases this approach is not scalable and it follows the hard maintainability of the system.

### §41.2. A tool hype
The popularity of a tool, doesn’t mean that it should be used to satisfy requirements of your product.

It is common when organizations try to use some technology or a product just because of its popularity on the market. Avoid it. It increases a toil and it knocks out the focus on the result.

### §41.3. A tool is just an implementation
Every third party tool used in the product is just a part of the implementation.

It means that your solution should not stick to a tool, but to an abstract model for the solution. Always build the abstract model for your design and use tools just for the implementation of the model. Don’t stick to the 3rd party tools, they are just a part of your solution, which you have to be able to substitute by other tools if needed. 

### §41.4. Architecture comes before a technology
Work on the architecture first, then choose a technology to implement it.

If a technology comes first, it will always affect initial requirements and shrink an ability to implement them. First things first - design the system to satisfy the requirements, then select a technology for the implementation.

## §42. Leave the source code in a better state than it was before your change
Every code change should improve the code maintainability. 

Because code maintainability is a core characteristic of the code quality, always try to improve the source code maintainability together with any change made in the source code. 

### §42.1. Avoid technical debt as much as possible
Try to avoid implementing temporary solutions that must be fixed later in “proper” way. 

Technical debt is a concept in software development that reflects the implied cost of additional rework caused by choosing an easy (limited) solution now instead of using a better approach that would take longer. Some solutions seem to be attractive because of the delivery time, but they can increase the technical debt. The problem with the technical debt is it  is not repaid. This means that as more patches and not-well solutions are used, that more time will be needed in the future to refix them to improve the system maintainability. 

Another problem with the technical debt is an avalanche effect. The technical debt can dramatically affect the system maintainability which causes very hard and long changes. The bug that normally should be fixed for an hour or two can take weeks to be fixed because of poor code maintainability, which at the point doesn’t improve the system, but just increases technical debt itself. 

## §43. System extensibility rule
Third party tools decrease the system extensibility.

Using 3rd party tools allows to reduce delivery time, but it affects the product extensibility because the tools can limit the solution to be extended to meet future requirements.

## §44. Big problems are caused by small fraction of source code
Small fraction of the source code causes most product problems.

Finding and re-implementing the weak piece or component of the source code can improve the situation dramatically.

## §45. Customers will use your product an unexpected way
Be prepared that your product will be used in a not planned straight-forward way.

### §45.1. Corner cases always happen
If the corner cases are not tested by you, they will be tested by the product customers.

### §45.2. Customers will reveal the product use-cases you never thought of
Customers will reveal the product use-cases you never thought of.

The Hyrum's Law (The Law of Implicit Interfaces) states that with a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviours of your system will be depended on by somebody. It means that when you have a large enough number of consumers of an API, all behaviours of the API (even those not defined as part of a public contract) will eventually come to be depended on by someone. 

We often observe the situation of adopting some products by users of a market, but the products were initially targeting another market. For example Elasticsearch, which was initially targeted as a full-text search database widely adopted as an application log storage solution.

This is one of the examples when the changes in the API can be caused by adoption of the software, which can cause an evolutionary change of the interface contract with better satisfaction of the discovered use-cases.

## §46. Rules enforcements
Asking to obey rules doesn’t work in common. Asking to fix according to what the rules need works. 

For example, If a source code should be written according to a style, just have the style written, so everybody can read it. In case a source code change violates the style, the reviewer doesn’t approve the commit until the code will be written according to the style. The reviewer can refer to the style and the author always have a chance to read it through to comply next time.

## §47. A group is smarter than each member of the group
A group is no dumber than every member of the group.

Every group member contributes their wisdom to the collective mind. 

### §47.1 Collecting feedback ASAP
Collecting feedback can help to build a better vision to a problem or its solution.

Someone, who works on a problem or a solution can become a hostage of looped thinking or produce a tunnel vision for the solution. Collecting feedback and second opinions can help dramatically to find a better way or help to take another view to the problem, which was not considered initially.

### §47.2 Unnecessary secrecy causes poor solutions
Secrecy doesn’t contribute to better solutions, but develops a tunnel vision and shrinking the space of possible solutions.

### §47.3. Linus Law
Given enough eyeballs, all bugs are shallow.

This law simply states that the more people who can see a problem, the higher the likelihood that someone will have seen and solved the problem before, or something very similar.

## §48. Unclear decision making process indicates the avoiding responsibility
If the decision making process is not clear or there is no individual who makes decisions the avoiding of responsibility takes place. 
 
A decision can be made by an individual person, or by a group of individuals via a voting process. Either way the individual or a group of the individual shares their responsibilities for the decision. If there is no such individual or the group doesn’t have the process established and documented the avoiding responsibility takes place. 
 
Avoiding responsibility is not healthy because it encourages not owning mistakes by not being accountable for our flaws. There is a real danger that we will not learn from them and will make the very same mistakes over and over again. Even worse, a wrong self-conception can be adopted by lying to themself and, thus, will not give people room to improve and grow. By removing ourselves from a potential role in the problem we also remove ourselves from creating a potential solution. That, in turn, will take from us the satisfaction and joy to solve problems ourselves and overcome hurdles.
 
In short - avoiding responsibility is a demotivator.

---
This work is licensed under a [Creative Commons Attribution 4.0 International License](LICENSE.md). <img src="https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-sa.png" alt="drawing" width="88"/>
