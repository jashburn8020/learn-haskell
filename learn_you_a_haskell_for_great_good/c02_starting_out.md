# 2. Starting Out

## Ready, set, go

* **GHCi (REPL):** The interactive environment for Haskell. Start it by typing `ghci` in the terminal.
  * Load file definitions with `:l filename`.
  * Reload a changed file with `:r`.
* **Arithmetic:** Standard operators (`+`, `-`, `*`, `/`) follow usual mathematical precedence.
* **Negation Pitfall:** To denote a negative number in an expression, surround it with parentheses (e.g., `5 * (-3)`) to avoid the compiler interpreting the minus sign as a binary operator.
* **Boolean Logic:**
  * `&&` for AND, `||` for OR.
  * `not` for negation.
  * Equality is checked with `==`, while inequality uses `/=` (not `!=`)
* **Static Typing:** Haskell enforces strict type matching. You cannot compare or operate on differing types (e.g., `5 == True` or `5 + "llama"`) without triggering a compile-time error.
* **Numerical Flexibility:** Integer literals (like `5`) are polymorphic and can act as floating-point numbers if the operation requires it (e.g., `5 + 4.0`).

### Function Application

* **Syntax:** Unlike imperative languages that use `f(x, y)`, Haskell uses spaces: `f x y`.
* **Prefix vs. Infix:** * Most functions are **prefix** (e.g., `min 9 10`).
  * Mathematical operators are **infix** (e.g., `2 + 2`).
  * **Infix Conversion:** Any prefix function taking two arguments can be used as an infix operator by wrapping it in backticks.
    * Example: ``92 `div` 10`` is equivalent to `div 92 10`.
* **Precedence:** **Function application has the highest precedence** of all operations.
  * `succ 9 + 1` is interpreted as `(succ 9) + 1`, not `succ (9 + 1)`.
* **Parentheses:** Use parentheses only to enforce evaluation order, not to denote a function call.
  * Imperative `bar(bar(3))` becomes Haskell `bar (bar 3)`.

**Key takeaways:**

* **Spaces over Parentheses:** For an OO developer, the most important habit to break is using `f(x)`. In Haskell, `f (x)` means "apply function `f` to the result of expression `x`", and `f x` is the standard call.
* **High-Precedence Application:** Always remember that functions "grab" their arguments before operators like `+` or `*` execute.
* **Type Homogeneity:** Operations and comparisons require identical types. The compiler will not implicitly cast a `String` to a `Number`.

## Baby's first functions

* **Function Definition Syntax:** Functions are defined by their name, followed by space-separated parameters, an equals sign, and the function body.
  * Example: `doubleMe x = x + x`
  * No parentheses are required around parameters during definition or application.
* **Workflow in GHCi:**
  * Save functions in a `.hs` file (e.g., `baby.hs`).
  * Load the file into the interpreter using `:l baby`.
  * Once loaded, functions can be invoked directly in the terminal.
* **Function Composition:** Haskell encourages building complex logic by combining simple, atomic functions.

```haskell
doubleMe x = x + x
doubleUs x y = doubleMe x + doubleMe y
```

* **Definition Order:** The order in which functions are defined in a source file does not matter. A function can call another function even if it is defined later in the file.
* **The `if` Expression:** Unlike imperative languages where `if` is a statement used for control flow, in Haskell, `if` is an **expression**.
  * **Mandatory `else`:** Because an expression must always return a value, the `else` clause is required.
  * Every `if` expression must result in a value that can be used in further computations.

```haskell
doubleSmallNumber x = if x > 100 then x else x*2
```

* **Naming Conventions:**
  * **Lowercase Requirement:** Function and variable names **must** start with a lowercase letter.
  * **The "Tick" (`'`):** The apostrophe is a valid character in names. It is conventionally used to denote a slightly modified version of a function or a "strict" (non-lazy) version.
  * **Definitions:** A function with zero parameters is called a **definition** (or a name). Because values are immutable, the name and its assigned value are strictly interchangeable.

**Key takeaways:**

* **Expressions over Statements:** In Haskell, almost everything is an expression. The `if` construct is a prime example: it evaluates to a result rather than just branching execution.
* **Immutability and Names:** Zero-argument functions are essentially constants. Since data cannot change, these names are persistent references to their evaluated results.
* **Syntactic Conciseness:** By removing parentheses and commas from function application, Haskell reduces "boilerplate" noise, allowing developers to focus on the mathematical relationships between data.

## An intro to lists

* **Homogeneity:** Lists in Haskell are **homogenous** data structures; every element must be of the same type.
* **Strings as Lists:** A `String` is simply a list of characters (`[Char]`). Functions that work on lists also work on strings.
* **List Construction and Concatenation:**
  * `++`: Joins two lists. This operation is **O(n)** where *n* is the length of the list on the left, as Haskell must traverse the entire first list.
    * Example: `[1,2,3,4] ++ [9,10,11,12]`, `"hello" ++ " " ++ "world"`
  * `:` (**cons operator**): Prepends an element to a list. This is an **O(1)** (instantaneous) operation.
    * Example: `5:[1,2,3,4,5]`
  * Syntactic Sugar: `[1,2,3]` is internally represented as `1:2:3:[]`.
* **Indexing:** Use the `!!` operator for 0-based indexing. Accessing an index out of bounds results in a runtime error.

```haskell
ghci> "Steve Buscemi" !! 6
'B'
ghci> [9.4, 33.2, 96.2] !! 1
33.2
```

* **Nested Lists:** Lists can contain other lists. While the inner lists can have different lengths, they must all contain the same type of data.
  * Example: `[[1,2,3], [4,5,6,7]]`
* **List Comparison:** Lists can be compared using `>`, `>=`, `<`, and `<=` if the elements within them are comparable. Comparisons are performed in **lexicographical order** (element by element starting from the head).

```haskell
ghci> [3,2,1] > [2,2,1]
True
ghci> [3,4,-1] > [3,4]
True
ghci> [4,3,2] < [4,3,1]
False
ghci> [1,2,3] == [1,2,3]
True
```

* **Basic List Functions:**
  * **Extraction:**
    * `head`: Returns the first element.
    * `tail`: Returns the list minus its first element.
    * `last`: Returns the final element.
    * `init`: Returns everything except the last element.
    * **Safety Warning:** Calling these on an empty list (`[]`) causes a runtime exception that cannot be caught at compile time.
  * **Information and Search:**
    * `length`: Returns the number of elements.
    * `null`: Returns `True` if a list is empty. Use this function instead of `a_list == []`.
    * `elem`: Usually used as an infix function (e.g., ``4 `elem` [1,2,3,4]``) to check if a value exists in the list.
  * **Transformation:**
    * `reverse`: Reverses the list order.
    * `take n`: Extracts the first *n* elements.
    * `drop n`: Removes the first *n* elements and returns the remainder.
  * **Aggregation:**
    * `maximum` / `minimum`: Returns the largest or smallest element.
    * `sum` / `product`: Calculates the sum or product of a numeric list.

```haskell
ghci> head [1,2,3,4,5]
1
ghci> tail [1,2,3,4,5]
[2,3,4,5]
ghci> last [1,2,3,4,5]
5
ghci> init [1,2,3,4,5]
[1,2,3,4]
ghci> -- head, tail, last and init throw a runtime exception if used on empty lists
ghci> head []
*** Exception: Prelude.head: empty list
CallStack (from HasCallStack):
  error, called at libraries/base/GHC/List.hs:1644:3 in base:GHC.List
  errorEmptyList, called at libraries/base/GHC/List.hs:87:11 in base:GHC.List
  badHead, called at libraries/base/GHC/List.hs:83:28 in base:GHC.List
  head, called at <interactive>:33:1 in interactive:Ghci22
ghci> length [1,2,3,4,5]
5
ghci> null [1,2,3,4,5]
False
ghci> null [[]]
False
ghci> null []
True
ghci> reverse [1,2,3,4,5]
[5,4,3,2,1]
ghci> take 3 [1,2,3,4,5]
[1,2,3]
ghci> take 9 [1,2,3,4,5]
[1,2,3,4,5]
ghci> take 0 [1,2,3,4,5]
[]
ghci> take 3 []
[]
ghci> drop 3 [1,2,3,4,5]
[4,5]
ghci> drop 9 [1,2,3,4,5]
[]
ghci> drop 0 [1,2,3,4,5]
[1,2,3,4,5]
ghci> drop 3 []
[]
ghci> maximum [1,2,3,4,5]
5
ghci> minimum [1,2,3,4,5]
1
ghci> sum [1,2,3,4,5]
15
ghci> product [1,2,3,4,5]
120
ghci> 3 `elem` [1,2,3,4,5]
True
ghci> 9 `elem` [1,2,3,4,5]
False
```

**Key takeaways:**

* **Performance Awareness:** Prefer the cons operator (`:`) over concatenation (`++`) when building lists, as appending to the end of a list becomes increasingly expensive as the list grows.
* **Singly Linked Nature:** Haskell lists behave like singly linked lists. This explains why `head` and `:` are fast, while `last` and `++` require traversing the entire structure.
* **Partial Functions:** Functions like `head` and `tail` are "partial," meaning they don't return a value for all possible inputs (specifically empty lists). Professional Haskell code often uses safer alternatives or pattern matching to avoid these runtime crashes.

## Texas ranges

* **Definition:** Ranges are a concise syntax for generating lists that form arithmetic sequences. They work with any type that can be enumerated (e.g., integers, characters).
* **Basic Syntax:** Use `[start..end]` to generate a sequence inclusive of both boundaries.
  * Example: `[1..20]` produces a list from 1 to 20.
  * Example: `['a'..'z']` produces the lowercase alphabet.
* **Stepped Ranges:** To specify a common difference other than 1, provide the first two elements followed by the upper limit: `[first, second..limit]`.
* Haskell calculates the step based on the difference between the first and second elements.
  * Example: `[2,4..20]` produces even numbers up to 20.
  * Example: `[3,6..20]` produces multiples of 3 up to 18.
* **Reverse Ranges:** To count down, you **must** specify a step. Writing `[20..1]` will return an empty list because the default increment is +1. Use `[20,19..1]` instead.
* **Floating Point Caution:** Avoid using floating-point numbers in ranges. Due to IEEE 754 precision limitations, the results can be unpredictable.

```haskell
ghci> [0.1, 0.3 .. 1]
[0.1,0.3,0.5,0.7,0.8999999999999999,1.0999999999999999]
```

* **Infinite Lists:** By omitting the upper limit, you create an infinite list.
  * Haskell’s **lazy evaluation** ensures the computer does not crash or hang; it only calculates the portion of the list that is explicitly requested.
  * Example: `take 24 [13,26..]` efficiently retrieves the first 24 multiples of 13.
* **Infinite List Functions:**
  * `cycle`: Takes a list and repeats its elements infinitely.
  * `repeat`: Takes a single element and repeats it infinitely.
  * `replicate`: A non-infinite alternative that creates a list of a specific length with a repeated value (e.g., `replicate 3 10` returns `[10,10,10]`).

```haskell
ghci> take 10 (cycle [1,2,3])
[1,2,3,1,2,3,1,2,3,1]

ghci> take 10 (cycle "lol ")
"lol lol lo"

ghci> take 5 (repeat 7)
[7,7,7,7,7]

ghci> replicate 10 5
[5,5,5,5,5,5,5,5,5,5]
```

* Ranges are powered by a typeclass called `Enum`. Anything that implements the `Enum` typeclass -- meaning its values can be mapped to an integer sequence -- can be used in a range.
* The Haskell compiler actually translates range syntax into specific functions from the `Enum` typeclass.

| Syntax     | Internal Function      | Description                                         |
| ---------- | ---------------------- | --------------------------------------------------- |
| `[x..]`    | `enumFrom x`           | An infinite list starting at `x`.                   |
| `[x..y]`   | `enumFromTo x y`       | A list from `x` up to (and including) `y`.          |
| `[x,y..]`  | `enumFromThen x y`     | An infinite list with a step (step size = `y - x`). |
| `[x,y..z]` | `enumFromThenTo x y z` | A stepped list that ends at `z`.                    |

**Key takeaways:**

* **Arithmetic Only:** Ranges only support linear arithmetic progressions. You cannot use them for geometric sequences (like powers of 2) using the `[1,2,4..]` syntax.
* **Lazy Evaluation is Native:** The ability to define an infinite list and only "realize" a small part of it is a core feature of Haskell, allowing for more declarative and decoupled code.
* **Enumeration Requirement:** Ranges only work on types that belong to the `Enum` typeclass (anything that has a clear "next" and "previous" value).

## I'm a list comprehension

* **Mathematical Origin:** Based on mathematical set-builder notation. For example, the set of the first ten even natural numbers -- $S = \lbrace\ 2 \cdot x \mid x \in \mathbb{N},\ x \le 10\ \rbrace$ -- is mirrored in Haskell syntax.
* **Basic Syntax:** `[ output_expression | variable <- input_list, predicate ]`
  * The **output function** (before the `|`) defines how to transform the elements.
  * The **generator** (`x <- [1..10]`) binds elements from an input list to a variable.
  * The **predicate** (after the generator) acts as a filter.

```haskell
ghci> [x*2 | x <- [1..10]]
[2,4,6,8,10,12,14,16,18,20]
```

* **Filtering with Predicates:** You can narrow down the input list by adding boolean conditions. Multiple predicates are separated by commas and must all evaluate to `True` for an element to be included.

```haskell
-- Numbers 50-100 where (x mod 7) is 3
ghci> [ x | x <- [50..100], x `mod` 7 == 3]
[52,59,66,73,80,87,94]

-- Multiple predicates: 10-20 excluding 13, 15, and 19
ghci> [ x | x <- [10..20], x /= 13, x /= 15, x /= 19]
[10,11,12,14,16,17,18,20]
```

* **Multiple Generators:** When a comprehension draws from multiple lists, it produces a **Cartesian product** (all possible combinations) of those lists.
* The rightmost generator varies the fastest.
* A comprehension with two lists of length *N* and *M* results in a list of length *N* x *M*.

```haskell
-- Combining adjectives and nouns
ghci> [adj ++ " " ++ noun | adj <- ["lazy","pope"], noun <- ["hobo","frog"]]
["lazy hobo","lazy frog","pope hobo","pope frog"]
```

* **The Wildcard Pattern (`_`):** Used when you need to draw from a list but do not intend to use the actual value of the elements.
  * Example: Implementing a custom length function by replacing every element with `1` and summing the result.

```haskell
length' xs = sum [1 | _ <- xs]
```

* **Strings and List Comprehensions:** Because strings are character lists, comprehensions are an efficient way to process text.

```haskell
-- Remove all characters except uppercase letters
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]
```

* **Nested List Comprehensions:** Used for processing lists of lists (multidimensional arrays). You can nest a comprehension inside the output expression of another.

```haskell
-- Remove odd numbers from a list of lists without flattening
ghci> let xxs = [[1,3,2],[1,2,3,4,5]]
ghci> [ [ x | x <- xs, even x ] | xs <- xxs]
[[2],[2,4]]
```

**Key takeaways:**

* **Declarative Filtering:** List comprehensions replace the "initialize list, loop, if-check, append" pattern found in imperative languages with a single, declarative expression.
* **Combinatorial Logic:** Multiple generators automatically handle nested iteration, making them highly effective for generating permutations or combinations.
* **Uniformity:** Since strings and lists are treated identically, the same comprehension logic applies to data processing, mathematical sets, and string manipulation.

## Tuples

* **Definition:** Tuples are used to store a fixed number of elements as a single value.
* **Heterogeneity:** Unlike lists, tuples can contain a combination of different types (e.g., a string and an integer, `("one", 2)`).
* **Type Rigidity:** The type of a tuple is defined by its length and the specific types of its components.
  * A pair (2-tuple) of type `(Int, Int)` is a distinct type from a triple (3-tuple) of type `(Int, Int, Int)`.
  * You cannot have a list containing both pairs and triples, such as `[(1,2), (8,11,5)]`, as it violates list homogeneity.
* **Use Case:** Use tuples when the number of components is known in advance and fixed (e.g., coordinates or a name-age pair).
* **Constraints:** * There is no such thing as a singleton tuple; a tuple must contain at least two elements.
  * Comparison (`==`, `<`, etc.) is only possible between tuples of the same size and component types.
* **Accessing Data (Pairs Only):** Haskell provides two specific functions for extracting values from pairs (2-tuples):
  * `fst`: Returns the first component.
  * `snd`: Returns the second component.

```haskell
ghci> fst (8, 11)
8
ghci> snd ("Wow", False)
False
```

* **The zip Function:** Takes two lists and joins them into a single list of pairs.
  * If lists are of unequal length, the longer list is truncated to match the shorter one.
  * Due to **lazy evaluation**, `zip` can be used to pair a finite list with an infinite list.

```haskell
ghci> zip [1,2,3] ["one", "two", "three"]
[(1,"one"),(2,"two"),(3,"three")]

ghci> zip [1..] ["apple", "orange", "cherry"]
[(1,"apple"),(2,"orange"),(3,"cherry")]
```

* **Problem Solving Pattern:** Tuples are frequently used within **list comprehensions** to solve combinatorial problems. The idiomatic functional approach involves:
  1. Generating a starting set of solutions (a "raw" list of tuples).
  2. Applying transformations and filters (predicates) to narrow down the results.

```haskell
-- Finding a right triangle with sides <= 10 and perimeter of 24
-- Starting set of solutions
triangles = [ (a,b,c) | c <- [1..10], b <- [1..10], a <- [1..10] ]
-- Transform and filter
rightTriangles = [ (a,b,c) | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2]
-- Filter again
rightTriangles' = [ (a,b,c) | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2, a+b+c == 24]
```

**Key takeaways:**

* **Structure vs. Collection:** Think of lists as dynamic-length collections for one type of data, and tuples as fixed-structure "records" or "structs" for related data of potentially different types.
* **Type Safety:** Because the tuple's size is baked into its type, the compiler prevents common errors like passing a 3D coordinate to a function expecting a 2D coordinate.
* **Declarative Transformation:** Instead of using nested loops and mutable state to find a specific data point, Haskell developers use list comprehensions to generate all possibilities and filter them based on logical constraints.
