# Software Engineering Code
Software Engineering is the systematic application of engineering approaches to the development of software. Thousands of books are written about programming, management, practices and other things. There are engineering organizations that could use some healthy principles for building the organizations, but it is not easy to find the engineering approaches in a written form. 

The project reflects some ideas and conclusions the author came or tested during his experience of working on different software projects. With years, it became obvious that thoughts should be written for 2 reasons - I will not keep forgetting them and some people could find them interesting to apply for the software development or using the practices in the engineering organization.

The project consists of paragraphs of thoughts, principles and some practices. The intention is to have each paragraph bring one concise idea. Not all of them relate to software development only, but they were found to be useful there as well. 

## §1. The quality of a source code.
The source code should be maintainable: the good source code is friendly for changes, and can be changed easily for either bug fixing, introducing new functionalities or making some improvements. 

If the changes in a source code are painful, hard to be tested, or the result of the changes are hardly predicted, the source code is not maintainable and it is not friendly for the changes.

The source code maintainability sounds more important than the source code correctness. This is because if the source code is maintainable, but it is not correct, it can be fixed due to the code being friendly for the changes. The things could become more complicated when the code is correct, but not open for the new changes. This case introducing changes can be hard, but this is what normally is needed for alive products - the changes. 

## §2. Ability to write unit tests is an indicator of the source code maintainability.
If one can easily cover a source code by unit tests, most probably the source code has good quality and it is maintainable.

If somebody has some difficulties writing unit tests for a source code, or the source code requires special integration tests to be tested, this is the obvious sign that the source code has been designed badly and this is an indicator to rewrite or consider rewriting the source code ASAP.

## §3. Unimportance of programming language.
The source code quality is not related to the programming language the source code is written by. 

Good and bad source code can be written in any programming language. The programming languages are just tools. Blaming them is like saying that hammers with red-colored handles are much better than with the green ones or vise versa. 

## §4. The source code quality is caused by the system design.
Source code maintainability is caused by the object model and abstractions design used for modelling the process covered by the source code. 

If the abstraction model is not designed properly, or it has significant deviations from the modeled process, contains excess or lack of some objects, this will affect the source code quality and its maintainability. Always design the object model and try to build proper abstractions used in a source code.

The things like "spagetty code", "labirints of branches" etc. are effects of the poor design.

## §5. Design patterns learning path.
Writing the source code is a good way to understand what the design patterns are about.

Some people found books and articles about any design patterns are boring and uninteresting, but everyone who writes source code sooner or later comes up to understanding the main design patterns.

## §6. Design patterns revelation.
Most of the systems built by humans are built by using the same design patterns. 

The software system is not an exception. The source code for a single application and the big distributed systems use the same design patterns for developing the system architectures.

## §7. Using a debugger for a source code tells about complexity of the code.
If a debugger is needed to understand the source code behaviour - most probably the source code has maintainability problems.

Debugger is a great tool to investigate some problems, but the fact of using debugger can be a sign of the source code complexity and inability to understand it with minimal effort. There is a class of applications where debugger could be a main tool to work with the source code, but in general, in the software development world the following is true: if you use a debugger, most probably you don’t understand how the source code works and most probably the source code should be revised. 

## §8. Ability to run the source code on local developer machine. 
If it is impossible to run all or major parts of the source code locally with minimal configuration and external dependencies, most probably the source code is immature and it needs to be revised.

Use the local environment to run the source code. If the environment doesn’t exist, the environment must be created. In the majority of applications the source code must be run in local environments for debugging and test purposes. 

It doesn't matter what kind of software is developed - cloud service, distributed system, embedded automotive system etc. If there is no ability to test the source code partially or in whole in the local environement, something wrong with the source code.

If for testing a general algorithm like sorting a dedicated development environment is needed, the system is not designed properly.

## §9. Application logs are essential.
For major class of applications the application logs are the main tool for investigating problems. 

Writing application logs is important. If the source code doesn’t produce the applications logs in the amount to understand most of the scenarios and use cases, it could significantly affect the technical support of the application.

Always write the application logs for your application in a way that a reader could understand how the application was used and which scenarios were run.

## §10. Application logs should be human readable. 
It is a good idea to write the application log messages in the plain human-readable language in a way to understand easily what the function was run on.

A reader must easily understand the scenario which was run by the group of messages from the application logs. 

## §11. Structure of a log message.
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

## §12. Use log levels with a purpose.
It is proposed to use the following log levels: `INFO`, `DEBUG`, `WARN` and `ERROR`. 

`INFO` - use the level for the messages that explain what functions* are used. By these messages a reader should be able to understand what functions are executed and the use cases can be reconstructed. Ideally every `INFO` message can explain a function, its parameters and the result.

`DEBUG` - use the level to add more information to a context of the functionality. These messages should not contribute to understanding use-case (`INFO` messages do), but they can contain some internal details about the function. The rule of thumb - the `DEBUG` messages are for the developer, who tries to understand the internals of the execution. The INFO messages are for everybody, who wants to understand what scenarios were run and in which conditions.

`WARN` - use the log level for the messages, when some error, exception or a non-standard deviation from the normal execution flow took a place and when the application could handle it and continue to execute the function. Use the level for recoverable errors.

`ERROR` - use the level for the messages, when the error, exception or a deviation from the standard flow took place and when the application was not able to perform the function and interrupt its execution. It is not a fatal situation, but this is a situation when the source code doesn’t suppose some recovery on the request function level and interrupted the function.

*“functions” in the paragraph are some application/product functionality, but not literally a function in programming language terms. 

## §13. Writing application logs is an art.
Writing good quality application logs is a challenge. 

Due to the informality of the process it is hard to test or formally check the application logs. Practice to write application log messages and always read the application log for test-runs and test-cases to reveal the quality of the application log messages.

## §14. Stack trace in the application logs
Stack trace for a use-case should be printed once in the application logs. 

Use the rule: print the stack trace in the final point where an exception or the error is finally handled and no further actions will be done to the exceptional case. Do not print the stack trace in the application log in the middle of the process of handling the exceptional case or an error.

Writing stack traces multiple times or in the middle of the handling process produce garbage in the application logs, and it affects its readability.

## §15. Avoid printing log messages in not final error handlers.
If the error is not handled finally and it still can be propagated through another call stack in the application, consider not to print a log message, but use the other objects to collect information about the error. This may not be 100% true for all the cases, but in the majority of the cases it makes sense. 

For example, in the languages with exception support, it makes sense to gather some parameters in the object that will be re-thrown. 

If a source code function returns an error, which causes the caller interruption so the caller is not going to continue, gather the information about the error and return an object with the information upper by the call stack. Let the code, which is finally handling the situation, print the log message with all the gathered information by the stack trace. 

## §16. Layers of errors.
Design the application source code the way that internal components errors are always handled internally. The user level errors should be part of user interface or the API and they should be designed independ of the internal components behaviour and errors.

Errors and exceptional cases are essential parts of any processing, so they can happen on different parts of programming systems and be handled there as well. Software normally consists of different levels of abstractions and some errors can be handled on a level and some errors should be propagated to upper more-abstract levels for taking decisions about them there. 

The highest abstract level is the application user interface or an API. The principle calls to separate the class of errors exposed to the end user and the internal errors which happen on the application components.

Don’t expose internal component errors to the application API or to the user. The class of API or user errors should be separated from the internal component errors. The errors, exposed to the user, should be designed the same way as other system functions. They should be a part of the user interface and they should not contain any implementation specific. Always design the user errors class and find a way of translation of internal components errors to the first class. Never expose internal errors, or stack traces to the end user. 

## §17. A system liveness.
A software system is alive until it is possible to make changes in it. The software system which does not accept any changes is a dead system.

## §18. Nonlinear team scalability.
Increasing the size of a software engineering team in a short period of time doesn't contribute to the team performance, but the opposite can slow it down significantly or paralyze it all.

The short period of time here is weeks and even months. The team growth should be managed with care, with a pace the new members don’t affect the whole team performance significantly.

## §19. Complex systems are built from very small and simple blocks.
Big and complex systems can be decomposed to small and simple sub-systems and eventually to small programming components. 

Never try to design the complex system from scratch. Better to have a vision and the high-level design, which is open for the changes. By delivering and testing different pieces and components in a time the plan can be changed.

## §20. Source code granularity/decomposition.
Source code should be delivered in a way that any independent piece (component)  of it can be written in several hours, maximum 2 days by an engineer. 

By “delivered” in the context it means that the source code is written and tested via unit-tests.

## §21. Incremental small and concise changes
Source code of any system should be written by adding small, but complete changes. The good pace is to add any component to the source code base as soon as the component is ready (from several hours up to 2 days).

The errorprone approach is to attempt delivering a feature by one shot. Avoid keeping the new source code detached from the common codebase. Trying to test the more complex functionality without testing its components independently first will be followed by a longer development/debug cycle. 

## §22. Delivering changes to the main codebase.
Deliver changes to the main codebase as soon as the change is ready (written and tested) with the following conditions:
* The change doesn’t break the main codebase build.
* All unit-tests are passed.

## §23. The product is always ready to run from the source code.
The main codebase always contains ready to run the complete product. 

It means the following:
* If the source code is written in compilable language the source code is compiled and linked successfully
* All unit tests are passed
* The executable file(s) or the source code can be run in the required environment. 

The source code can contain some components for not-ready feature yet. These components can simply be not exposed to the user, but the source code most probably will contain such code all the time. 

## §24. Distinguish unit- and other types of tests (integration, load etc.)
Unit tests are parts of the source code, they must be delivered together with the tested components.

The unit tests don’t need special setups, environments or external dependencies. 

If for testing some special functionality an external component is needed and the functionality can be substituted by a mock object, most probably it is an integration test.

## §25. Unit-testing is a part of the delivery process.
For compilable languages the running unit test cycle should be a part of delivering the executable file - compilation and linking. For interpreted language the running unit-test suite can be the part of the source code delivery for executing in an environement.

## §26. Unit-testing cycle should run quickly.
The running unit-test cycle should take from several seconds up to several minutes.

The unit-tests run cycle should take minutes in very rare exceptional cases. 

## §27. Non-unit tests are running against an environment
Run non-unit-tests suites like integration, load, regression etc. against an environment where the executables are deployed.

Unit-testing is a part of delivering an executable or a source code to be ready to execute. Other tests should be run against the environments where the executables or the source code run.











