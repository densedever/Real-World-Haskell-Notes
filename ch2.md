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

The benefits of Haskell's type system will take several chapters to explain, but early on it may seem a chore to deal with.

Why take the effort to line types up if Python or Ruby can take care of it for you?

Static typing makes the language safe, type inference makes it concise. We thus have a strong, expressive language that will be justified throughout the book.

Fixing type errors results in fewer runtime errors, so the debugging you'd normally have to do after compilation is moved up-front.

Haskell's type system serves you. The type systems of other statically-typed languages serve the compiler. A more complete vision of the usefulness of Haskell's type system will be clarified throughout the book.

## Some Common Basic Types

Here are more common basic types:

* `Char` - unicode character
* `Bool` - true/false value
* `Int` - signed, 32/64-bit long int
* `Integer` - unbounded signed integer
* `Double` - 64-bit floating-point value. `Float`s are unoptimized, don't use
  them.

*Notation expression* `(::)` explicitly defines a type. The type is inferred
otherwise.

```
ghci> :type 'a'
'a' :: Char
ghci> 'a' :: Char
'a'
ghci> [1,2,3] :: Int
<interactive>:1:0:
    Couldn't match expected type 'Int' against inferred type '[a]'
    In the expression: [1, 2, 3] :: Int
    In the definition of 'it': it = [1, 2, 3] :: Int
```

A *type signature* is the `::` and the type after it.


## Function Application

We'll now work with the data types using functions.

A *function application* is the name of the function followed by its arguments without parentheses or commas:

```
ghci> odd 3
True
ghci> odd 6
False
```

The compare function takes two arguments:

```
ghci> compare 2 3
LT
ghci> compare 3 3
EQ
ghci> compare 3 2
GT
```

This syntax is simple, but takes getting used to when coming from C.

Function application has higher precedence than operators, so these two expressions are the same:

```
ghci> (compare 2 3) == LT
True
ghci> compare 2 3 == LT
True
```

The parentheses don't do harm, but they do add noise. They're sometimes necessary though:

```
ghci> compare (sqrt 3) (sqrt 6)
LT
```

This applies `compare` to `sqrt 3` and `sqrt 6`, otherwise it would be like passing four arguments to `compare`.


## Useful Composite Data Types: Lists and Tuples

Lists and tuples are constructed from other types - composite data types.

A `String` is a list of `Char` values, denoted `[Char]`.

`head` returns a list's first element:

```
ghci> head [1,2,3,4]
1
ghci> head ['a','b','c']
'a'
```

`tail` returns all but the head:

```
ghci> tail [1,2,3,4]
[2,3,4]
ghci> tail [2,3,4]
[3,4]
ghci> tail [True,False]
[False]
ghci> tail "list"
"ist"
ghci> tail []
*** Exception: Prelude.tail: empty list
```

We can apply `head` and `tail` to lists of different types. Applied to a `[Bool]`, it returns `Bool`, and etc.

The list type can hold any type, so it's called *polymorphic.* When writing a polymorphic type, we use a *type variable* with a lowercase letter, that will eventually be replaced by a real type.

"List of `a`" is `[a]`, meaning "list of whatever type."

*Note: The reason a type name must start with a capital letter is to distinguish it from a type variable.*

Just like `[Int]` is a list of `Int`s, a `[[Int]]` is a list of `[Int]`.

```
ghci> :type [[True],[False,False]]
[[True],[False,False]] :: [[Bool]]
```

This is a list of lists of `Bool`.

*Note: in imperative languages, looping is done iteratively. In Haskell it's done by recursing over a list. We use data to structure our program and control flow instead of jumping/branching structures.*

A tuple is a fixed-size value collection that can have any mixture of types in it, different from a list that can be any length, but can only have one type in it.

If we want to store a book's title and year of publication in one data structure, we'd use a tuple because a list can't have both an Int and a String in it.

```
ghci> (1964, "Labyrinths")
(1964,"Labyrinths")
```

We write tuples and their types inside parentheses and separated by commas:

```
ghci> :type (True, "hello")
(True, "hello") :: (Bool, [Char])
ghci> (4, ['a', 'm'], (16, True))
(4,"am",(16,True))
```

The value `()` is a special empty value similar to C's `void` called "unit".

Haskell doesn't have 1-element tuples, and tuples of more than one element are called n-tuples. You rarely see long tuples having more than a handful of elements.

Tuples are ordered, so two tuples with the same data in different orders are different types.

```
ghci> :type (False, 'a')
(False, 'a') :: (Bool, Char)
ghci> :type ('a', False)
('a', False) :: (Char, Bool)
```

`(Bool, Char)` ≠ `('a', False)`, even though the data is the same.

```
ghci> :type (False, 'a', 'b')
(False, 'a', 'b') :: (Bool, Char, Char)
```

`(Bool, Char, Char)` is distinct from `(Bool, Char)` because the number of elements isn't the same.

Tuples can be used to return multiple values from a function or as a basic
container when we don't want to make a custom type.


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
