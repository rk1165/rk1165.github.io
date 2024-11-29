+++
title = 'Haskell Notes'
date = 2024-02-11

+++

### Haskell in a startup

- Haskell distinguishes itself from most languages by purity, laziness, and its strong, static, inferred type system.
- Purity lets you reason more easily about your program.
- Laziness allows for better function composition and optimisation (at the cost of harder-to-understand memory profiles).
- Strong, static, inferred types deliver a lightweight mechanism for enforcing consistency.
- [State of the Haskell ecosystem](https://github.com/Gabriel439/post-rfc/blob/master/sotu.md)
- [lens](https://twitter.com/fylwind/status/549342595940237312)
- [QuickCheck](https://hackage.haskell.org/package/QuickCheck), for example, can leverage Haskell's type system to automatically generate a large number of random test-cases to check that a programmer-specified property hold for a given function.

### Useful GHCi commands

| Command            | Meaning               |
| ------------------ | --------------------- |
| `:load name`       | load script name      |
| `:reload`          | reload current script |
| `:set editor name` | set editor to `name`  |
| `:edit name`       | edit script `name`    |
| `:edit`            | edit current script   |
| `:type expr`       | show type of `expr`   |
| `:?`               | show all commands     |
| `:quit`            | quit GHCi             |

### data, type, newtype, instance, class

- `data` is used to declare a new algebraic data type.

```haskell
data Bool = True | False
data Maybe a = Nothing | Just a
```

- `type` is used to create an alias for an algebraic data type.

```haskell
type String = [Char]
```

- `newtype` acts similarly to `type` with a syntax akin to `data`

```haskell
newtype Radius = Radius Double
data Diameter = Diameter Double
```

- Difference between `newtype` and `data`:
  - `newtype` can only have a single constructor taking a single argument
  - `newtype` creates a strict value constructor and type creates a lazy one
  - `newtype` introduces no runtime overhead.
- A typeclass is a way to guarantee that a type implements certain function (or data). A type is declared to implement the functions using the keyword `instance`.

```haskell
type Point = (Int, Int)
data Triangle = Triangle Point Point Point deriving (Show)
data Square = Square Point Point Point Point deriving (Show)

class Shape a where
  rotate :: a -> a
  simple :: a

instance Shape Triangle where
  rotate (Triangle x y z) = Triangle z x y
  simple = Triangle (0, 0) (1, 0) (0, 1)

instance shape Square where
  rotate (Square w x y z) = Square z w x y
  simple = Square (0, 0), (1, 0), (1, ,1), (0, 1)
```

- Think of typeclass as sort of interface or a contract.

### Chap 2

- A **value** in Haskell is any data. The word "Step", for example.
- All values have a **type**.
- A **type** is a name for a set or group of values that are similar.
- A **function** is a relation between one type and another type, and they're used in expressions with values.
- In Haskell a function is itself a value.
- `main = putStrLn "Polly wants a cracker"`
- Every program must define `main`, and it is evaluated and executed by Haskell to run your program.

### Chap 3

- stack ghc -- <file-names>
- Haskell requires that `main` is an `IO` action. Its type is written `IO ()`, which is an `IO` action that returns nothing of interest (but does some action when executed). We call this "Nothing of Interest" value **Unit**, and it's written like this: `()`

### Chap 4

- In Haskell, all the things you can write are called **pure**: definitions, values, expressions and functions, even the values that produce actions.
- **Pure** here means they are consistent, equational and express truths about value.
- **IO actions** describe some **action** on the input/output context
- Haskell has certain special functions and values that are marked as `IO` actions.
- In Haskell, the type of things that let us do this are made with a constructor named `IO`
- `IO` actions can be **executed**, which is what happens when their `IO` behaviour is activated - this is how programs are run

### Chap 5

- The process of making a function that returns a function itself is called **currying**.
- Haskell functions cannot take multiple arguments, only one each. However, what they can do is return another function.
- When we say a function of two arguments, it really means a function that produces a function that uses both their arguments in its inner body.

### Chap 6

- If you have an infix operator such as `(+)`, to use it as a prefix function, we can just wrap it in parentheses.
- A **section** is a partially applied operator. That means it has one of its two arguments supplied, and that becomes a function. It always use round brackets.

### Chap 7

- `Integer`, by the way, is the type of **unbounded whole numbers**
- the `maxBound` of `Int` is 2\*\*64-1
- `(+) :: Num a => a -> a -> a` : `a` is called **type variable**. That means it can be any type. `Num` is a typeclass.
- Type variables can be named anything
- `Num a =>` can be read as "a is constrained to types which are instances of the `Num` typeclass"
- A **typeclass** is a way of tagging **many** types so that we can have functions or values that work with many similar types that do similar things, but that are actually different.
- The `Float`, `Int`, and `Integer` types are all instances of the `Num` typeclass
- There's a typeclass called `Show` and this provides a single function: `show`, that can take any instance of `Show`, and makes a `String` version of it.
- `print` function can take a value whose type is any instance of `Show`

### Chap 9

- The kind of values with typeclass-constrained types, like the type `Num a => a`, are called **polymorphic values**
- The `:` operator is **binding** to the right.
- `print :: Show a => a -> IO ()`
- `length :: Foldable t => t a -> Int`
- `(++)` joins, or **concatenates**, two lists together.
- `String` type is itself just a list of type `Char`.
- `(++) :: [a] -> [a] -> [a]`

### Chap 10

- `(<=) :: Ord a => a -> a -> Bool`
- `map :: (a -> b) -> [a] -> [b]` This type says that map is a function of two arguments, the first one of which is a function.
- In Haskell `a -> b` means the types **can** be different, but don't have to.

### Chap 11

- We use `type` to tell Haskell we're defining a **type alias** (or **type synonym**)
- All types must start with a capital letter, and all variables names must start with a lowercase letter.
- `foldr` takes three arguments! The first is a function of two arguments: the folding function. The second is the base value, and the third is the list we're folding over.
- `foldr :: Foldable t => (a -> b -> b) -> b -> t a -> b`

### Chap 13

- the `data` keyword which creates a new data type, and specifies all the values it can possibly be. Another name for these kinds of types is a **sum type**.
- Making an import qualified means all the imports actually sit underneath a special name
- The type of `intercalate` is `[a] -> [[a]] -> [a]`

### Chap 14

- **product type** lets us make values that combine more than one type.
- Both **sum type** and **product type** are examples of **algebraic data type**
- You can have types that are sums of product types.
- `(.)` kind of glues two functions together. It turns them into a single function that does the same as calling the first one on the result of calling the second one.
- It's called the **function composition operator**.
- A function from `Data.List` module called `find` can be used to get a particular item from a List.
- The `qualified` keyword makes sure when it imports all the function names, it doesn't load them directly into our local **namespace**
- `Data.List.find :: Foldable t => (a -> Bool) -> t a -> Maybe a`
- `Ordering` is a built-in **sum** data type which has values of `LT`, `EQ` and `GT`. `Ordering` values are used by sorting functions in Haskell
- `Maybe` is a type that takes another type to make types with (this is called a **type constructor**). That means you can have values of type `Maybe Int`, `Maybe Cat`, `Maybe House`
- it's what we use in Haskell when we want to make a type's value optional.
- The way `Maybe` values work is there's a value called `Nothing` which represents "no value" of the wrapped type, and there's a value called `Just a` which represents any other value of our wrapped type.

### Chap 16

- All of the actions in an `IO` do block are executed in the order they're written. That is, they're sequenced together.
- Using `<-` we can connect an **inner value** from an `IO` action on its right to a variable on its left.
- We can only use this `<-` syntax within a `do` block
- The `do` block takes one or more actions, and packs them into a single action.
- We can get input from the user in Haskell with `getLine`. The type of `getLine` is `IO String`. When we use `<-`, we can think about it like it's extracting the `String` value from the action.
- `do` blocks must always end with the same type as the whole `do` block.

### Chap 17

- To build a single type out of a combination of other types, we use a **product type**
- `data Person = Person Name Name Year` The part to the left of the = is the **type name**. The part to the right of the = symbol is a **value constructor**
- `deriving (Show)` tells Haskell that we'd like to create an easy to print version of the data type for us.
- **record syntax** is simply a way to let us name the fields as we specify a data type.
- `filter :: (a -> Bool) -> [a] -> [a]`
- A **higher-order function** is a function that takes one or more functions as argument(s).
- The technical name for this process of getting rid of these variables that are repeated on the right hand side of the inside and outside is called **eta reduction**
- `sortBy` has a type of `(a -> a -> Ordering) -> [a] -> [a]` and `compare` has type `Ord a => a -> a -> Ordering`
- `flip` takes any 2-argument function, and it'll return a function that works exactly the same, but has its arguments swapped.
- `Data.List` has a `sortOn` function whose type is `Ord b => (a -> b) -> [a] -> [a]`
- `minimum :: (Ord a, Foldable t) => t a -> a`
- `maximum :: (Ord a, Foldable t) => t a -> a`
- There is a higher order function called `($)` that will take a function on the left of it, and some expression on the right, and apply the function to the expression. It will pretty much be applied last of all.
- `Data.List`'s `minimumBy :: Foldable t => (a -> a -> Ordering) -> t a -> a` function is another higher-order function that takes a function which is a comparing function `(a -> a -> Ordering)`.

### Chap 18

- `[3,5..12]` is what's called a **range**. It is every number between 3 and 12, in odd numbers.
- The range of numbers between 1 and 100, for example, would be represented as [1..100]
- The `repeat` function fives an infinite list of whatever its argument is.
- The `zip` function takes two lists, and builds pairs out of those lists, taking one item from each as it does.
- `concat [[1], [2], [3]]` results in `[1, 2, 3]`.
- When we use the `@` symbol, it's called an **as pattern**
- A `let` expression is another way we can define some locally scoped definitions.
- In a `do` block, name defined in a `let` expression will be available for the remainder of the `do` block.

### Chap 20

- The `return` function places a value into an `IO` action.

### Chap 22

- Other advanced topics include Monads, Combinators, the Y-Combinator, beta-reduction, Generalized Algebraic Data Types, etc.
- Y combinator's sole purpose is to allow one to do calculation in a functional language without any named variables or functions.
- The mathematical name for **folding** is **catamorphism**.
- **Anamorphism** are things that unfold.

- To avoid running out of memory, we have a version of `foldl` called `foldl'` that is strict — it forces the evaluation of `f` immediately at each step
- As a rule of thumb, you should use `foldr` on lists that might be infinite or where the fold is building up a data structure and use `foldl'` if the list is known to be finite and comes down to a single value.
- `foldr1` ("fold - R - one") and `foldl1` which dispenses with an explicit "zero" for an accumulator by taking the last element of the list instead.
- One reason that right-associative folds are more natural in Haskell than left-associative ones is that right folds can operate on infinite lists.
- A scan does both: it accumulates a value like a fold, but instead of returning only a final value it returns a list of all the intermediate values.
- `scanl` accumulates the list from the left, and the second argument becomes the first item in the resulting list. So, `scanl (+) 0 [1,2,3] = [0,1,3,6]`
- `scanl1` uses the first item of the list as a zero parameter. `scanl1 (+) [1,2,3] = [1,3,6]`
- `scanr (+) 0 [1,2,3] = [6,5,3,0]`
- `scanr1 (+) [1,2,3] = [6,5,3]`