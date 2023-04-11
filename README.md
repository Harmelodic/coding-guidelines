# coding-guidelines

A set of language-agnostic coding guidelines.

## Contents

- [Naming Things](#Naming-things)
  - [Variables](#Variables)
  - Functions / Methods
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

I quite like Domain-Driven Design. To summarise poorly, we should name things according to the real thing they represent. Representing a car? Call it a car. Open an bank account? Don't "create" it, open it.  
That way, everyone's on the same page with what process is happening, and new developers with no knowledge can understand both the product & technical context in the same way.

### Variables

A variable's name should convey to the developer what sort of thing is stored in that variable.

This usually means that you want the variable name to be long enough to convey to the developer what thing exists, but concise enough to make the code quick to read.
Too long and the developer is reading a book, too short and they're reading algebra. We want neither. We want coherant code.

If in doubt, opt for a clearer, verbose name over a obscure, short one. This allows for developers who are new the codebase (or even programming language) to more easily understand what is going on.

Never use abbreviated variable names (e.g. `b`, `fr`, `svc`), unless it's an index in an for-loop. Even then, many modern languages have alternative iteration choices where the index is unnecessary (e.g. `forEach(...)`, `for ... in`).

In cases where the you're storing data that has units, either ensure the variable is using a type that has units as part of the type, or put the units in the variable name.

I have no solid opinion on whether acronyms should be capitalised in variables names, other than a simple: "Do what reads better."

```
f.readAll(); // poor
file.readAll(); // okay
files

int wait; // poor
int waitSeconds; // better

CustomerRepository r, // poor
                   repo, // poor
                   repository, // okay
                   customerRepository; // better
```

---

Thanks for reading.

I may change my mind on many of these things, or update to provide clarification on something. I often make mistakes, there may be many here.

~ Harmelodic
