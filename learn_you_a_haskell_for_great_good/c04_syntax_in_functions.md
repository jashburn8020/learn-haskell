# Syntax in Functions

## Pattern matching

* **Definition:** Pattern matching involves specifying patterns that data should conform to, then deconstructing that data according to those patterns. It allows for defining separate function bodies for different data shapes.
* **Evaluation Order:** Patterns are checked from **top to bottom**. Once a match is found, the corresponding body is executed.
* **Specificity:** Always place the most specific patterns (e.g., constants like `0` or `7`) above general ones (variables like `x` or `n`). Placing a "catch-all" variable pattern at the top will intercept all inputs, preventing more specific patterns from ever being reached.
* **Exhaustiveness:** Functions must account for all possible inputs of a given type. Failing to provide a pattern for a specific input results in a `Non-exhaustive patterns` runtime exception.
* **Catch-all Patterns:** It is standard practice to include a final variable pattern (like `x`) or a wildcard pattern (`_`) to handle unexpected or remaining cases.

```haskell
lucky :: (Integral a) => a -> String
lucky 7 = "LUCKY NUMBER SEVEN!"
lucky x = "Sorry, you're out of luck, pal!"
```

* **Recursion with Patterns:** Pattern matching is the primary way to define recursive functions by specifying a "base case" (edge condition) and a "recursive step."

```haskell
factorial :: (Integral a) => a -> a
factorial 0 = 1
factorial n = n * factorial (n - 1)
```

* **Tuple Matching:** Patterns can deconstruct tuples directly in the function's parameter list. This replaces the need for accessor functions like `fst` or `snd`.
* **Wildcard (`_`):** Used to match and ignore specific parts of a data structure.

```haskell
-- Custom accessors for triples (3-tuples)
first :: (a, b, c) -> a
first (x, _, _) = x

addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)
addVectors (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)
```

* **List Matching:** Lists can be matched using specific syntax:
  * `[]` matches an empty list.
  * `(x:xs)` matches a list with at least one element, binding the head to `x` and the tail to `xs`.
  * `(x:y:zs)` matches a list with at least two elements.
  * `[x,y,z]` is syntactic sugar for `x:y:z:[]`.
* **List Comprehensions:** Pattern matching can be used within the generator of a list comprehension. If an element fails to match the pattern, it is simply skipped rather than throwing an error.

```haskell
ghci> [a+b | (a,b) <- [(1,3), (4,3)]]
[4,7]
```

* **The `error` Function:** Used to trigger a runtime crash with a custom message. While useful for "impossible" states (like calling `head` on `[]`), it should be used sparingly in production code.
* **As-patterns (`@`):** Used to break an item into its components while still maintaining a reference to the original, un-deconstructed value. Syntax: `name@pattern`.

```haskell
capital :: String -> String
capital "" = "Empty string, whoops!"
capital all@(x:xs) = "The first letter of " ++ all ++ " is " ++ [x]
```

* **Restrictions:** You **cannot** use the concatenation operator (`++`) in a pattern match (e.g., `(xs ++ ys) = ...` is invalid) because it is ambiguous how the list should be split.

**Key takeaways:**

* **Deconstruction over Accessors:** Instead of using "getter" methods or functions, Haskell developers use pattern matching to pull values out of structures (tuples, lists) at the moment of function application.
* **Edge Case Safety:** Patterns force developers to think about base cases (like the empty list) and recursive cases as distinct logical branches.
* **Interchangeability:** Because `[1,2]` is just `1:2:[]`, you can use either the bracket syntax for fixed lengths or the colon syntax for head/tail processing interchangeably in your patterns.

## Guards, guards!

* **Definition:** Guards are a way of testing properties of values (or combinations of values) using Boolean expressions. They serve a similar purpose to `if-else` trees in imperative languages but offer significantly better readability.
* **Syntax:**
  * Guards are indicated by a pipe character `|` following the function name and its parameters.
  * Each guard is followed by an expression that must evaluate to a `Bool`.
  * If the expression evaluates to `True`, the corresponding function body is executed.
  * **Crucial Syntax Rule:** Unlike standard function definitions, there is **no equals sign (`=`)** between the function parameters and the first guard.
* **Evaluation Flow:** Haskell evaluates guards sequentially from top to bottom. The first guard that evaluates to `True` determines the result.
* **The `otherwise` Keyword:** Often used as the final guard, `otherwise` is simply a variable defined as `True`. It acts as a catch-all for any cases not caught by previous guards.
* **Fall-through Behavior:** If all guards in a specific pattern match evaluate to `False`, the compiler "falls through" to the next pattern defined for that function. If no patterns match and no `otherwise` guard is present, a runtime error is thrown.

```haskell
densityTell :: (RealFloat a) => a -> a -> String
densityTell mass volume
    | mass / volume < 1.2 = "Wow! You're going for a ride in the sky!"
    | mass / volume <= 1000.0 = "Have fun swimming, but watch out for sharks!"
    | otherwise = "If it's sink or swim, you're going to sink."
```

* **Infix Definitions:** Functions can be defined using infix notation with backticks, which can improve readability when using guards to compare two values.

```haskell
myCompare :: (Ord a) => a -> a -> Ordering
a `myCompare` b
    | a > b     = GT
    | a == b    = EQ
    | otherwise = LT
```

* **Inline Guards:** While syntactically valid to write guards on a single line, it is discouraged as it degrades code legibility.

### Combining both

* In professional code, you usually use Pattern Matching to *get the data out of its box*, and Guards to *decide what to do with that data*.

```haskell
-- Pattern match to get 'x' and 'xs',
-- then use a guard to check the value of 'x'
specialFilter :: [Int] -> String
specialFilter [] = "Empty"
specialFilter (x:xs)
    | x > 100   = "Starts with a huge number"
    | even x    = "Starts with an even number"
    | otherwise = "Starts with a small odd number"
```

**Key takeaways:**

* **Patterns vs. Guards:** Patterns check the **structure** of the data (deconstruction), while guards check **properties** of the data (Boolean logic). They are often used in tandem to handle complex branching logic.
* **Boolean Logic:** Because guards are expressions that return a `Bool`, you can use any existing function or logic (like `mass / volume`) within the guard itself to perform calculations on the fly.
* **Visual Alignment:** Guards are typically indented and vertically aligned, providing a clean, table-like overview of conditional logic that is easier to scan than nested `if` statements.

## Where!?

* **Local Bindings:** The `where` keyword allows you to bind variables or functions to expressions at the end of a function definition. These bindings are accessible across all **guards** of that function.
* **DRY Principle:** It prevents code duplication by allowing you to calculate an expression once and reference it multiple times, improving both maintainability and readability.
* **Scope:**
  * Variables defined in a `where` block are only visible within that specific function.
  * `where` bindings are **not** shared across different pattern matches of the same function. If multiple patterns need access to the same constant, it must be defined at a higher (global) scope.
* **Syntax and Layout:** Haskell uses indentation to determine the extent of a `where` block. All defined names must be aligned in the same column for the compiler to recognize them as part of the same block.

```haskell
densityTell :: (RealFloat a) => a -> a -> String
densityTell mass volume
    | density < air = "Wow! You're going for a ride in the sky!"
    | density <= water = "Have fun swimming, but watch out for sharks!"
    | otherwise   = "If it's sink or swim, you're going to sink."
    where density = mass / volume
          air = 1.2
          water = 1000.0
```

* **Pattern Matching in `where`:** You can use **pattern matching** within a `where` clause to deconstruct tuples, lists, or other data structures.

```haskell
initials :: String -> String -> String
initials firstname lastname = [f] ++ ". " ++ [l] ++ "."
    where (f:_) = firstname
          (l:_) = lastname
```

* **Local Functions:** Beyond simple constants, you can define full functions within a `where` block. This is particularly useful for helper functions used in **list comprehensions**.

```haskell
calcDensities :: (RealFloat a) => [(a, a)] -> [a]
calcDensities xs = [density m v | (m, v) <- xs]
    where density mass volume = mass / volume
```

* **Nesting:** `where` bindings can be nested; a function defined in a `where` clause can have its own `where` clause for its own internal helper functions.

**Key takeaways:**

* **Syntactic Placement:** Unlike `let` expressions (which you may encounter later), `where` bindings are placed at the end of a function, allowing the main logic to be presented first.
* **Granular Scope:** `where` provides a clean way to encapsulate "helper" logic that is only relevant to a specific function, keeping the global namespace clean.
* **Indentation Matters:** Proper vertical alignment of names in a `where` block is mandatory for the Haskell compiler to correctly parse the local definitions.

## Let it be

* **Definition:** `let` bindings allow you to bind values to names anywhere an expression is expected. Unlike `where` bindings, which are a syntactic construct attached to a function body, `let` is an **expression** itself.
* **Syntax:** The basic form is `let <bindings> in <expression>`. The names defined in the `let` section are only accessible within the expression following the `in` keyword.
* **Layout and Alignment:** Multiple bindings within a single `let` block must be vertically aligned in the same column. For inline multiple bindings, use **semicolons** as separators.

```haskell
cylinder :: (RealFloat a) => a -> a -> a
cylinder r h =
    let sideArea = 2 * pi * r * h
        topArea = pi * r ^2
    in  sideArea + 2 * topArea
```

* **Expression Nature:** Because `let` bindings are expressions, they can be embedded into other expressions (like `if` statements) or used to define local functions.

```haskell
-- Using let inside a calculation
ghci> 4 * (let a = 9 in a + 1) + 2
42

-- Defining a local function
ghci> [let square x = x * x in (square 5, square 3, square 2)]
[(25,9,4)]
```

* **Pattern Matching:** Like other binding constructs in Haskell, `let` supports pattern matching for deconstructing data structures like tuples.

```haskell
ghci> (let (a,b,c) = (1,2,3) in a+b+c) * 100
600
```

* **List Comprehensions:** `let` can be used inside list comprehensions to bind names to intermediate results.
  * In this context, the `in` keyword is **omitted**.
  * The bound names are visible to the output function (before the `|`) and all subsequent predicates/generators.
  * They act as bindings rather than filters (predicates).

```haskell
calcDensities :: (RealFloat a) => [(a, a)] -> [a]
calcDensities xs = [density | (m, v) <- xs, let density = m / v, density < 1.2]
```

* **GHCi Usage:** In the interactive environment, you can use `let` without the `in` part to define constants or functions that persist throughout the entire session.

**Key takeaways:**

* **Scope Limitation:** While `where` bindings are visible across all guards in a function, `let` bindings are very local and **cannot be used across guards**.
* **Let vs. Where:** Use `let` when you need a local expression-level binding; use `where` for function-level declarations that need to be shared across guards or to keep the main function logic at the top of the definition.
* **Expression-First:** The primary advantage of `let` is its status as a first-class expression, allowing for high granularity in where logic is defined and executed.

## Case expressions

* **Definition:** `case` expressions allow for pattern matching against a value anywhere within an expression. While similar to `switch` statements in imperative languages, Haskell's `case` constructs are **expressions** that evaluate to a specific result.
* **Syntactic Sugar:** Pattern matching in function definitions is actually syntactic sugar for `case` expressions. The compiler translates multiple function body patterns into a single `case` block.

```haskell
-- Function-level pattern matching
head' :: [a] -> a
head' [] = error "No head for empty lists!"
head' (x:_) = x

-- Equivalent implementation using a case expression
head' :: [a] -> a
head' xs = case xs of [] -> error "No head for empty lists!"
                      (x:_) -> x
```

* **Syntax:**

```haskell
case expression of pattern -> result
                   pattern -> result
                   pattern -> result
```

* **Evaluation:** The `expression` is matched against patterns sequentially. The first successful match determines the return value of the entire block. If no pattern matches, the program throws a runtime error.
* **Versatility:** Because they are expressions, `case` blocks can be used in the middle of other operations, such as string concatenation or within other functional compositions.

```haskell
describeList :: [a] -> String
describeList xs = "The list is " ++ case xs of [] -> "empty."
                                               [x] -> "a singleton list."
                                               xs -> "a longer list."
```

* **Indentation:** Proper alignment is required for the patterns following the `of` keyword to be parsed correctly by the compiler.

**Key takeaways:**

* **Expressions vs. Statements:** Unlike the `switch` statement in C-style languages, a `case` in Haskell is an expression that must return a value. This allows it to be used as a "right-hand side" value in any definition.
* **Foundational Pattern Matching:** Understanding `case` expressions is fundamental because they are the underlying mechanism for all pattern matching in the language, including that used in function headers.
* **Mid-expression Deconstruction:** `case` is the preferred tool when you need to deconstruct a data structure (like a list or tuple) in the middle of a larger computation without defining a separate helper function.
