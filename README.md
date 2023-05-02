# coding-guidelines

A set of language-agnostic coding guidelines.

## Contents

- [Naming things](#naming-things)
    - [Variables](#variables)
    - [Functions and methods](#functions-and-methods)
    - [Classes and Structures](#classes-and-structures)
    - [Applications](#applications)
- [Splitting code](#splitting-code)
    - [File structure](#file-structure)
    - [Package structure](#package-structure)
    - [Breaking into separate projects](#breaking-into-separate-projects)
    - [Creating libraries](#creating-libraries)
- Patterns
    - Dependency Injection
    - Layers
    - Constructor versus Builder
    - Immutable versus Mutable
    - Composition over inheritance
- Testing
    - Do it
    - Don't monkey-patch
- Indentation
    - Tabs versus Spaces
- Comments and Code-clarity
- My preferences

## Naming things

Good names convey information to developers. Poor names convey little to no information, or worse, can mislead
developers.

This usually means that the name should be long enough to convey meaning to the developer, but concise enough to make
the code quick to read. Too long and the developer is reading a book. Too short and they're reading algebra. We want
neither. We want coherent code.  
If in doubt, opt for a clearer, verbose name over an obscure, short one. This allows for developers who are new the
codebase (or even programming language) to more easily understand what is going on.

I quite like Domain-Driven Design. To summarise poorly, we should name things according to the real thing they
represent. Representing a car? Call it a car. _Opening_ a bank account? Don't "create" it, "open" it.  
That way, everyone's on the same page with what process is happening, and new developers with no knowledge can
understand both the product & technical context in the same way.

No cute names - many names can be fancy or fun, but eventually someone else will need to understand the code, and
accurate names are much more clear ("Cute" names are only for product names, for SEO/marketing purposes).

### Variables

Variables store things. A variable's name should convey to the developer what sort of thing is stored in that variable.

Never use abbreviated variable names (e.g. `b`, `fr`, `svc`), unless it's an index in a for-loop. Even then, many
modern languages have alternative iteration choices where the index is unnecessary (e.g. `forEach(...)`
and `for ... in`).

When the data has units, either ensure the variable is using a type that has units as part of the type, or put the units
in the variable name.

I have no solid opinion on whether acronyms should be capitalised in variables names, other than a simple: "Do what
reads better."

```
f.readAll(); // poor
file.readAll(); // poor
clearingFile.readAll(); // good
clearingFileToSendToAccountingDepartment.readAll(); // probably too much

int wait; // poor
int waitSeconds; // good

CustomerRepository r, // poor
                   repo, // poor
                   repository, // okay, if no other repositories present in file
                   customerRepository, // good
                   customerDatabaseRepository; // too much

var tlsRoute; // fine
var TLSRoute; // I "feel" this is less readable.
var accountApi; // fine
var accountAPI; // I "feel" this is more readable.
```

### Functions and methods

A function or method (hereafter: "functions") performs an action. Whether the function is simple or complex, the
function should have a single actionable purpose.

Function names should be defined from a consumer's point of view. Implementation has no business here. The function
name, parameters & return type are the contract/interface of that function to consumers. The consumer of the function
does not, and should not, care about the implementation (sorting algorithm, way of processing something), unless the
function provides them with a means to affect the implementation (a lambda or flag).

If the function takes a parameter of a particular type, the type doesn't need to be in the function name - unless the
language has no support for [overloading](https://en.wikipedia.org/wiki/Function_overloading).

Avoid irrelevant, overly-technical jargon, even if they already exist in a programming context e.g. `curry()`,
`malloc()` or `diff()`. This confuses and creates a steeper learning-curve for developers to be able to contribute to
the code.

```
int sumDigs(int number) {} // poor
int sumOfDigits(int number) {} // good
int summationOfIndividualDigits(int number) {} // too much

void processTransaction(Transaction transaction) {} // poor
void process(Transaction transaction) {} // good

list.quicksort(); // poor
list.sort(); // good
list.sort(SortingAlgorithm.QUICKSORT); // good, given it is supported
list.sort(() -> {...}); // good, given it is supported
```

### Classes and structures

Classes, structures, structs (hereafter: "classes") define blueprints for creating "objects". Depending on the language,
these classes can also contain "methods", which provide functionality relating to the class
(see: [Object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming)).

Classes should come in two varieties: Data classes, and Application classes (if doing OOP).

Data classes are, by their name, classes which define a data structure, and should be named after the structure of data
they represent, as a singular. If that data structure exists in the real world (outside of code) then opt to name it
that to further a Ubiquitous Language.  
Depending on the language, class names can have different requirement for casings, but general if
given the option: opt for making class names in PascalCase.
If the language offers an option for handling data classes, (Java Records, Python dataclasses, Kotlin data classes,
etc.), then use it - it will make usability of the code much more consistent.

If using Java, avoid Lombok and use Java Records.

```java
// Java
record Customer() {}
```

```kotlin
// Kotlin
data class Account {}
```

```python
# Python
@dataclass
class ApplicationForm:
```

Application classes, in OOP, would be classes that perform functionality. These often don't contain data themselves, but
instead contain references to other Application classes (through dependency injection). Exceptions to this are
applicable, e.g. When using
the [MVVM application architecture](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel).  
Application classes should be named after their functionality, as a Role, in the singular.

Concern can be had about the length of names here, and many developers will complain of too much verbosity, especially
in languages like Java. However, too short or vague names here can obscure or hide information that the developer may or
should have when developing on the code. If in doubt, opt for a more verbose name than a less verbose one to ensure that
developers new to the codebase can acquire all the necessary information they need to understand the code.

When using an interface-class relationship in OOP code, interfaces should be named clearly but abstractly, and the class
should then be named relating more to the implementation of the interface.

When a single class is implementing the interface, the interface can be omitted and the class can be named as whatever
the interface was going to be called, as the class and method signatures effectively become the interface, and the
contents of the class & methods become the implementation.

```java
class WikipediaApiClient {}

class ApplicationFormValidator {}

interface CustomerRepository {}
class SQLCustomerRepository implements CustomerRepository {}
class NoSQLCustomerRepository implements CustomerRepository {}
```

### Applications

Applications are codebases that, when built, result in a deployable piece of software. This can take the form of
services, microservices, daemons, desktop apps, mobile apps.

Applications should be named after their purpose, as a role, in the singular - using kebab-case.

As aforementioned in the [general guidelines for naming things](#naming-things), fun/fancy/cute names are very tempting
for applications. However, this will result in poor discoverability for the application within the organisation's SCM
and tooling, and will obscure the purpose of the application from fellow developers and consumers.  
Even with a "cute" product name, name the applications with simple, functional, accurate names.

Example: Building a Product / System for viewing media on consumer devices (laptops, mobiles) with optional, paid-for,
streaming functionality.

Application names should be along the lines of:

- `desktop-app`
- `android-app`, `ios-app` / `mobile-app` - depending on mobile app implementation
    - These could be called `client`s rather than `app`s, but `app` is the more widely-used term.
- `account-manager`
- `stream-provider`
- `invoice-generator`
- `payment-processor`
- `av1-encoder`
- etc.

With these application names, it is clear how the system is split up, and what each application is doing.

## Splitting code

Keeping all the code in one file can be good for little scripts or very small programs, but code will eventually reach
the point where it is much more maintainable to have the code organised into multiple files, packages, or even projects.

Never have a file/package/project named "utils" or "utilities". If this feels appropriate, code is either being split or
named incorrectly.

### File structure

Split code into components. One component per file.    
This does not have to be OOP-styled, assuming the codebase/language doesn't work with OOP principles, but breaking
applications up into components and layers delegates responsibility across the codebase and increases maintainability.

### Package structure

Package by feature, not by layer.  
This increases the decoupling and modularity of the project, by isolating features and allowing developers to work
within a specific context when developing on code.  
Conversely, packaging by layer simply identifies the different patterns and components of a project which, while
helpful for developers to know, obscures the feature-offering of the project and dependencies between layers. Since code
should already be split into files corresponding to layers, a package containing

### Breaking into separate projects

Assuming adherence to the "package by feature" principle, splitting a project into separate projects should feel
natural.

Some common triggers for a project split are:

- When a package's maintenance has become significantly cumbersome that it warrants its own project lifecycle.
- When the non-functional requirements on functionality have increased - e.g. redundancy, scalability, etc.
- When the project has become too much to "keep in your head".
- When the functionality/API of a package is used by more than just containing project.

### Creating libraries

Libraries form when multiple projects require common functionality, and it would be more efficient to reduce
code-reuse than duplicate code

Before a library forms, code duplication is natural and perfectly acceptable. A library/package to prevent
code-duplication should be formed when the code is being duplicated for the third+ time, and it is suspected that
duplication would happen again.

Libraries shouldn't contain common _product_-functionality, as applications fulfil that. Libraries should exist to
reduce complexity in the codebase and increase composability of software development.

Do not make a library containing an object model - doing this indicates that software architecture is veering towards a
rigid canonical model due to applications being too coupled together.

---

Thanks for reading.

I may change my mind on many of these things, or update to provide clarification on something. I often make mistakes,
there may be many here.

~ Harmelodic
