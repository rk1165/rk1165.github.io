+++
title = 'Crafting Interpreters'
date = 2024-02-11

+++

# Introduction

- **Yacc** is a tool that takes in a grammar file and produces a source file for a compiler, so it’s sort of like a “compiler” that outputs a compiler, which is where we get the term “compiler-compiler"
- A compiler reads in files in one language and translates them to files in another language. You can implement a compiler in any language, including the same language it compiles, a process called **self-hosting**.
- Use another compiler for your language written in some other language to compile your compiler once. Now use the compiled version of your own compiler to compile future versions of itself and you can discard the original one compiled from the other compiler. This is called **bootstrapping**

# A map of the territory

## The Parts of a Language

![implementation path](/images/implementation_path.png)

- Let's trace through the points of interest. Our journey begins on the left with the bare text of the user's source code:
  `var average = (min + max) / 2;`

### Scanning

- The first step is **scanning**, also known as **lexing**, or **lexical analysis**.
- A **scanner** takes in the linear stream of characters and chunks them together into a series of something more akin to "words". In PLs, each of these words is called a **token**. Example `(`, `,`, `123`, `"hi!"`

### Parsing

- The next step is **parsing**. This is where our syntax gets a **grammar** - the ability to compose larger expressions and statements out of smaller parts.
- A **parser** takes the flat sequence of tokens and builds a tree structure that mirrors the nested nature of the grammar.
- These trees have a couple of different names - **parse tree** or **abstract syntax tree**. In practice they are called **syntax trees**, **ASTs**

![syntax trees](/images/syntax_trees.png)

- The parser's job also includes telling us by reporting **syntax errors**
- The first two stages are pretty similar across all implementations
- At this point, we know the syntactic structure of the code - things like operator precedence and expression nesting.

### Static analysis

- The first bit of analysis that most languages do is called **binding** or **resolution**.
- For each **identifier** we find out where that name is defined and wire the two together. This is where **scope** comes into play.
- If the language is statically typed this is where we type check.
- All the semantic insight that is visible to us from analysis needs to be stored somewhere.
- Often, it gets stored right back as **attributes** on the syntax tree itself
- Other times, we may store data in a look-up table typically with keys being the identifiers - names of variables and declarations, off to the side. We call it a **symbol table**
- The most powerful bookkeeping tool is to transform the tree into an entirely new data structure that more directly expresses the semantics of the code.
- Everything up to this point is considered the **front end** of the implementation.

### Intermediate representations

- Think of the compiler as a pipeline where each stage's job is to organise the code in a way that makes the next stage simpler to implement.
- The front end of the pipeline is specific to the source language the user is programming in.
- The back end is concerned with the final architecture that the code will run on.
- In the middle, the code may be stored in some **intermediate representation** or **IR** that isn't tightly tied to either the source or destination forms.
- Few well established styles of IRs are - control flow graph, static single-assignment, continuation-passing style and three-address code.
- This lets you support multiple source languages and target platforms with less effort.
- Write _one_ front end for each source language that produces the IR. Then _one_ back end for each target architecture.
- [GIMPLE](https://gcc.gnu.org/onlinedocs/gccint/GIMPLE.html)
- [RTL](https://gcc.gnu.org/onlinedocs/gccint/RTL.html)

### Optimization

- Once we understand what the user's program means, we are free to swap it out with a different program that has the _same semantics_ but implements them more efficiently - we can **optimize** it.
- A simple example is **constant folding** - if some expression evaluates to the exact same value, we can do the evaluation at compile time and replace the code for the expression with its result.
- Some methods for optimization - "constant propagation”, “common subexpression elimination”, “loop invariant code motion”, “global value numbering”, “strength reduction”, “scalar replacement of aggregates”, “dead code elimination”, and “loop unrolling”.

### Code generation

- The last step is converting the user's program to a form the machine can actually run. In other words **generating code**, where "code" refers to the kind of primitive assembly-like instructions a CPU runs.
- This is the **back end**
- We have a decision to make. Do we generate instructions for a real CPU or a virtual one?
- If we generate real machine code, we get an executable that the OS can load directly onto the chip.
- Instead of instructions for some real chip, Niklaus Wirth and Martin Richards produced code for a hypothetical, idealized machine. Today we generally call it **bytecode** because each instruction is often a single byte long.
- You can think of these synthetic instructions like a dense, binary encoding of the languages's low-level operations.

### Virtual machine

- Since there is no chip that speaks that bytecode, it's your job to translate.
- You can write a mini-compiler for each target architecture that converts the bytecode to native code for that machine.
- Or you can write a **virtual machine (VM)**, a program that emulates a hypothetical chip supporting your virtual architecture at runtime.
- The basic principle here is that the farther down the pipeline you can push the architecture-specific work, the more of the earlier phases you can share across architectures.
- There is a tension, though. Many optimizations, like register allocation and instruction selection, work best when they know the strengths and capabilities of a specific chip. Figuring out which parts of your compiler can be shared and which should be target-specific is an art.

### Runtime

- If we compiled it to machine code, we simply tell the operating system to load the executable and off it goes.
- If we compiled it to bytecode, we need to start up the VM and load the program into that.
- In a fully compiled language, the code implementing the runtime gets inserted directly into the resulting executable.
- If the language is run inside an interpreter or VM, then the runtime lives there.

## Shortcuts and Alternate Routes

### Single-pass compilers

- Some simple compilers interleave parsing, analysis, and code generation so that they produce output code directly in the parser, without ever allocating any syntax trees or other IRs.
- These **single-pass compilers** restrict the design of the language.
- [Syntax-directed translation](https://en.wikipedia.org/wiki/Syntax-directed_translation) is a structured technique for building these all-at-once compilers. You associate an action with each piece of the grammar, usually one that generates output code. Then, whenever the parser matches that chunk of syntax, it executes the action, building up the target code one rule at a time.

### Tree-walk interpreters

- Some PLs begin executing code right after parsing it to an AST. To run the program, they traverse the syntax tree one branch and leaf at a time, evaluating each node as it goes.

### Transpilers

- We write a front end for our language. Then, in the back end, instead of doing all the work to _lower_ the semantics to some primitive target language, we produce a string of valid source code for some other language that's about as high level as ours. We use the existing compilation tools for _that_ language as our escape route off the mountain and down to something we can execute.
- This was called **source-to-source compiler** or a **transcompiler**

### Just-in-time compilation

- On the end user's machine, when the program is loaded - either from source in the case of JS, or from platform-independent bytecode for the JVM and CLR - you compile it to native for the architecture their computer supports.
- The most sophisticated JITs insert profiling hooks into the generated code to see which regions are most performance critical and what kind of data is flowing through them.

## Compilers and Interpreters

- **Compiling** is an _implementation technique_ that involves translating a source language to some other - usually lower-level - form. When you generate bytecode or machine code, you are compiling. When you transpile to another high-level language you are compiling too.
- When we say a language implementation “is a **compiler**”, we mean it translates source code to some other form but doesn’t execute it. The user has to take the resulting output and run it themselves.
- Conversely, when we say an implementation “is an **interpreter**”, we mean it takes in source code and executes it immediately. It runs programs “from source”.

## Challenges

- What reasons are there not to use JIT?
- Most Lisp implementations that compile to C also contain an interpreter that lets them execute Lisp code on the fly as well. Why?

# The Lox Language

- [declaration reflects use](http://softwareengineering.stackexchange.com/questions/117024/why-was-the-c-syntax-for-arrays-pointers-and-functions-designed-this-way)

#### Automatic memory management

- There are two main techniques for managing memory: **reference counting** and **tracing garbage collection** (usually just called “garbage collection” or “GC”).
- [A Unified Theory of Garbage Collection](http://www.cs.virginia.edu/~cs415/reading/bacon-garbage.pdf)

### Data Types

- Booleans : true; false;
- Numbers: double-precision floating point, `1234;`, `12.34`;
- Strings: `"I am a string";`
- Nil: `nil;`

### Expressions

- If built-in data types and their literals are atoms, then **expressions** must be the molecules.
- Lox features the basic arithmetic operators
- `add + me;`
- `subtract - me;`
- `multiply * me;`
- `divide / me;`
- The `-` operator can also be used to negate a number
- **Missing ops** that can be implemented : bitwise, shift, modulo or conditional operators.

### Statements

- Where an expression's main job is to produce a _value_, a statement's job is to produce an _effect_
- An expression followed by a semicolon promotes the expression to statement-hood. This is called, an **expression statement**
- do-while, for-in loop can be implemented

### Functions

- An **argument** is an actual value you pass to a function when you call it. Sometimes called "**actual parameter**"
- A **parameter** is a variable that holds the value of the argument inside the body of the function. Thus, a function _declaration_ has a _parameter_ list. Sometimes called **formal parameters**

### Closures

- An inner function that "holds on" to references to any surrounding variables that it uses so that they stay around even after the outer function has returned. The functions that do this are called **closures**
- [The Next 700 Programming Languages](https://homepages.inf.ed.ac.uk/wadler/papers/papers-we-love/landin-next-700.pdf)

### Classes

- When it comes to objects, there are actually two approaches to them, [classes](https://en.wikipedia.org/wiki/Class-based_programming) and [prototypes](https://en.wikipedia.org/wiki/Prototype-based_programming).
- In prototypes, you don't need to have some "class"-like construct that represents a "kind of thing". Methods can exist right on an individual object and objects can inherit from ("delegate to" in prototypical lingo) each other.
- With classes, state is on the instance, but for methods, there is always a level of indirection.
- The idea behind OOP is encapsulating behaviour _and state_ together. To do that one need fields.
- If you wanted to turn Lox into an actual useful language, the very first thing you should do is flesh this out. String manipulation, trigonometric functions, file I/O, networking, heck, _even reading input from the user_ would help.

### Challenges

- Write some sample Lox programs and run them.
- Several open questions about the language's syntax and semantics.
- Features that might be added.

# Scanning

- This task has been variously called "scanning" and "lexing" (lexical analysis)
- The scanner takes in raw source code as a series of characters and groups it into meaningful chunks - the "words" and "punctuation" that make up the language's grammar.
- It’s a good engineering practice to separate the code that _generates_ the errors from the code that _reports_ them.
- Ideally, we would have an actual abstraction, some kind of "ErrorReporter" interface that gets passed to the scanner and parser so that we can swap out different reporting strategies.
- Lexical analysis is about scanning through the list of characters and group them together into the smallest sequences that still represent something. Each of these blobs of characters is called a _lexeme_. e.g `var` `language` `=` `"lox"` `;`
- The lexemes are only the raw substrings of the source code.
- The point that we recognize a lexeme, we also remember which _kind_ of lexeme it represents.
- Some token implementations store the location as two numbers: the offset from the beginning of the source file to the beginning of the lexeme, and the length of the lexeme.
- One might consider defining a regex for each kind of lexeme and use those to match characters.
- The rules that determine how characters are grouped into lexemes for some languages are called its **lexical grammar**
- The rules of that grammar are simple enough to be classified as a [regular language](https://en.wikipedia.org/wiki/Regular_language).
- [Chomsky hierarchy](https://en.wikipedia.org/wiki/Chomsky_hierarchy)
- [FSM](https://en.wikipedia.org/wiki/Finite-state_machine)
- You very precisely _can_ recognize all of the different lexemes for Lox using regexes if you want to, and there’s a pile of interesting theory underlying why that is and what it means. Tools like [Lex](http://dinosaur.compilertools.net/lex/) or [Flex](https://github.com/westes/flex) are designed expressly to let you do this—throw a handful of regexes at them, and they give you a complete scanner back.
- General strategy for handling longer lexemes : after detection of beginning of one, we shunt off to some code specific to that kind of lexeme that keeps eating characters until it sees the end.
- **lookahead** means looking at the current unconsumed character. The lexical grammar dictates how much lookahead we need.
- **Maximal munch** - When two lexical grammar rules can both match a chunk of code that the scanner is looking at, _whichever one matches the most characters wins_
- If you are designing a new language, you almost surely _should_ avoid an explicit statement terminator.

### Challenges

- Lexical grammars of Python and Haskell are not _regular_. What does that mean, and why aren't they?
- Add support to Lox's scanner for C-style `/* ... */` block comments. Handle newlines in them. Consider nesting.

# Representing Code

### Context-Free Grammars

- A workable representation of our code is a tree that matches the grammatical structure of the language.
- [Formal grammars](https://en.wikipedia.org/wiki/Formal_grammar) : A formal grammar takes a set of atomic pieces it calls its "alphabet". Then it defines a (usually infinite) set of "strings" that are "in" the grammar. Each string is a sequence of "letters" in the alphabet.
- In the syntactical grammar each "letter" in the alphabet is an entire token and a "string" is a sequence of _tokens_ - an entire expression.

| Context-free grammar | Lexical grammar | Syntactic grammar |
| :------------------- | :-------------- | :---------------- |
| The "alphabet" is... | Characters      | Tokens            |
| A "string" is...     | Lexeme or token | Expression        |
| Implemented by the.. | Scanner         | Parser            |

- A formal grammar's job is to specify which strings are valid and which aren't.

#### Rules for grammars

- To write down a grammar that contains an infinite number of valid strings we can "play" in one of two directions.
- Start with the rules, and used them to _generate_ strings that are in the grammar. Strings created this way are called **derivations** because each is "derived" from the rules of the grammar.
- Rules are called **productions** because they _produce_ strings in the grammar.
- Each production in a c-f grammar has a **head** - its name - and a **body** which describes what it generates.
- Restricting heads to a single symbol is a defining feature of context-free grammars. More powerful formalisms like [unrestricted grammars](https://en.wikipedia.org/wiki/Unrestricted_grammar) allow a sequence of symbols in the head as well as in the body.
- In its pure form the body is simply a list of symbols. Symbols come in two delectable flavours :
- A **terminal** is a letter from the grammar's alphabet. In the syntactic grammar these would be individual lexemes - like `if` or `1234`
- A **nonterminal** is a named reference to another rule in the grammar. It means "play that rule and insert whatever it produces here"
- [Backus-Naur form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form)

#### A Grammar for Lox expressions

```
expression -> literal | unary | binary | grouping ;
literal    -> NUMBER | STRING | "true" | "false" | "nil";
grouping   -> "(" expression ")"
unary      -> ( "-" | "!" ) expression
binary     -> expression operator expression ;
operator   -> "==" | "!=" | "<" | "<=" | ">" | ">=" | "+" | "-" | "*" | "/";
```

#### Implementing Syntax Trees

- Tree to represent the syntax of our language is called a **syntax tree**. In particular an _abstract_ syntax tree. In a **parse tree**, very single grammar production becomes a node in the tree. An AST elides productions that aren’t needed by later phases.
- We have a family of classes and we need to associate a chunk of behaviour with each one. The natural solution in in OOPL like Java is to put that behaviour into methods on the classes themselves. We could add an `interpret()` method on `Expr` which each subclass then implements to interpret itself. This is called the **Interpreter pattern**
  ![OOP Style](/images/OOP_style.png)
- An object-oriented language like Java assumes that all of the code in one row naturally hangs together. It figures all the things you do with a type are likely related to each other, and the language makes it easy to define them together as methods inside the same class
  ![Functional Style](/images/functional_style.png)
- Functional paradigm languages in the ML family flip that around. There, you don’t have classes with methods. Types and functions are totally distinct. To implement an operation for a number of different types, you define a single function. In the body of that you use pattern matching—sort of a type-based switch on steroids—to implement the operation for each type all in one place.
- An OO language wants you to _orient_ your code along the rows of types.
- A functional language instead encourages to lump each each column's words of code together into _functions_
- Converting a tree to a string is sort of the opposite of a parser, and is often called "pretty printing" when the goal is to produce a string of text that is valid syntax in the source language.
