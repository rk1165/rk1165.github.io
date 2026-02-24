+++
title = 'Learn You a Haskell'
date = 2024-02-11

+++

### Introduction

- In purely functional programming you don't tell the computer what to do as such but rather you tell it what stuff _is_. The sum of a list of numbers is the first number plus the sum of all other numbers.

### Starting out

- `:set prompt "ghci> "` to change the prompt in ghci
- The `succ` function takes anything that has a defined successor and returns that successor
- If a function takes two parameters, we can also call it as an infix function by surrounding it with backticks. For instance **92 `div` 10**
- `div` does integral division.
- When a function doesn't take any parameters, we usually say it's a _definition_ (or a _name_)
- Lists are a **homogeneous** data structure. It stores several elements of the same type.
- We can use the `let` keyword to define a name right in GHCI.
- Doing `let a = 1` inside GHCI is the equivalent of writing `a = 1` in a script and then loading it.
- Putting something at the beginning of a list using the `:` operator is instantaneous
- To get an element out of a list by index, use `!!`.
- Lists can be compared if the stuff they contain can be compared.
- When using `<`, `<=`, `>` and `>=` to compare lists, they are compared in lexicographical order.
- `head` takes a list and returns its head.
- `tail` takes a list and returns its tail.
- `last` takes a list and returns its last element
- `init` takes a list and returns everything except its last element.
- `length` takes a list and returns its length
- `null` checks if a list is empty. Use this instead of xs == []
- `reverse` reverses a list.
- `take` takes number and a list. It extract that many elements from the beginning of the list.
- `drop` works in a similar way, only it drops the number of elements from the beginning of a list.
- `maximum` takes a list of stuff that can be put in some kind of order and returns the biggest element.
- `minimum` returns the smallest.
- `sum` takes a list and returns their sum.
- `product` takes a list of numbers and returns their product.
- `elem` takes a thing and a list of things and tells us if that thing is an element of the list. Usually called as an infix.
- `[1..20]` makes a list containing all the natural numbers from 1 to 20.
- To make a list with all the numbers from 20 to 1, you can't just do `[20..1]`, you have to do `[20,19..1]`.
- `cycle` takes a list and cycles it into an infinite list.
- `repeat` takes an element and produces an infinite list of just that element.
- Just use the `replicate` function if you want some number of the same element in a list.
- **list comprehension** `[x*2 | x <- [1..10]]`
- `[x*2 | x <- [1..10], x*2 >= 12]` list comprehension with predicate
- `[ x | x <- [10..20], x /= 13, x /=15, x /= 19]` multiple predicates.
- Not only can we have multiple predicates in list comprehensions (an element must satisfy all the predicates to be included in the resulting list), we can also draw from several lists
- `[ [x | x <- xs, even x] | xs <- xxs]` nested list comprehensions
- Tuples, however, are used when you know exactly how many values you want to combine.
- Tuples don't have to be homogeneous.
- Tuples can be compared if they are of the same size.
- `fst` takes a pair and returns its first component.
- `snd` takes a pair and returns its second component.
- `zip` takes two lists and then zips them together into one list by joining the matching elements into pairs.

### Types and Typeclasses

- A type is a kind of label that every expression has. It tells us in which category of things that expression fits.
- `:t` command followed by any valid expression, tells us its type.
- types are written in capital case
- `[a] -> a` here `a` is what's called a **type variable**. That means that `a` can be of any type.
- Functions that have type variables are called **polymorphic functions**
- A typeclass is a sort of interface (Java analogy) that defines some behaviour.
- If a type is a part of a typeclass, that means that it supports and implements the behaviour the typeclass describes.
- Everything before the `=>` symbol is called a **class constraint**.
- `(==) :: (Eq a) => a -> a -> Bool` : the equality function takes any two values that are of the same type and returns a **Bool**. The type of those two values must be a member of the **Eq** class
- `Eq` is used for types that support equality testing.
- `Ord` is for types that have an ordering.
- The `compare` function takes two `Ord` members of the same type and returns an ordering.
- `Ordering` is a type that can be **GT**, **LT** or **EQ**, meaning greater than, lesser than and equal, respectively
- To be a member of `Ord`, a type must first have membership in the prestigious and exclusive `Eq` club.
- Members of `Show` can be presented as strings.
- `show` takes a value whose type is a member of `Show` and presents it to us as a string.
- `Read` is sort of the opposite typeclass of `Show`.
- The `read` function takes a string and returns a type which is a member of `Read`
- `read "5" :: Int` to explicitly tell Haskell that read 5 as an Integer.
- `Enum` members are sequentially ordered types - they can be enumerated.
- The main advantage of the `Enum` typeclass is that we can use its types in list ranges.
- `succ` and `pred` functions can get the successor and predecessor.
- `Bounded` members have an upper and a lower bound.
- All tuples are also part of `Bounded` if the components are also in it.
- `Num` is a numeric typeclass. Its members have the property of being able to act like numbers.
- `Integral` includes only integral numbers. In this typeclass are `Int` and `Integer`
- `Floating` includes only floating point numbers, so `Float` and `Double`
- A very useful function for dealing with numbers is `fromIntegral`.

### Syntax in Functions

- `error` function takes a string and generates a runtime error using that string as information about what kind of error occurred.
- There's also a thing called as _patterns_. Those are handy way of breaking something up according to a pattern and binding it to names whilst still keeping a reference to the whole thing.
- You do that by putting a name and `@` in front of a pattern.
- For instance, `xs@(x:y:ys)`. This pattern will match exactly the same thing as `x:y:ys` but you can easily get the whole list via `xs`
- Guards are indicated by pipes that follow a function's name and its parameters.
- _where_ bindings aren't shared across function bodies of different patterns. If you want several patterns of one function to access some shared name, you have to define it globally.
- We can also define functions in where blocks.
- _where_ bindings can also be nested. It's a common idiom to make a function and define some helper function in its _where_ clause and then to give those functions helper functions as well, each with its own _where_ clause.
- Let bindings bind to variables anywhere and are expressions themselves, but are very local, so they don't span across guards.
- let bindings can be used for pattern matching.
- The form is `let <bindings> in <expression>`
- let bindings cal also be used to introduce functions in a local scope.
- If we want to bind to several variables inline, we obviously can't align them at columns. That's why we can separate them with semicolons.

```
case expression of pattern -> result
                   pattern -> result
                   ...
```

- expression is matched against the patterns.

### Higher order function

- Haskell functions can take functions as parameters and return functions as return values.
- Using partial application is a neat way to create functions on the fly so we can pass them to another function or to seed them with some data.
- `zipWith` takes a function and two lists as parameters and then joins the two lists by applying the function between corresponding elements.
- `takeWhile` function takes a predicate and a list and then goes from the beginning of the list and returns its elements while the predicate holds true.
- Normally, we make a lambda with the sole purpose of passing it to a higher-order function.
- To make a lambda, we write a `\` and then we write the parameters, separated by spaces. After that comes a `->` and then the function body.
- You can pattern match in lambdas. The only difference is that we can't define several patterns for one parameter, like making a `[]` and a `(x:xs)` pattern for the same parameter and then having values fall through.
- `foldl` also called the left fold. It folds the list up from the left side. The binary function is applied between the starting value and the head of the list.
- `foldr` the right fold's binary function has the current value as the first parameter and the accumulator as the second one `\x acc -> ...`
- The accumulator value (and hence, the result) of a fold can be of any type.
- The `++` function is much more expensive than `:`, so we usually use right folds when we're building up new lists from a list.
- Right folds work on infinite lists, whereas left ones don't.
- The `foldl1` and `foldr1` functions work much like `foldl` and `foldr`, only we don't need to provide them with an explicit starting value.
- They assume the first (or last) element of the list to be the starting value and then start the fold with the element next to it.
- `scanl` and `scanr` are like `foldl` and `foldr`, only they report all the intermediate accumulator states in the form of a list. There are also `scanl1` and `scanr1`, which are analogous to `foldl1` and `foldr1`.
- `filter` doesn't work on infinite lists.
- `$` also called _function application_
- Function application with `$` is right-associative.
- We do function composition with the `.` function.
- Another common use of function composition is defining functions in the so-called point free style (also called the pointless style)

### Modules

- A Haskell module is a collection of related functions, types and typeclasses.
- `nub` is a function defined in `Data.List` that takes a list and weeds out duplicate elements.
- To put the functions of modules into the global namespace when using GHCI. `:m + Data.List` or for multiple modules `:m + Data.List Data.Map Data.Set`
- To selectively import `import Data.List (nub, sort)`
- `import Data.List hiding (nub)` doesn't import `nub` function.
- `import qualified Data.Map as M` if we want to use filter we write `M.filter`
- [reference](https://downloads.haskell.org/~ghc/latest/docs/html/libraries/)

#### Data.List

- `intersperse` takes an element and a list and then puts that element in between each pair of elements in the list.
- `intercalate` takes a list of lists and a list. It then inserts that list in between all those lists and then flattens the result.
- `transpose` transposes a list of lists.
- `foldl'` and `foldl1'` are stricter versions of their respective lazy incarnations. lazy folds can produce stackoverflow error. Use these in that case.
- `concat` flattens a list of lists into just a list of elements.
- Doing `concatMap` is the same as first mapping a function to a list and then concatenating the list with `concat`
- `any` and `all` take a predicate and then check if any or all the elements in a list satisfy the predicate, respectively.
- `iterate` takes a function and a starting value. It applies the function to the starting value, then it applies that function to the result etc.
- `splitAt` takes a number and a list. It then splits the list at that many elements, returning the resulting two lists in a tuple.
- `takeWhile` takes elements from a list while the predicate holds and then when an element is encountered that doesn't satisfy the predicate, it's cut off. It turns out to be very useful.
- `dropWhile` drops all the elements while the predicate is true.
- `span` is kind of like `takeWhile`, only it returns a pair of lists. The first list contains everything the resulting list from `takeWhile` would contain if it were called with the same predicate and the same list. The second list contains the part of the list that would have been dropped.
- `break` breaks the list when the predicate is first true. `break p` is equivalent to `span (not . p)`
- `group` takes a list and groups adjacent elements into sublists if they are equal.
- `inits` and `tails` are like `init` and `tail`, only they recursively apply that to a list until there's nothing left.
- `isInfixOf` searches for a sublist with a list and returns True is the sublist is found.
- `isPrefixOf` and `isSuffixOf` search for a sublist at the beginning and at the end of a list, respectively.
- `elem` and `notElem` check if an element is or isn't inside a list.
- `partition` takes a list and a predicate and returns a pair of lists. The first list in the result contains all the elements that satisfy the predicate, the second contains all the ones that don't.
- `find` takes a list and a predicate and returns the first element that satisfies the predicate. But it returns that element wrapped in a `Maybe` value
- a `Maybe` value can either be `Just something` or `Nothing`.
- `elemIndex` is kind of like `elem`, only it doesn't return a boolean value. It maybe returns the index of the element we're looking for. If that element isn't in our list, it returns a `Nothing`
- `elemIndices` is like `elemIndex`, only it returns a list of indices, in case the element we're looking for crops up in our list several times.
- `findIndex` is like find, but it maybe returns the index of the first element that satisfies the predicate.
- `findIndices` returns the indices of all elements that satisfy the predicate in the form of a list.
- `lines` is a useful function when dealing with files or input from somewhere. It takes a string and returns every line of that string in a separate list.
- `unlines` is the inverse function of `lines`. It takes a list of strings and joins them together using a `'\n'`.
- `words` and `unwords` are for splitting a line of text into words or joining a list of words into a text. Very useful.
- `delete` takes an element and a list and deletes the first occurrence of that element in the list.
- `\\` is the list difference function. It acts like a set difference, basically. For every element in the right-hand list, it removes a matching element in the left one.
- `union` also acts like a function on sets.
- `intersect` works like set intersection.
- `insert` will start at the beginning of the list and then keep going until it finds an element that's equal to or greater than the element that we're inserting and it will insert it just before the element.
- Data.List has their more generic equivalents, named genericLength, genericTake, genericDrop, genericSplitAt, genericIndex and genericReplicate
- The nub, delete, union, intersect and group functions all have their more general counterparts called nubBy, deleteBy, unionBy, intersectBy and groupBy
- `on` function from `Data.Function`. `on` is used a lot with the _By_ functions.
- `f on g = \x y -> f (g x) (g y)`
- sortBy, insertBy, maximumBy and minimumBy take a function that determine if one element is greater, smaller or equal to the other
- When you're dealing with By functions that take an equality function, you usually do `(==) *on* something` and when you're dealing with By functions that take an ordering function, you usually do `compare *on* something`.

#### Data.Char

- `GeneralCategory` type is also an enumeration. It presents us with a few possible categories that a character can fall into.
- The main function for getting the general category of a character is `generalCategory`

#### Data.Map

- They are implemented internally using trees.
- The `fromList` function takes an association list (in the form of a list) and returns a map with the same associations.
- It needs the keys to be orderable so it can arrange them in a tree.
- `empty` represents an empty map. It takes no arguments, it just returns an empty map.
- `insert` takes a key, a value and a map and returns a new map that's just like the old one, only with the key and value inserted.
- `null` checks if a map is empty.
- `size` reports the size of a map
- `singleton` takes a key and a value and creates a map that has exactly one mapping
- `lookup` works like the Data.List lookup, only it operates on maps. It returns `Just something` if it finds something for the key and `Nothing` if it doesn't.
- `member` is a predicate takes a key and a map and reports whether the key is in the map or not.
- `map` and `filter` work much like their list equivalents.
- `toList` is the inverse of `fromList`
- `keys` and `elems` return lists of keys and values respectively.
- `fromListWith` is a cool little function. It acts like `fromList`, only it doesn't discard duplicate keys but it uses a function supplied to it to decide what to do with them.
- `insertWith` is to `insert` what `fromListWith` is to `fromList`. It inserts a key-value pair into a map, but if that map already contains the key, it uses the function passed to it to determine what to do

#### Data.Set

- The `fromList` function works much like you would expect. It takes a list and converts it into a set.
- use the `intersection` function to see which elements they both share.
- We can use the `difference` function to see which letters are in the first set but aren't in the second one and vice versa.
- we can see all the unique letters used in both sentences by using `union`.
- The null, size, member, empty, singleton, insert and delete functions all work like you'd expect them to.
- `isSubsetOf`, `isProperSubsetOf`
- Modules can also be given a hierarchical structures. Each module can have a number of sub-modules and they can have sub-modules of their own. Let's section these functions off so that `Geometry` is a module that has three sub-modules, one for each type of object.

### Making our own Types and Typeclasses

- One way to define our own type is using the **data** keyword.
- `data Bool = False | True`. `data` means we're defining a new data type.
- The part before the `=` denotes the type, which is `Bool`.
- The parts after the `=` are **value constructors**. They specify the different values that this type can have.
- When we write a value constructor, we can optionally add some types after it and those types define the values it will contain.
- Value constructors are actually functions that ultimately return a value of a data type.
- A value constructor can take some value parameters and then produce a new value.
- similarly, **type constructors** can take types as parameters to produce new types.
- `data Maybe a = Nothing | Just a` - the `a` here is the type parameter. Because there is a type parameter involved we call `Maybe` a type constructor.
- It's a very strong convention in Haskell to **never add typeclass constraints in data declarations.**
- When declaring a data type, the part before the `=` is the type constructor and the constructors after it are value constructors.
- It our data type can act like something that can be equated, we make it an instance of the `Eq` typeclass. If it can act like something that can be ordered, we make it an instance of the `Ord` typeclass.
- The `Show` and `Read` typeclasses are for things that can be converted to or from strings, respectively. If the constructors have fields, their type has to be a part of `Show` or `Read` if we want to make our type an instance of them.
- `Read` is pretty much the inverse typeclass of `Show`.
- `Show` is for converting values of our a type to a string, `Read` is for converting strings to values of our type.
- When we use the `read` function, we have to use an explicit type annotation to tell Haskell which type we want to get as a result.
- We can derive instances for the `Ord` type class, which is for types that have values that can be ordered.
- If we compare two values of the same type that were made using different constructors, the value which was made with a constructor that's defined first is considered smaller
- We can easily use algebraic data types to make enumerations and the `Enum` and `Bounded` typeclasses help us with that.
- `Enum` typeclass is for things that have predecessors and successors.
- `Bounded` typeclass is for things that have a lowest possible value and highest possible value.
- `type String = [Char]` - **type synonym**
- `data Either a b = Left a | Right b deriving (Eq, Ord, Read, Show)`
- When we're interested in how some function failed or why, we usually use the result type of `Either a b`. Errors use the `Left` value constructor while results use `Right`
- We can define functions to be automatically infix by making them comprised of only special characters. We can also do the same with constructors.
- When we define functions as operators, we can use that to give them a fixity (but we don't have to). A fixity states how tightly the operator binds and whether it's left-associative or right-associative.
- Sets and maps from `Data.Set` and `Data.Map` are implemented using trees, only instead of normal binary search trees, they use balanced binary search trees.
- When we write `class Eq a where` this means that we're defining a new typeclass and that's called `Eq`. `a` will play the role of the type that we will be making an instance of `Eq`.
- So _class_ is for defining new typeclasses and _instance_ is for making our types instances of typeclasses
- You can also make typeclasses that are subclasses of other typeclasses.
- Subclassing is just a class constraint on a _class_ declaration. `class (Eq a) => Num a where ...`
- To see what the instances of a typeclass are, just do `:info YourTypeClass`
- `Functor` typeclass is basically for things that can be mapped over. eg. lists.
- concrete type means (a type that a value can hold, like `Int`, `Bool` or `Maybe String`)
- for lists `fmap` is just `map`
- The `Functor` typeclass wants a type constructor that takes only one type parameter. So we will partially apply `Either`
- Types have their own little labels, called **kinds**. A kind is more or less the type of a type.
- We can examine the kind of a type by using the `:k` command in GHCI.
- A `*` means that the type is a concrete type.
- `* -> *` means that the type constructor takes one concrete type and returns a concrete type.

### Input and Output

- `putStrLn` takes a string and returns an **IO action** that has a result type of `()`
- An I/O action is something that, when performed, will carry out an action with a side-effect and will also contain some kind of return value inside it.
- We can use _do_ syntax to glue together several I/O actions into one.
- In a _do_ block, the **last action cannot be bound to a name**
- In Haskell `return` makes an I/O action out of a pure value.
- `putStr` similar to `putStrLn` except it doesn't jump to a new line.
- `putChar` takes a character and returns an I/O action that will print it out to the terminal.
- `print` takes a value of any type that's an instance of `Show`, calls `show` with that value to stringify and then outputs it.
- `getChar` is an I/O action that reads a character from the input.
- `when` is found in `Control.Monad`. It's useful for encapsulating `if something then do some I/O action else return ()`
- `sequence` takes a list of I/O actions and returns an I/O actions that will perform those actions one after the other.
- `mapM` takes a function and a list, maps the function over the list and then sequences it.
- `mapM_` does the same, only it throws the result later.
- `forever` takes an I/O action and returns an I/O action that just repeats the I/O action it got forever. It's located in `Control.Monad`
- `forM` (located in Control.Monad) is like `mapM`, only that it has its parameters switched around
- Normally we write `forM` when we want to map and sequence some actions that we define there on the spot using _do_ notation

#### Files and Streams

- `getContents` is an I/O action that reads everything from the standard input until it encounters an end-of-file character. It does lazy I/O.
- `interact` takes a function of type `String -> String` as a parameter and returns an I/O action that will take some input, run that function on it and then print out the function's result.
- `openFile` :: `FilePath -> IOMode -> IO Handle`.
- `hGetContents` It takes a `Handle`, so it knows which file to get the contents from and returns an `IO String`
- `hClose` takes a handle and returns an I/O action that closes the file.
- There's also `hGetLine`, `hPutStr`, `hPutStrLn`, `hGetChar` : they take handle
- `readFile` take a path to a file and returns an I/O action that will read that file and bind its contents to something as a string.
- `writeFile` takes a path to a file and a string to write to that file and returns an I/O action that will do the writing.
- `appendFile` has a type signature like `writeFile` only it appends stuff to the already existing file.
- `hSetBuffering` takes a handle and a `BufferMode` and returns an I/O action that set the buffering.
- `BufferMode` is a simple enumeration data type and the possible values it can hold are: `NoBuffering`, `LineBuffering` or `BlockBuffering (Maybe Int).`
- `hFlush` is a function that takes a handle and returns an I/O action that will flush the buffer of the file associated with the handle.
- `openTempFile` takes a path to a temporary directory and a template name for a file and opens a temporary file.
- `removeFile` takes a path to a file and deletes it.
- `renameFile` renames the temporary file _todo.txt_. takes file path.

#### Command Line Arguments

- The `System.Environment` module has two cool I/O actions. One is `getArgs`, which has a type of `getArgs :: IO [String]`. `getProgName` is an I/O action that contains the program name.

#### Randomness

- `System.Random` is used for randomness.
- `random :: (RandomGen g, Random a) => g -> (a, g)`
- The `RandomGen` typeclass is for types that can act as source of randomness.
- The `Random` typeclass is for things that can take on random values.
- The `System.Random` module exports `StdGen` that is an instance of the `RandomGen` typeclass.
- To manually make a random generator, use the `mkStdGen` function. It's type is `mkStdGen :: Int -> StdGen`.
- `randoms` takes a generator and returns an infinite sequence of values based on that generator.
- `randomR` is like `random`, only it takes as its first parameter a pair of values that set the lower and upper bounds and the final value produced will be within those bounds.
- `randomRs` produces a stream of random values within our defined ranges.
- `getStdGen` is an I/O action, which has a type of IO StdGen. It asks the system for a good random number generator and stores that in a so called global generator. `getStdGen` fetches us that global random generator.
- `newStdGen` action, splits our current random generator into two generators. It updates the global random generator with one of them and encapsulates the other as its result.

#### Bytestrings

- **ByteStrings** are sort of like lists, only each element is one byte in size.
- ByteStrings come in two flavors: strict and lazy ones.
- Strict bytestrings resided in `Data.ByteString`
- Lazy bytestrings resides in `Data.ByteString.Lazy`.
- The function `pack` has the type signature `pack :: [Word8] -> ByteString`. It takes a list of bytes of type `Word8` and returns a `ByteString`.
- `unpack` is the inverse function of `pack`. It takes a bytestring and turns it into a list of bytes.
- `fromChunks` takes a list of strict bytestrings and converts it to a lazy bytestring.
- `toChunks` takes a lazy bytestring and converts it to a list of strict ones.
- The bytestring version of `:` is called `cons`. It takes a byte and a bytestring and puts the byte at the beginning. use `cons'` if you're going to be inserting a lot of bytes at the beginning of a bytestring.
- `empty` makes an empty byte string

#### Exceptions

- To check if the file exists before trying to open it by using the `doesFileExist` function from `Sys.Directory`
- `catch` function from `System.IO.Error`
- `ioeGetFileName` takes an `IOError` as a parameter and maybe returns a `FilePath`.
- Using case expressions is commonly used when you want to pattern match against something without bringing in a new function.

```
main = do toTry catch handler1
          thenTryThis catch handler2
          launchRockets
```

### Functors, Applicative Functors and Monoids

- Haskell allows us to implement polymorphism on a much higher level.
- We think about what the types can act like and then connect them with the appropriate typeclasses.
- An `Int` can act like a lot of things. It can act like an equatable thing, like an ordered thing, like an enumerable thing, etc.
- Functors are things that can be mapped over, like lists, `Maybe`s, tree, and such. In Haskell, they're described by the typeclass `Functor`, which has only one typeclass method, namely `fmap`, which has a type of `fmap :: (a -> b) -> f a -> f b`
- If you ever find yourself binding the result of an I/O action to a name, only to apply a function to that and call that something else, consider using `fmap`, because it looks prettier.
- `(->) r` is an instance of `Functor`
- Using `fmap` over functions is just composition.
- Think of `fmap` as either a function that takes a function and a functor and then maps that function over the functor, or think of it as a function that takes a function and lifts that function so that it operates on functors.
- **functor laws** In order for something to be a functor, it should satisfy some laws.
  - The first functor law states that if we map the `id` function over a functor, the functor that we get back should be the same as the original functor.
  - The second law says that composing two functions and then mapping the resulting function over a functor should be the same as first mapping one function over the functor and then mapping the other one. `fmap (f.g) = fmap f . fmap g`

#### Applicative functors

- Applicative functors are represented in Haskell by the `Applicative` typeclass, found in the `Control.Applicative` module.
- by mapping "multi-parameter" functions over functors, we get functors that contain functions inside them. We can map functions that take these functions as parameters over them.
- `Applicative` typeclass lies in the `Control.Applicative` module and it defines two methods, `pure` and `<*>`. It doesn't provide a default implementation for any of them, we have to define them both if we want something to be an applicative functor.
- `pure :: a -> f a` it should take a value of any type and return an applicative functor with that value inside it.
- The `<*>` function has a type declaration of `f (a -> b) -> f a -> f b`. It takes a functor that has a function in it and another functor and sort of extracts that function from the first functor and then maps it over the second one.
- Use `pure` if you're dealing with `Maybe` values in an applicative context, otherwise stick to `Just`
- Applicative functors, allow you to operate on several functors with a single function.
- `pure f <*> x` equals `fmap f x`
- `Control.Applicative` exports a function called `<$>`, which is just `fmap` as an infix operator.
- Lists (actually the list type constructor, []) are applicative functors.
- Another instance of `Applicative` is `IO`
- Another instance of `Applicative` is `(->) r`
- Another instance of `Applicative` is `ZipList`, and it lives in `Control.Applicative`
- `[(+3), (*2)] <*> [1, 2]` when used as list applicative functors results in applying all the function in the left list to all the values in the right list.
- Another way this could work out is the first function being applied to the first value in the right list, the second function to the second element of the right list. This happens when `ZipList` applicative is used.
- `getZipList` function extracts a raw list out of a zip list.
- The `(,,)` function is the same as `\x y z -> (x, y, z)`
- `Control.Applicative` defines a function that's called `liftA2`. It just applies a function between two applicatives, hiding the applicative style.
- we can take two applicative functors and combine them into one applicative functor that has inside it the results of those two applicative functors in a list
- Applicative functors come with a few laws:
- `pure f <*> x = fmap f x`
- `pure (.) <*> u <*> v <*> w = u <*> (v <*> w)`
- `pure f <*> pure x = pure (f x)`
- `u <*> pure y = pure ($ y) <*> u`

### The newtype keyword

- The `newtype` keyword is used for cases when we want to just take one type and wrap it in something to present it as another type.
- When using `newtype` we are restricted to just one constructor with one field.
- If we derive the instance for a type class, the type that we're wrapping has to be in that type class to begin with.

### Monoids

- When we make a type, we think about which behaviours it supports, i.e. what it can act like and then based on that we decide which type classes to make it an instance of.
- A _monoid_ is when you have an associative binary function and a value which acts as an identity with respect to that function.
- `Monoid` type class exists for types which can act like monoids.
- The `Monoid` type class is defined in `import Data.Monoid`.
- Only concrete types can be made instances of `Monoid`, because the `m` in the type class definition doesn't take any parameters.
- `mempty` is a polymorphic constant
- `mappend` takes two values of the same type and returns a value of that type as well.
- Think of `mappend` being a binary function that takes two monoid values and returns a third.
- `mconcat` takes a list of monoid values and reduces them to a single value by doing `mappend` between the list's elements.
- Laws followed by monoids
- mempty `mappend` x = x
- x `mappend` mempty = x
- (x `mappend` y) `mappend` z = x `mappend` (y `mappend` z)
- Lists are monoids
- When there are several ways for some type to be an instance of the same type class, we can wrap that type in _newtype_ and then make the new type an instance of the type class in a different way.
- The `Data.Monoid` module exports two types for this, namely `Product` and `Sum`.
- Another type whih can act like a monoid in two distinct but equally valid ways is `Bool`. Any and All
- `Ordering` is a Monoid
- The `Ordering` monoid allows us to easily compare things by many different criteria and put those criteria in an order themselves, ranging from most important to the least.
- `Maybe a` Monoid
- `Foldable` is for things that can be folded up! It can be found in `Data.Foldable`
- To make a type constructor an instance of `Foldable` in easier way is to implement the `foldMap` function.
- `foldMap` comes in handy for reducing our structure to a single monoid value.
