- OCaml has Win32 bindings, so you can write native Windows applications
- It supports functional, imperative, object-oriented, and logic programming all in the same language.
- OCaml has threads, exceptions, call-with-continuation, calling conventions to and from C, a rich standard library with collections, networking, I/O, graphics, a complete interface to the Unix Programming API, and a powerful module system.

## Functional Programming in OCaml

- There are five essential components to learning a language: syntax, semantics, idioms, libraries, and tools
- The _dynamic_ semantics define the run-time behavior of a program as it is executed or evaluated.
- The _static_ semantics define the compile-time checking that is done to ensure that a program is legal, beyond any syntactic requirements
- `ocamlc -o hello.byte hello.ml`
- `ocamlbuild hello.byte`
- `_build/sanitize.sh` to remove to clean up
- `ocamlbuild -clean`
- The primary task of computation in a functional language is to _evaluate_ an expression to a _value_
- The expression `assert e` evaluates `e`. If the result is `true`, nothing more happens else an exception is raised.
- There are two equality operators in OCaml, `=` and `==`, with corresponding inequality operators `<>` and `!=`. Operators `=` and `<>` examine _structural_ equality whereas `==` and `!=` examine _physical_ equality.
- Mutually recursive functions can be defined with the `and` keyword:
- `fun x -> x+1`. Here, `fun` is a keyword indicating an anonymous function, `x` is the argument, and `->` separates the argument from the body.
- There is a built-in infix operator in OCaml for function application that is written `|>`
- It's called the _pipeline_ operator.
- The `'a` is a _type variable_: it stands for an unknown type.
- OCaml supports labeled arguments to functions. You can declare this kind of function using the following syntax: `let f ~name1:arg1 ~name2:arg2 = arg1 + arg2;;`
- To declare a function with some arguments optional:
  `let f ?name:(arg1=8) arg2 = arg1 + arg2;;`
- Every OCaml function takes exactly one argument.
- Function types are _right associative_:there are implicit parentheses around function types, from right to left
- Function application, on the other hand, is _left associative_: there are implicit parenthesis around function applications, from left to right
- `let x = 42` defines `x` to be 42. We call this use of `let` a _let definition_.
- `let x = 42 in x + 1`. Here we're _binding_ a value to the name `x` then using that binding inside another expression, `x+1`. We call this use of `let` a _let expression_.
- _Principal of Name Irrelevance_ : the name of a variable shouldn't intrinsically matter.
- each let definition binds an entirely new variable. If that new variable happens to have the same name as an old variable, the new variable temporarily shadows the old. But the old variable is still around, and its value is immutable: it never, ever change
- OCaml has built-in printing functions for several of the built-in primitive types: `print_char`, `print_int`, `print_string`, and `print_float`. There's also a `print_endline` function, which is like `print_string` but also outputs a newline

## How to debug

- **Print statements**

```Ocaml
let inc x =
    let () = print_int(x) in
    x + 1
```

- **Function traces** : To see the _trace_ of recursive calls and returns for a function. Use the #trace directive
- **Debugger** `ocamldebug`
- `(** [sum lst] is the sum of the elements of [lst]. *)` an example of OCamldoc comment.

## Data in OCaml

- An OCaml list is a sequence of values all of which have the same type. They are implemented as singly-linked lists
- Lists are immutable.
- A function is _tail recursive_ if it calls itself recursively but doesn't perform any computation after the recursive call returns.
- In an OCamldoc comment, source code is surrounded by square brackets. That code will be rendered in typewriter face and syntax-highlighted in the output HTML.
- For list comprehension we can use `ppx_monadic` package. `opam install -y ppx_monadic`
- `[%comp x + y || x <-- [1;2;3]; y <- [1;2;3]; x < y];;`
- Unit testing workflow:
- Write a function in a file `f.ml`
- Write unit tests in a separate file `f_test.ml`
- Build and run `f_test.byte` to execute the unit tests.
- `ocamlbuild -pkgs oUnit sum_test.byte`
- To enable stack traces :
  `ocamlbuild -pkgs oUnit -tag debug sum_test.byte`
- A _variant_ is a data type representing a value taht is one of several possibilities.
- The individual names of the values of a variant are called _constructors_
- A _record_ is a kind of type that programmers can define. It is a composite of other types of data, each of which is named.
- A _tuple_ is a composite of other types of data. But instead of naming the _components_, they are identified by position.
- A value of a vari 1ant type is _one of_ a set of possibilities, whereas a value of a tupe/record type provides _each of_ a set of possibilities.
- `let f p1 ... pn = e1 in e2` function as part of let expression
- `let f p1 .. pn = e` function definition at toplevel
- `fun p1 ... pn` -> e anonymous function
- **Advanced patterns**
- `p1 | ... | pn` an "or" pattern
- `(p : t)` : a pattern with an explicit type annotation
- `c` here, `c` means any constant.
- `'ch1'..'ch2'` : 'A' .. 'Z' matches any uppercase letter.
- `p when e` : matches `p` but only if `e` evaluates to true.
- An implementation of dictionary is an _association list_, which is a list of pairs.
- A _type synonym_ is a new name for an already existing type.
- Type synonyms let us give descriptive names to complex types.
- Variant types are sometimes are called _tagged unions_
- Abother name for these variant types is an _algebraic data type_. "Algebra" here refers to the fact that variant type contain both sum and product types.
- Variants also make it possible to discriminate which tag a value was constructed with, even if multiple constructors carry the same type.
- Catch-all cases lead to buggy code, avoid them.
- Variant types may mention their own name inside their own body.
- Polymorphic variants are just like variants, except:

1.  You don't have to declare their type or constructors before using them.
2.  There is no name for a polymorphic variant type.
3.  The constructors of a polymorphic variant start with **`**

- Exception is defined with the syntax `exception E of t`
- To raise and exception `e` simply write, `raise e`
-
