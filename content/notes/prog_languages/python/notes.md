+++
title = 'Python Notes'
date = 2024-02-11

+++

- python can be passed arbitrary code as a string in the shell. `python3 -c 'print("Hello, World")'`.

```python
import keyword
print(keyword.kwlist)
```

- `dir(__builtins__)`
- `help(max)`
- pip list --outdated
- pip install [package_name] --upgrade
- Upgrading pip : pip install -U pip
- import cmath : for doing complex numbers maths
- The function `math.expm1(x)` computes e \*\* x - 1 . When x is small, this gives significantly better precision than `math.exp(x) - 1`.
- `math.hypot(a, b)`: Euclidean norm `math.hypot(x2-x1, y2-y1)`
- `math.log(5, base=math.e)` computes the log of a number
- `math.log2(8)`, `math.log10(1000)`
- `math.log1p(5)` : Higher precision for low values.
- Bitwise operations are necessary in working with device drivers, low-level graphics,
  cryptography, and network communications.
- The `~` operator will flip all of the bits in the number. In general `~n = -n - 1`
- `~n` -> -|n+1| When applied to positive numbers
- `~-n` -> |n-1| when applied to negative numbers
- The `<<` operator will perform a bitwise "left shift," where the left operand's value is moved left by the number of bits given by the right operand.
- `or`: Evaluates to the first truthy argument if either one of the arguments is truthy. If both arguments are falsey, evaluates to the second argument.
- `and`: Evaluates to the second argument if and only if both of the arguments are truthy. Otherwise evaluates to the first falsey argument.
- There is a powerful **translate** method available for strings. The idea is that one sets up desired substitutions — for every character, we can give a corresponding replacement character. - The **translate** method will apply these replacements throughout the whole string. The method is: `somestring.maketrans(“given string”, “result string”).`
- `str.swapcase()` method is use to swap the strings case.
- `str.isalnum()` checks if all the characters of a string are alphanumeric (a-z, A-Z,0-9).
- `str.isalpha()` checks if all the characters of a string are alphabetical (a-z, A-Z).
- `str.isdigit()` checks if all the characters of a string are digits (0-9)
- `str.islower()` checks if all the characters of a string are lowercase characters (a-z).
- `str.isupper()` checks if all the characters of a string are uppercase characters (A-Z).
- The **try** statement has three separate clauses, or parts, intriduced by the keywords **try … except … finally**. Either the _except_ or the _finally_ clauses can be omitted.
- The try statement executes and monitors the statements in the first block. If no exceptions occur, it skips the block under the except.
- We can use multiple except clauses to handle different kinds of exceptions.
- We can raise our own exceptions using **_raise_**. e.g. **_raise ValueError(“something”)_**
- The **finally** clause of the try statement is the way to “clean up” the resources we grabbed e.g. close the window or close the file. Whether we complete the statements in the try clause successfully or not, the finally block will always be executed.
- In `dict.get()` method first argument is the key, the second is the value get should return if the key is not in the dictionary. We should use it wisely with careful default value.
- If we want to add a single element to an existing set, we can use the .add() operation. It adds the element to the set and returns ‘None’.
- `set.remove(x)` removes element x from the set. If element x does not exist, it raises a `KeyError`. It returns `None`
- `set.discard(x)` also removes element x from the set. If element x does not exist, it does not raises a `KeyError`. It returns `None`.
- `set.pop()` removes and return an arbitrary element from the set. If there are no elements to remove, it raises a `KeyError`.
- `set.union()` returns the union of a set and the set of elements in an iterable.
- `set.intersection()` returns the intersection of a set and the set of elements in an iterable.
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
- `format(12/5, '.2f')`, `format(2 ** 100, '.6e')`
- a comma in the format specifier adds comma separators to the result
- For example, to produce the string 'Hello'
  - left-justified in a field width of 20 characters `format('Hello', '<20')`
  - right-justified in a field width of 20 characters `format('Hello', '>20')`
  - center-justified in a field width of 20 characters `format('Hello', '^20')`
- Another use of the format function is to create strings of blank characters, which is sometimes useful, `format(' ', '30')`
- A positional argument is an argument that is assigned to a particular parameter based on its position in the argument list
  - all positional arguments must come before all keyword arguments in the function call,
  - all positional arguments must come before any default arguments in a function definition.

```Python
sys.stderr.write('This is stderr text\n')`
sys.stderr.flush()
sys.stdout.write('This is stdout text\n')
```

- `sys.argv` displays all of the arguments
- bytes.decode() from bytes to string
- The idea of a socket is to aid in the communication between two entities. When you view a website, you are opening a port and connecting to that website via sockets
- You can think of a port much like a shipping port, where boats dock at the port and unload goods. Then, you can think of the ship itself as the socket. The ocean is the internet. Much like shipping ports, a socket (our ship in this metaphor), is bound by a specific port. Docking at a different port is not allowed, for ships or sockets!
- `eval` : it evaluates any expression passed to it in the form of a string and it evaluates it.
- We can define function in `exec`. even multiline function
- **Functions are first-class**: Functions can be manipulated as values in our programming language.
- **High-order function**: A function that takes a functio as an argument value or return a function as a return value.
- Higher order functions:
- Express general methods of computation
- Remove repetition from programs
- Separate concerns among functions
- This discipline of sharing names among nested definitions is called _lexical scoping_. Critically, the inner functions have access to the names in the environment where they are defined (not where they are called.)
- Newton's method is a classic iterative approach to finding the arguments of a mathematical function that yield a return value of 0. These values are called the _zeros_ of the function.
- We can use higher-order functions to convert a function that takes multiple arguments into a chain of functions that each take a single argument. More specifically, given a function `f(x, y)`, we can define a function `g` such that `g(x)(y)` is equivalent to `f(x, y)`. Here, `g` is a higher-order function that takes in a single argument `x` and returns another function that takes in a single argument `y`. This transformation is called _currying_.
- `nonlocal <name>`
- **Effect**: Future assignments to that name change its pre-existing binding in the **first non-local frame** of the current environment in which that name is bound.
- Names listed in a nonlocal statement must refer to pre-existing bindings in an enclosing scope.
- Expressions are **referentially transparent** if substituting an expression with its value does not change the meaning of a program.
- A generator function is a function that yields values instead of returning them.
- A normal function returns once; a generator function can yield multiple times.
- A generator is an iterator created automatically by calling a generator function
- enumerate(iterable (such as list, dictionary etc)) it gives (index, value) on iteration

### Python 3 features

- Python 3 offer [pathlib](https://docs.python.org/3/library/pathlib.html) as a convenient abstraction for working with file paths.
- [Why you should be using pathlib](https://treyhunner.com/2018/12/why-you-should-be-using-pathlib/)
- Python 3 supports type hints.
- Python 3 supports easy way to write enumerations through the `Enum` class.
- Python 3 makes using cache very simple by exposing an LRU cache as a decorator called [lru_cache](https://docs.python.org/3/library/functools.html#functools.lru_cache)
- Python 3 introduces [data classes](https://docs.python.org/3/library/dataclasses.html) which do not have many restrictions and can be used to reduce boilerplate code because the decorator auto-generates special methods, such as `__init__()` and `__repr()__`

### Summary of Turtle Methods

| Method     | Parameters | Description                                                    |
| ---------- | ---------- | -------------------------------------------------------------- |
| Turtle     | None       | Creates and returns a new turtle object                        |
| forward    | distance   | Moves the turtle forward                                       |
| backward   | distance   | Moves the turle backward                                       |
| right      | angle      | Turns the turtle clockwise                                     |
| left       | angle      | Turns the turtle counter clockwise                             |
| up         | None       | Picks up the turtles tail                                      |
| down       | None       | Puts down the turtles tail                                     |
| color      | color name | Changes the color of the turtle’s tail                         |
| fillcolor  | color name | Changes the color of the turtle will use to fill a polygon     |
| heading    | None       | Returns the current heading                                    |
| position   | None       | Returns the current position                                   |
| goto       | x,y        | Move the turtle to position x,y                                |
| begin_fill | None       | Remember the starting point for a filled polygon               |
| end_fill   | None       | Close the polygon and fill with the current fill color         |
| dot        | None       | Leave a dot at the current position                            |
| stamp      | None       | Leaves an impression of a turtle shape at the current location |
| shape      | shapename  | Should be ‘arrow’, ‘blank’ ‘classic’, ‘turtle’, or ‘circle’    |
| speed      | integer    | 0 = no animation, fastest; 1 = slowest; 10 = very fast         |

- Here a couple of new tricks for our turtles:
  - We can get a turtle to display text on the canvas at the turtle’s current position. The method to do that is `alex.write("Hello")`
  - We can fill a shape (circle, semicircle, triangle, etc.) with a color. It is a two-step process. First we call the method `alex.begin_fill()`, then we draw the shape, then we call `alex.end_fill()`
  - We’ve previously set the color of our turtle — we can now also set its fill color, which need not be the same as the turtle and the pen color. We use `alex.color("blue","red")` to set the turtle to draw in blue, and fill in red.

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
| \.          | period. must use backslash, because . normally means any character. |

| Modifiers | meaning                                                |
| --------- | ------------------------------------------------------ | ---------------------------- | -------------------------- |
| {1,3}     | for digits, u expect 1-3 counts of digits, or "places" |
| +         | match 1 or more                                        |
| ?         | match 0 or 1 repetitions.                              |
| \*        | match 0 or MORE repetitions                            |
| $         | matches at the end of string                           |
| ^         | matches start of a string                              |
|           |                                                        | matches either/or. Example x | y will match either x or y |
| []        | range, or "variance"                                   |
| {x}       | expect to see this amount of the preceding code.       |
| {x,y}     | expect to see this x-y amounts of the precedng code    |

- Characters to REMEMBER TO ESCAPE IF USED!
- . + \* ? [ ] $ ^ ( ) { } | \

| Brackets    | Meaning                                                         |
| ----------- | --------------------------------------------------------------- |
| []          | quant[ia]tative will find either quantitative, or quantatative. |
| [a-z]       | return any lowercase letter a-z                                 |
| [1-5a-qA-Z] | return all numbers 1-5, lowercase letters a-q and uppercase A-Z |

### Lesser known features

```python
>>> n = 14310023
# underscore separation
>>> f'{n:_}'
'14_310_023'
# you can also use comma separation for integers
>>> f'{n:,}'
'14,310,023'

>>> f'{n:_b}'
'1101_1010_0101_1010_1000_0111'
>>> f'{n:#_b}'
'0b1101_1010_0101_1010_1000_0111'
```

- Learn functools and itertools

### Getting a list of all subdirectories in the current directory

- os.walk(directory) will yield a tuple for each subdirectory. The first entry in the 3-tuple is a directory name, so
- `[x[0] for x in os.walk(directory)]`

### How to use subprocess command with pipes e.g ps -A | grep process_name

- create the `ps` and `grep` processes separately, and pipe the output from one into the other

```python
ps = subprocess.Popen(('ps', '-A'), stdout=subprocess.PIPE)
output = subprocess.check_output(('grep', 'process_name'), stdin=ps.stdout)
ps.wait()
```

- `python3 -m pip show soupsieve` : to get the metadata about soupsieve package
- `python3 -m pip install git+https://github.com/codio-content/shapes-package#wheel=shapes`
- A source distribution is the necessary source files and associated metadata that pip needs to install a package.
- a wheel is a built distribution
- There are three types of wheel :
  - Universal wheel - this wheel can run on either Python 2 or Python 3
  - Pure-Python wheel - this wheel only runs on Python 2 or Python 3, not both
  - Platform wheel - this wheel is designed for a specific version of Python and platform (process and/or operating system)
- 