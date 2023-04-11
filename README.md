# coding-guidelines

A set of language-agnostic coding guidelines.

## Contents

- [Naming Things](#Naming-things)
  - [Variables](#Variables)
  - Functions / Methods
  - Classes / Structures
  - Application / Service naming
  - Mention DDD here.
- Splitting code
  - Package structure
  - File structure
  - When to break into libraries
  - When to break into separate services / applications
- Patterns & Anti-Patterns
  - Dependency Injection
  - Constructor vs Builder
  - Immutable record objects
  - etc.
- Testing
  - Why you should do it
  - Don't monkey-patch
- Indentation
  - Tabs vs. Spaces
- Comments / Code-clarity

## Naming things

Good names convey information to developers. Poor names convey little to no information, or worse, can mislead developers.

### Variables

Variables contain things. A variable's name should convey to the developer what sort of thing is stored in that variable.

This usually means that you want the variable name to be long enough to convey to the developer what thing exists, but concise enough to make the code quick to read.

Unnecessary lengthening can result in a developer spending more time reading than thinking about function of the code, but unnecessary shortening results in a lack of conveyance in what is happening.

If in doubt, opt for a clearer, verbose name over a obscure, short one. This allows for developers who are new the codebase, or even programming language, to gain insight & understanding quicker.

Never use single or abbreviated variable name, unless it's an index in an for-loop.

In cases where the you're storing data that has units, either ensure the variable is using a type that has units as part of the type, or put the units in the variable name.

I have no solid opinion on whether acronyms should be capitalised in variables names, other than a simple: "Do what reads better."

```
f.read(); // poor
file.read(); // better

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
