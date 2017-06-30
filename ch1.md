# Chapter 1: Getting Started

We will introduce ideas in simplified form at first, but Haskell is a deep language, and these simplifications will be expanded in the future.

## Your Haskell Environment

Haskell has two major implementations: Hugs for teaching, and GHC for real work. We'll be using GHC.

GHC has three major components:
`ghc` - an optimizing compiler generating blazing fast code.
`ghci` - an interactive interpreter and debugger.
`runghc` - A program for running Haskell programs as scripts, without needing to compile
them first.

We're assuming you're using at least GHC version 6.8.2, although the latest version as of 2017 is [8.0.2](https://www.haskell.org/ghc/). Using the latest version is recommended for working code.

If you're using Linux or BSD, download pre-built binaries [here](http://www.haskell.org/ghc/
distribution_packages.html). Appendix A gives detailed install instructions.

## Getting Started with ghci the Interpreter

`ghci` is an interactive interpreter that lets us explore and test Haskell code, kind of like `python` or `irb`!

In Linux, `ghci` is run from the terminal. On Windows, it's available on the Start menu.

Here's an example of what you'll see when starting up `ghci` on a Linux box:

```
$ ghci
GHCi, version 6.8.3: http://www.haskell.org/ghc/ :? for help
Loading package base ... linking ... done.
Prelude>
```

`Prelude`, aka 'standard prelude', defined by the Haskell '98 standard, is a set of standard, built-in functions available in Haskell, and it's ready to use. Other loaded modules will show up here as well. `:?` will put up a help message.

`:set prompt string` changes the prompt to `string`. We'll be using `ghci>`.

The prelude is always available, but to add more functions, load modules with `:module + moduleName`:

```
ghci> :module + Data.Ratio
```

## Basic Interaction: Using ghci as a Calculator

We can become familiar with how Haskell works by using `ghci` as a calculator.

### Simple Arithmetic

Haskell works similar to C-like languages: it evaluates expressions in infix form:

```
ghci> 2 + 2
4
ghci> 31337 * 101
3165037
ghci> 7.0 / 2.0
3.5
```

Infix is just a convenience; we can also write in prefix notation. Just put parentheses around the operator:

```
ghci> 2 + 2
4
ghci> (+) 2 2
4
```

In Haskell, numbers can be arbitrarily large:

```
ghci> 313 ^ 15
27112218957718876716220410905036741257
```

## An Arithmetic Quirk: Writing Negative Numbers

Just like in math, to remove ambiguity, enclose negative numbers in parentheses. Sure, simple expressions work as expected:

```
ghci> -3
-3
```

Unfortunately, the negative sign (-) is Haskell's only unary operator, and has to be separated from the rest of the function calls.

```
ghci> 2 + -3
<interactive>:1:0:
    precedence parsing error
        cannot mix `(+)' [infixl 6] and prefix `-' [infixl 6] in the same infix
                                                              expression
```

Wrap it in () and everything will be ok:

```
ghci> 2 + (-3)
-1
ghci> 3 + (-(13 * 37))
-478
```

This avoids ambiguity in our expressions. Functions are called by their name and their arguments, separated by spaces: `f 3`. Thus `f-3` could be misconstrued either as "apply `f` to `-3`", or "subtract `f` and `3`".

Sometimes we can omit whitespace between infix function calls:

```
ghci> 2*3
6
```

But not always:

```
ghci> 2*-3
<interactive>:1:1: Not in scope: `*-'
```

Haskell is reading `*-` as a single operator! We can define our own operators in Haskell, but this one isn't defined. Add () to fix the problem:

```
ghci> 2*(-3)
-6
```

At first, it might seem crappy that we have to put () around negative numbers, but this is a side-effect of being able to invent your own operators, which is so much cooler than the minor annoyance!

## Boolean Logic, Operators, and Value Comparisons

Haskell uses capitalized `True` and `False` as their Boolean operators, and C-like `&&` and `||` for their logical operators.

```
ghci> True && False
False
ghci> False || True
True
``

Haskell does not consider 0 to be false, or any non-negative number to be true. 

```
ghci> True && 1
<interactive>:1:8:
    No instance for (Num Bool)
      arising from the literal `1' at <interactive>:1:8
    Possible fix: add an instance declaration for (Num Bool)
    In the second argument of `(&&)', namely `1'
    Basic Interaction: Using ghci as a Calculator | 5In the expression: True && 1
    In the definition of `it': it = True && 1
```

This error says that `Bool` is not a member of the `Num` type class. The message is big because it's pointing out the exact place of the problem and some ways we could fix it!

Here is a more detailed breakdown of the error message:

```
No instance for (Num Bool)
```

Tells us that `ghci` is trying to treat the numeric value 1 as having a Bool type, but it
cannot.

```
arising from the literal '1'
```

Our use of the number 1 caused the problem.

```
In the definition of 'it'
```

Refers to a `ghci` shortcut that we will revisit in a few pages. 

Pg. 6





