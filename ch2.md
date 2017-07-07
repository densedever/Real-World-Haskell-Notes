# Types and Functions

## Why Care About Types?

Every expression and function has a type - they share properties with other values with the same type. We can add numbers and concatenate lists. `True` has type Bool, `"foo"` has type String.

Why do things have types? To add abstract interfaces to raw bytes to set them apart as strings, numbers, or airline reservations. They also prevent us from mixing up types so your hotel reservation can't be used as a car rental receipt.

This abstraction lets us ignore implementation details so things work like you assume they will.

Not all type systems are equal, and color the way you use and think about the language. Haskell's type system focuses on abstraction, and lets us write concise, powerful code.

## Haskell's Type System

Haskell types can be *strong*, *static*, and *automatically inferred*. Let's talk more about the strengths/weaknesses of these types and their similarities to other languages.

### Strong Types

Strong type systems prevent you from writing things that don't make sense, like using an integer as a function. Haskell compilers will reject passing in strings that require functions.

An expression obeying type rules is *well-typed*, otherwise it's *ill-typed*and causes a *type error*.

Haskell won't coerce one type to another, you have to use conversion functions. C will silently cast ints to doubles in certain expressions, for example, whereas Haskell will simply throw an error.

Strong typing may make it difficult to write certain code, but will save you from nasty and hard-to-spot coercion errors in return. Such an error is what brought down the Challenger, for example.

### Weaker and Stronger Types

*The notion of "strong" typing means that fewer expressions are valid than in "weakly" typed expressions. Perl says `"foo" + 2` equals 2, but Haskell will just throw an error.*


### Static Types

A *static* type system means that types will be checked at compile-time and
invalid expressions will be treated as errors. Mismatched types will be caught
before our code is even run:

```
ghci> True && "false"
<interactive>:1:8:
    Couldn't match expected type 'Bool' against inferred type '[Char]'
    In the second argument of '(&&)', namely '"false"'
    In the expression: True && "false"
    In the definition of 'it': it = True && "false"
```

`"false"` is a string, and so can't be compared with a boolean because `(&&)` requires both its operands to be booleans.

"Duck typing" or "dynamic typing" says that some types look and act enough
like each other that they can be treated similarly. Haskell provides this
functionality through *typeclasses* in a safe and robust manner.

Haskell's strong, static type system make entire classes of errors impossible
up-front. This differs from other languages in that if the code compiles, it's
much more likely to work correctly (it has fewer trivial bugs).

Dynamically-typed languages need large amounts of type checks, and tests often
don't cover everything. Refactoring sometimes introduces more errors that
testing might not pick up.

A Haskell program that compiles will have no type errors, and refactoring is
just moving code around. In dynamic languages, refactoring is a process of constantly checking values and types and making pieces fit. 

### Type Inference

A Haskell compiler can deduce the types of all data through *type inference*. You can explicitly declare values, but you don't have to.

## What to Expect from the Type System

## Some Common Basic Types

## Function Application

## Useful Composite Data Types: Lists and Tuples

## Functions Over Lists and Tuples

### Passing an expression to a Function

## Function Types and Purity

## Haskell Source Files, and Writing Simple Functions

### Just What is a Variable, Anyway?

### Conditional Evaluation

## Understanding Evaluation by Example

### Lazy Evaluation

### A More Involved Example

### Recursion

### Ending the Recursion

### Returning from the Recursion

### What Have We Learned?

## PolyMorphism in Haskell

### Reasoning About Polymorphic Functions

### Further Reading

## The Type of a Function of More Than One Argument

## Why the Fuss Over Purity?

## Conclusion
