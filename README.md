# coding-guidelines

A set of language-agnostic coding guidelines.

## Contents

- [Naming Things](#Naming-things)
  - [Variables](#Variables)
  - [Functions and methods](#Functions-and-methods)
  - Classes / Structures
  - Application / Service naming
- Splitting code
  - Package structure
  - File structure
  - When to break into libraries
  - When to break into separate services / applications
- Patterns & Anti-Patterns
  - Dependency Injection
  - Layers
  - Constructor vs. Builder
  - Immutable vs. Mutable
  - Composition over inheritance
- Testing
  - Why you should do it
  - Don't monkey-patch
- Indentation
  - Tabs vs. Spaces
- Comments / Code-clarity
- My preferences

## Naming things

Good names convey information to developers. Poor names convey little to no information, or worse, can mislead developers.

This usually means that you want the name to be long enough to convey meaning to the developer, but concise enough to make the code quick to read.
Too long and the developer is reading a book, too short and they're reading algebra. We want neither. We want coherant code.  
If in doubt, opt for a clearer, verbose name over a obscure, short one. This allows for developers who are new the codebase (or even programming language) to more easily understand what is going on.

I quite like Domain-Driven Design. To summarise poorly, we should name things according to the real thing they represent. Representing a car? Call it a car. Open an bank account? Don't "create" it, open it.  
That way, everyone's on the same page with what process is happening, and new developers with no knowledge can understand both the product & technical context in the same way.

Many names can be fancy or fun, but eventually someone else will need to understand your code, and accurate names are much more clear. No cute names.

### Variables

Variables store things. A variable's name should convey to the developer what sort of thing is stored in that variable.

Never use abbreviated variable names (e.g. `b`, `fr`, `svc`), unless it's an index in an for-loop. Even then, many modern languages have alternative iteration choices where the index is unnecessary (e.g. `forEach(...)`, `for ... in`).

In cases where the you're storing data that has units, either ensure the variable is using a type that has units as part of the type, or put the units in the variable name.

I have no solid opinion on whether acronyms should be capitalised in variables names, other than a simple: "Do what reads better."

```
f.readAll(); // poor
file.readAll(); // okay
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

A function or method (hereafer: "functions") performs an action. Whether the function is simple or complex, the function should have a single actionable purpose.

Function names should be defined from a consumer's point of view. Implementation has no business here. The function name, parameters & return type are the contract/interface of that function to consumers. The consumer of the function does not, and should not, care about the implementation (sorting algorithm, way of processing something), unless the function provides them with a means to affect the implementation (a lambda or flag).

If the function takes a parameter of a particular type, you probably don't need to specify the type in the function name - unless you're working in a language that has no support for overloading.

Avoid irrelevant jargon - e.g. `curry()` (Sorry, functional programmers/mathematicians).

```
function sumDigs(int number): int {} // poor
function sumOfDigits(int number): int {} // good
function summationOfIndividualDigits(int number): int {} // too much

function processTransaction(Transaction transaction): void {} // poor
function process(Transaction transaction): void {} // good

list.quicksort(); // poor
list.sort(); // good
list.sort(SortingAlgorithm.QUICKSORT); // good, given it is supported
list.sort(() => {...}); // good, given it is supported
```

---

Thanks for reading.

I may change my mind on many of these things, or update to provide clarification on something. I often make mistakes, there may be many here.

~ Harmelodic
