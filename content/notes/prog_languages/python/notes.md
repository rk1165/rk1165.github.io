+++
title = 'Python Notes'
date = 2024-02-11

+++

- python can be passed arbitrary code as a string in the shell. `python3 -c 'print("Hello, World")'`.
- `dir(__builtins__)`
- `help(max)`
- pip list --outdated
- pip install [package_name] --upgrade
- Upgrading pip : pip install -U pip
- import cmath : for doing complex numbers maths
- `math.hypot(a, b)`: Euclidean norm `math.hypot(x2-x1, y2-y1)`
- `math.log(5, base=math.e)` computes the log of a number
- `math.log2(8)`, `math.log10(1000)`
- `math.log1p(5)` : Higher precision for low values.
- Bitwise operations are necessary in working with device drivers, low-level graphics, cryptography, and network communications.
- The `~` operator will flip all of the bits in the number. In general `~n = -n - 1`
- `~n` -> -|n+1| When applied to positive numbers
- `~-n` -> |n-1| when applied to negative numbers
- `or`: Evaluates to the first truthy argument if either one of the arguments is truthy. If both arguments are falsey, evaluates to the second argument.
- `and`: Evaluates to the second argument if and only if both of the arguments are truthy. Otherwise evaluates to the first falsey argument.
- `str = "weeksyourweeks"; str.translate(str.maketrans("wy", "gf", "u"))` - replace w->g, y->f and drop "u".
- `str.swapcase()` method is use to swap the strings case.
- The **try** statement has three separate clauses, or parts, introduced by the keywords **try … except … finally**. Either the _except_ or the _finally_ clause can be omitted.
- We can use multiple except clauses to handle different kinds of exceptions.
- We can raise our own exceptions using **_raise_**. e.g. **_raise ValueError(“something”)_**
- The **finally** clause of the try statement is the way to "clean up" the resources we grabbed e.g. close the window or close the file. The finally block is always executed.
- In `dict.get()` method first argument is the key, the second is the value get should return if the key is not in the dictionary. We should use it wisely with careful default value.
- `set.remove(x)` removes element x from the set. If element x does not exist, it raises a `KeyError`.
- `set.discard(x)` also removes element x from the set. If element x does not exist, it does not raises a `KeyError`. It returns `None`.
- `set.pop()` removes and return an arbitrary element from the set. If there are no elements to remove, it raises a `KeyError`.
- `set.difference()` returns a set with all the elements from the set that are not in an iterable.
- `set.symmetric_difference()` returns a set with all the elements that are in the set and the iterable but not both.
- We can use the following operations to create mutations to a set:
  - `.update` or `|=` - Update the set by adding elements from an iterable/another set.
  - `.intersection_update()` or `&=` - Update the set by keeping only the elements found in it and an iterable/another set.
  - `.difference_update()` or `-=` -> Updates the set by removing elements found in an iterable/another set.
  - `.symmetric_difference_update()` or `^=` -> Update the set by only keeping the elements found in either set, but not in both.
- `itertools.product()` computes the cartesian product of input iterables. It is equvalent to nested for-loops.
- `itertools.permutations(iterable[,r])` returns successive `r` length permutations of elements in an iterable. If `r` is not specified or is `None`, then `r` defaults to the length of the iterable, and all possible full length permutations are generated. Permutations are printed in a lexicographic sorted order. So, if the input iterable is sorted, the permutation tuples will be produced in a sorted order.
- `itertools.combinations(iterable,r)` returns `r` length subsequences of elements from the input iterable. Combinations are emitted in lexicographic sorted order. So, if the input iterable is sorted, the combination tuples will be produced in sorted order.
- A positional argument is an argument that is assigned to a particular parameter based on its position in the argument list
  - all positional arguments must come before all keyword arguments in the function call,
  - all positional arguments must come before any default arguments in a function definition.
- `sys.argv` displays all of the arguments
- `bytes.decode()` from bytes to string
- `eval` : it evaluates any expression passed to it in the form of a string and it evaluates it.
- We can define function in `exec`. even multiline function
- **Functions are first-class**: Functions can be manipulated as values in our programming language.
- **High-order function**: A function that takes a function as an argument value or return a function as a return value.
- Higher order functions:
  - Express general methods of computation
  - Remove repetition from programs
  - Separate concerns among functions
- This discipline of sharing names among nested definitions is called _lexical scoping_. Critically, the inner functions have access to the names in the environment where they are defined (not where they are called.)
- We can use higher-order functions to convert a function that takes multiple arguments into a chain of functions that each take a single argument. More specifically, given a function `f(x, y)`, we can define a function `g` such that `g(x)(y)` is equivalent to `f(x, y)`. Here, `g` is a higher-order function that takes in a single argument `x` and returns another function that takes in a single argument `y`. This transformation is called _currying_.
- A generator function is a function that yields values instead of returning them.
- A normal function returns once; a generator function can yield multiple times.
- A generator is an iterator created automatically by calling a generator function
- enumerate(iterable (such as list, dictionary etc)) it gives (index, value) on iteration
- `os.walk(directory)` will yield a tuple for each subdirectory. The first entry in the 3-tuple is a directory name, so `[x[0] for x in os.walk(directory)]`
- create the `ps` and `grep` processes separately, and pipe the output from one into the other
- `python3 -m pip install git+https://github.com/codio-content/shapes-package#wheel=shapes`
- A source distribution is the necessary source files and associated metadata that pip needs to install a package.
- a wheel is a built distribution
- There are three types of wheel :
  - Universal wheel - this wheel can run on either Python 2 or Python 3
  - Pure-Python wheel - this wheel only runs on Python 2 or Python 3, not both
  - Platform wheel - this wheel is designed for a specific version of Python and platform (process and/or operating system)

### Python 3 features

- supports easy way to write enumerations through the `Enum` class.
- makes using cache very simple by exposing an LRU cache as a decorator called [lru_cache](https://docs.python.org/3/library/functools.html#functools.lru_cache)
- introduces [data classes](https://docs.python.org/3/library/dataclasses.html) which do not have many restrictions and can be used to reduce boilerplate code because the decorator auto-generates special methods, such as `__init__()` and `__repr()__`

### Regular Expressions

| Identifiers | Meaning                                                             |
| ----------- | ------------------------------------------------------------------- |
| \d          | any number                                                          |
| \D          | anything but a number                                               |
| \s          | space                                                               |
| \S          | anything but a space                                                |
| \w          | any letter                                                          |
| \W          | anything but a letter                                               |
| .           | any character, except for a new line                                |
| \b          | space around whole words                                            |
| \.          | period. must use backslash, because . normally means any character  |

| Modifiers | meaning                                                    |
| --------- | ---------------------------------------------------------- |
| {1,3}     | for digits, u expect 1-3 counts of digits, or "places"     |
| +         | match 1 or more                                            |
| ?         | match 0 or 1 repetitions.                                  |
| *         | match 0 or MORE repetitions                                |
| $         | matches at the end of string                               |
| ^         | matches start of a string                                  |
| \|        | matches either/or. Example x \| y will match either x or y |
| []        | range, or "variance"                                       |
| {x}       | expect to see this amount of the preceding code            |
| {x,y}     | expect to see this x-y amounts of the precedng code        |

- Characters to REMEMBER TO ESCAPE IF USED - `. + * ? [ ] $ ^ ( ) { } | \`
