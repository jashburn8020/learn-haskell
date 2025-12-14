# 1. Introduction

* This tutorial is specifically aimed at developers experienced in imperative languages (C, C++, Java, Python, etc.) who have no experience with purely functional languages.
* Haskell requires a fundamental shift in programming mindset, but crossing the initial conceptual barrier (the "click" moment) is highly rewarding.

## So what's Haskell?

* **Purely Functional Programming**
  * Programming shifts from giving the computer a sequence of tasks (imperative flow) to telling it *what* things are (e.g., "factorial is the product of all numbers from 1 to *n*").
  * **Immutability** is enforced: once a variable is defined (e.g., `a` is 5), it cannot be reassigned or mutated later.
  * Functions have no **side effects**; the only thing a function does is calculate and return a result.
  * This results in **Referential Transparency**: a function called with the same parameters is guaranteed to return the exact same result every time. This aids in program correctness, deduction, and compiler reasoning.

* **Lazy Evaluation**
  * Haskell will not execute functions or calculate results until it is absolutely forced to show a result.
  * This enables programs to be thought of as a series of transformations on data, where only the required computations are performed.
  * A significant practical consequence is the ability to work with **infinite data structures** efficiently.

* **Statically Typed**
  * The compiler knows the type of every piece of code (number, string, etc.) at compile time, catching many errors early.
  * The language features powerful **type inference**, meaning developers often do not need to explicitly label every piece of code with a type.
  * Type inference encourages more generalized code, as functions will automatically work across any parameter types that satisfy the necessary constraints (e.g., any two types that support addition).

* **Elegant and Concise**
  * Haskell programs are typically shorter than their imperative equivalents due to the use of high-level functional concepts.
  * Shorter code is easier to maintain and tends to have fewer bugs.

### What you need to dive in

* You need a text editor and a Haskell compiler.
* The most widely used compiler is **GHC** (Glasgow Haskell Compiler).
* The recommended approach for learning is to use the interactive mode, **GHCi**:
  * Invoke by typing `ghci` at the prompt.
  * Load Haskell files (`.hs`) using the command `:l myfunctions`.
  * To quickly recompile and reload a file after making changes, use the command `:r`.

---

### Key Takeaways

* **Mindset Shift:** The core conceptual hurdle is moving from state manipulation to defining data relationships using purely functional, immutable values.
* **Trust the Function:** Due to **Referential Transparency**, you can rely on a function's output given its input, as there are no side effects to worry about.
* **Execution is Deferred:** **Lazy evaluation** means that code execution is demand-driven, a critical difference from imperative languages that execute statements immediately.
* **Strong, Smart Safety:** Haskell's **statically typed system** uses **type inference** to provide compile-time safety without requiring verbose type annotations.
