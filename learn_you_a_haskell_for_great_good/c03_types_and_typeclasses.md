# Types and Typeclasses

## Believe the type

* **Static Type System:** Haskell determines the type of every expression at compile time. This prevents runtime crashes by catching type mismatches (e.g., dividing a boolean by a number) during compilation.
* **Type Inference:** While the system is strictly typed, Haskell can usually deduce the type of an expression or function automatically. Developers are not required to explicitly declare types for every value.
* **Type Labels:** A type is a category for an expression. Types are always **capitalized** (e.g., `Bool`, `Char`, `Integer`).
* **The :t Command:** In GHCi, the `:t` command followed by an expression displays its type signature.
* **The :: Operator:** This operator is read as "has type of."

```haskell
ghci> :t 'a'
'a' :: Char
ghci> :t "HELLO!"
"HELLO!" :: [Char]
```

* **Lists and Strings:** A `String` is represented as `[Char]` (a list of characters). The two are synonymous in Haskell.
* **Tuple Types:** Unlike lists, a tuple's type is determined by its specific length and the types of its individual components. An empty tuple `()` is also a type that contains exactly one value: `()`.
* **Function Type Signatures:** It is best practice to provide explicit type declarations for functions. The `->` operator separates parameters and the return type.
  * The parameters and the return type are all separated by `->`.
  * The last type in the sequence is the **return type**.
  * A function taking three `Int` values and returning one `Int` is written as `Int -> Int -> Int -> Int`.

```haskell
removeNonUppercase :: [Char] -> [Char]
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]

addThree :: Int -> Int -> Int -> Int
addThree x y z = x + y + z
```

* Note: To try out the above example, you will need to first write the functions into a standard `.hs` file before loading them into GHCi.
* To write them directly in GHCi, you can write it in a single line.

```haskell
ghci> let square :: Int -> Int; square x = x * x
ghci> square 5
25
```

* Alternatively, write them in a multi-line block (`:{` and `:}`).

```haskell
ghci> :{
| greet :: String -> String
| greet "Alice" = "Hello, Alice!"
| greet name    = "Hi, " ++ name
| :}
ghci> greet "Alice"
"Hello, Alice!"
```

* **Common Basic Types:**
  * **Int:** Bounded integer. *The text notes 32-bit limits (-2147483648 to 2147483647), but in modern 64-bit GHC implementations, Int is usually 64-bit.*
  * **Integer:** Unbounded (arbitrary-precision) integer. It can store numbers of any size but is less efficient than `Int`.
  * **Float:** Single-precision floating point.
  * **Double:** Double-precision floating point.
  * **Bool:** Boolean values (`True` or `False`).
  * **Char:** A single Unicode character, denoted by single quotes.

```haskell
ghci> factorial :: Int -> Int; factorial n = product [1..n]
ghci> factorial 20
2432902008176640000
ghci> factorial 50
-3258495067890909184
ghci>
ghci> factorial :: Integer -> Integer; factorial n = product [1..n]
ghci> factorial 20
2432902008176640000
ghci> factorial 50
30414093201713378043612608166064768844377641568960512000000000000
```

**Key takeaways:**

* **Type Signatures as Documentation:** Function type signatures (`Int -> Int`) serve as a contract and documentation that the compiler strictly enforces.
* **Precision vs. Performance:** Choose `Int` for standard arithmetic and `Integer` when you need to avoid overflow for massive calculations.
* **Uniform Function Syntax:** The use of `->` for both parameters and return types reflects Haskell's underlying functional nature (Currying), where every function technically takes one argument and returns a new function or a value.

## Type variables

* **Definition:** A type variable is a placeholder for any type. Unlike concrete types (e.g., `Int`, `Bool`), type variables start with a **lowercase letter** (usually `a`, `b`, `c`, etc.).
* **Polymorphic Functions:** Functions that utilize type variables in their signatures are called **polymorphic functions**. These are conceptually equivalent to **generics** in Java or C#.
* **The head Function:** The type signature `head :: [a] -> a` indicates that the function takes a list of any type `a` and returns a single element of that same type `a`.

```haskell
ghci> :t head
head :: [a] -> a
```

* **The fst Function:** The signature `fst :: (a, b) -> a` shows that the function accepts a tuple containing two potentially different types and returns a value of the first type.
  * Although `a` and `b` are different variables, they *can* represent the same type; the nomenclature simply allows them to be different.

```haskell
ghci> :t fst
fst :: (a, b) -> a
```

**Key takeaways:**

* **Generic Programming by Default:** Polymorphism in Haskell is more common than in most imperative languages. If a function doesn't need to know the specific details of a type (like `head` just grabbing the first item in a list), it uses a type variable to remain as general as possible.
* **Lowercase vs. Uppercase:** This is a strict syntactic rule. If it starts with a capital, it is a **concrete type**; if it starts with a lowercase letter, it is a **type variable**.
* **Type Consistency:** Even with type variables, Haskell maintains strict safety. If a function is defined as `[a] -> a`, the compiler ensures the returned element is guaranteed to be the same type as the elements stored in the input list.

## Typeclasses 101

* **Definition:** A typeclass is a sort of interface that defines a set of behaviors (functions). If a type is an instance of a typeclass, it must implement those behaviors.
* **Relationship to OOP:** Typeclasses are similar to **Java interfaces** rather than classes in object-oriented languages.
* **Class Constraints (`=>`):** In a type signature, the `=>` symbol denotes a constraint. For example, `(Eq a) =>` means "the type `a` must be an instance of the `Eq` typeclass."
* **Operators as Functions:** Operators like `==`, `+`, and `*` are functions. By default, they are infix. To use them in a prefix manner or check their type, they must be wrapped in parentheses, e.g., `(==)`.

```haskell
ghci> :t (==)
(==) :: (Eq a) => a -> a -> Bool

```

### Basic Typeclasses

* **Eq:** Used for types that support equality testing. Implements `==` and `/=`.
* **Ord:** Used for types that have an ordering. Implements functions like `>`, `<`, `>=` and `<=`.
  * The `compare` function takes two `Ord` members and returns an `Ordering` type: `GT` (Greater Than), `LT` (Lesser Than), or `EQ` (Equal).
  * A type must be a member of `Eq` before it can be a member of `Ord`.
* **Show:** Used for types that can be represented as strings. The `show` function converts a value into a `String`.
* **Read:** The inverse of `Show`. The `read` function takes a string and parses it into a type that is a member of `Read`.
  * **Type Annotations:** Because `read` can return multiple types, the compiler sometimes cannot infer the result. Use `::` to explicitly tell the compiler the desired type.
* **Enum:** Used for sequentially ordered types that can be enumerated (e.g., `Bool`, `Char`, `Int`).
  * Enables the use of list ranges like `['a'..'e']`.
  * Provides `succ` (successor) and `pred` (predecessor) functions.
* **Bounded:** Used for types that have a defined minimum and maximum limit.
  * `minBound` and `maxBound` act as polymorphic constants.
* **Num:** A numeric typeclass. Its members can act like numbers. Whole numbers in Haskell are polymorphic constants of type `(Num t) => t`.
* **Integral:** A sub-category of `Num` that includes only whole numbers (`Int` and `Integer`).
* **Floating:** A sub-category of `Num` that includes only floating-point numbers (`Float` and `Double`).

```haskell
-- Example of Type Annotation with Read
ghci> read "5" :: Int
5
ghci> read "5" :: Float
5.0

-- Using Bounded
ghci> minBound :: Int
-2147483648
```

*In the text, the author mentions the Int limit as -2147483648 on 32-bit machines. On modern 64-bit GHC implementations, the Int type is typically 64-bit, resulting in significantly larger bounds.*

Numerical Conversion:

* **fromIntegral:** A crucial function for interoperability between different numeric types.
* **Type Signature:** `fromIntegral :: (Num b, Integral a) => a -> b`.
* **Use Case:** Converting an `Int` (like the result of `length`) into a more general `Num` so it can be added to a `Float` or `Double`.

```haskell
-- This would fail without fromIntegral because you cannot add Int to Double
ghci> fromIntegral (length [1,2,3]) + 3.2
6.2
```

**Key takeaways:**

* **Polymorphic Constants:** Literals like `5` or `minBound` do not have a single fixed type; they are polymorphic and can "become" any type that satisfies the required typeclass constraint.
* **Ad-hoc Polymorphism:** Typeclasses allow you to write functions that work across different types (like `==` or `+`) as long as those types implement the necessary interface.
* **Explicit Disambiguation:** In cases where type inference is impossible (like parsing a string with `read`), you must use a type annotation (`:: Type`) to provide the compiler with the necessary context.
