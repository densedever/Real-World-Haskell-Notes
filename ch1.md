# Chapter 1: Getting Started

We will introduce ideas in simplified form at first, but Haskell is a deep language, and these simplifications will be expanded in the future.

## Your Haskell Environment

Haskell has two major implementations: Hugs for teaching, and GHC for real work. We'll be using GHC.

GHC has three major components:
 * `ghc` - an optimizing compiler generating blazing fast code.
 * `ghci` - an interactive interpreter and debugger.
 * `runghc` - A program for running Haskell programs as scripts, without needing to compile
them first.

We're assuming you're using at least GHC version 6.8.2, although the latest version as of 2017 is [8.0.2](https://www.haskell.org/ghc/). Using the latest version is recommended for working code.

If you're using Linux or BSD, download pre-built binaries [here](http://www.haskell.org/ghc/distribution_packages.html). Appendix A gives detailed install instructions.

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
        cannot mix '(+)' [infixl 6] and prefix '-' [infixl 6] in the same infix
                                                              expression
```

Wrap it in `()` and everything will be ok:

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
<interactive>:1:1: Not in scope: '*-'
```

Haskell is reading `*-` as a single operator! We can define our own operators in Haskell, but this one isn't defined. Add `()` to fix the problem:

```
ghci> 2*(-3)
-6
```

At first, it might seem crappy that we have to put `()` around negative numbers, but this is a side-effect of being able to invent your own operators, which is so much cooler than the minor annoyance!

## Boolean Logic, Operators, and Value Comparisons

Haskell uses capitalized `True` and `False` as their Boolean operators, and C-like `&&` and `||` for their logical operators.

```
ghci> True && False
False
ghci> False || True
True
```

Haskell does not consider 0 to be false, or any non-negative number to be true. 

```
ghci> True && 1
<interactive>:1:8:
    No instance for (Num Bool)
      arising from the literal '1' at <interactive>:1:8
    Possible fix: add an instance declaration for (Num Bool)
    In the second argument of '(&&)', namely '1'
    In the expression: True && 1
    In the definition of 'it': it = True && 1
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

Comparison operators are similar to C:

```
ghci> 1 == 1
True
ghci> 2 < 3
True
ghci> 4 >= 3.99
True
```

except 'not equals', which is `/=`, after math's ≠ symbol.

Where C uses `!` for negation, Haskell uses `not`:

```
ghci> not True
False
```

## Operator Precedence and Associativity

Parentheses allow us to define explicit precedence in expressions, but precedence allows us to write fewer of them.

```
ghci> 1 + (4 * 4)
17
ghci> 1 + 4 * 4
17
```

Precedence is numeric from 1-9, highest being applied first. `ghci`'s `:info` command shows us the precedence level:

```
ghci> :info (+)
class (Eq a, Show a) => Num a where
(+) :: a -> a -> a        -- Defined in GHC.Num
...
infixl 6 +
ghci> :info (*)
class (Eq a, Show a) => Num a where
...
(*) :: a -> a -> a        -- Defined in GHC.Num
...
infixl 7 *
```

`infixl 6 +` indicates `(+)` has precedence 6. The other output will be explained later.

`infixl 7 *` indicates `(*)` has precedence 7. Since `(*)` is higher than `(+)`, `1+4*4` is evaluated as `1+(4*4)` and not `(1+4)*4`.

Haskell's operators also associate to the right or left. `(+)` and `(*)` are left-associative, which is what `infixl` means.

`infixr` is right-associative:

```
ghci> :info (^)
(^) :: (Num a, Integral b) => a -> b -> a        --defined in GHC.Real
infixr 8 ^
```

An operator's precedence and associativity are called *fixity* rules.

## Undefined Values, and Introducing Variables

The Prelude defines a few math constants for us, 

```
ghci> pi
3.141592653589793
```

but not all. Euler's number, for example, is not defined:

```
ghci> e
<interactive>:1:0: Not in scope: 'e'
```

*Note:* 'not in scope' just means there's no variable defined with that name yet.

We can make a temporary definition with `let`:

```
ghci> let e = exp 1
```

This is applying the exponential function, our first function! Other languages require applying a function like `exp(1)`, but Haskell has no need to do that.

`e` can now be used in expressions. `(^)` is used for integer powers, but `(**)` is used for real powers:

```
ghci> (e ** pi) - pi
19.99909997918947
```

*Note:* `let` is only used in `ghci`. It has a different meaning in full Haskell programs.

## Dealing with Precedence and Associativity Rules

Parentheses are okay to use if they help clarify your code. In fact, expressions that rely on precedence alone are common sources of bugs. Rather than try to remember precedence rules, just add parentheses.

## Command-Line Editing in ghci

Editing in ghci is very similar to editing on the command line. Up arrow cycles through command history. Left/right moves you around in the line. Tab-completion also works.

*Note:* Unix uses [GNU readline](http://tiswww.case.edu/php/chet/readline/rltop.html#Documentation), while Windows uses [doskey](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/doskey.mspx).

## Lists

A list is surrounded by square brackets and separated by commas, and can be of any length, with the empty list `[]` (said as *nil*) having length zero:

```
ghci> [1, 2, 3]
[1,2,3]
ghci> []
[]
ghci> ["foo", "bar", "baz", "quux", "fnord", "xyzzy"]
["foo","bar","baz","quux","fnord","xyzzy"]
```

*Note:* commas are separators, not terminators. Putting one after the last list element is an error.

All elements inside a list must be the same type, or there will be an error:

```
ghci> [True, False, "testing"]
    <interactive>:1:14:
    Couldn't match expected type `Bool' against inferred type `[Char]'
    In the expression: "testing"
    In the expression: [True, False, "testing"]
    In the definition of `it': it = [True, False, "testing"]
```

There's no way to turn a Bool into a String (`[Char]`), so the list isn't properly typed.

*Enumeration notation* is a short way to write long lists with a closed interval:

```
ghci> [1..10]
[1,2,3,4,5,6,7,8,9,10]
```

The `..` denotes enumeration on enumerable types. Strings are not enumerable, so `["foo".."quux"]` wouldn't work.

We can also give the size of the steps, it doesn't have to be 1. Just provide the beginning, the step, then the end:

```
ghci> [1.0,1.25..2.0]
[1.0,1.25,1.5,1.75,2.0]
ghci> [1,4..15]
[1,4,7,10,13]
ghci> [10,9..1]
[10,9,8,7,6,5,4,3,2,1]
```

Omit the end point for lists of infinite length. `[1..]` produces all the natural numbers. If you ever print such a list, press ctrl+C to stop the output.

*Note:* try not to enumerate real numbers. They're quirky in all programming languages, so you'll get incorrect results.

### Operators on Lists

Add something to the front of a list with `(:)`, and join two lists together with `(++)`:

```
ghci> 1 : [2,3]
[1,2,3]
ghci> 1 : []
[1]
ghci> [3,1,3] ++ [3,7]
[3,1,3,3,7]
ghci> [] ++ [False,True] ++ [True]
[False,True,True]
```

Remember that with `(:)` the first element absolutely has to be a single value. `[1,2]:3` won't work.

## Strings and Characters

Haskell strings are like Perl or C strings, surrounded by double-quotes, and including escape characters:

```
ghci> "This is a string."
"This is a string."
```

See appendix B for a complete list of escape characters.

```
ghci> putStrLn "Here's a newline -->\n<-- See?"
Here's a newline -->
<-- See?
```

`putStrLn` prints a string.

Characters work like in C too. Single-quotes denote a character, double denotes a string, which is just a list of characters.

```
ghci> let a = ['l', 'o', 't', 's', ' ', 'o', 'f', ' ', 'w', 'o', 'r', 'k']
ghci> a
"lots of work"
ghci> a == "lots of work"
True
```

The empty string is `""` and is a synonym for `[]`. Normal list operators work on strings since they're just lists themselves:

```
ghci> "" == []
True
ghci> 'a':"bc"
"abc"
ghci> "foo" ++ "bar"
"foobar"
```

## First Steps with Types

We haven't done much with types yet, but they are central to Haskell programming. Types are capitalized, while variable names aren't. Get `ghci` to give you more information by using "`:set`":

```
ghci> :set +t
ghci> 'c'
'c'
it :: Char
ghci> "foo"
"foo"
it :: [Char]
```

`+t` prints the type of an expression after printing its value. `it` holds the value of the last expression we typed into `ghci`. 

`x :: y` reads as "expression `x` has type `y`". Here, it has the type of `[Char]`, which just means String.

*Note: `it` lets us use the value of our last expression in a bigger expression:*

```
ghci> "foo"
"foo"
it :: [Char]
ghci> it ++ "bar"
"foobar"
it :: [Char]
```

*`ghci` also won't change the value of `it`, allowing you to make more mistakes freely:*

```
ghci> it
"foobar"
it :: [Char]
ghci> it ++ 3
    <interactive>:1:6:
    No instance for (Num [Char])
    arising from the literal `3' at <interactive>:1:6
    Possible fix: add an instance declaration for (Num [Char])
    In the second argument of `(++)', namely `3'
    In the expression: it ++ 3
    In the definition of `it': it = it ++ 3
ghci> it
"foobar"
it :: [Char]
ghci> it ++ "baz"
"foobarbaz"
it :: [Char]
```

*This is a great way to explore the language.*

Another type is `Integer`, which holds arbitrarily big numbers:

```
ghci> 7 ^ 80
40536215597144386832065866109016673800875222251012083746192454448001
it :: Integer
```

Rational numbers are used with the `(%)` operator, and are in `numerator%denominator` form:

```
ghci> :m +Data.Ratio
ghci> 11 % 29
11%29
it :: Ratio Integer
```

`:module` can be shortened to `:m`. `it :: Ratio Integer` means "ratio of integer", and the numerator and denominator must be integers of the same type:

```
ghci> 3.14 % 8
<interactive>:1:0:
    Ambiguous type variable 't' in the constraints:
      'Integral t' arising from a use of '%' at <interactive>:1:0-7
      'Fractional t'
        arising from the literal '3.14' at <interactive>:1:0-3
    Probable fix: add a type signature that fixes these type variable(s)
ghci> 1.2 % 3.4
<interactive>:1:0:
    Ambiguous type variable 't' in the constraints:
      'Integral t' arising from a use of '%' at <interactive>:1:0-8
      'Fractional t'
        arising from the literal '3.4' at <interactive>:1:6-8
    Probable fix: add a type signature that fixes these type variable(s)
```

You won't always need `+t`, so to turn it off, use `:unset +t`, and we can still get the type with `:type it`, which prints only the type of the supplied expression.

Why this weird output?:

```
ghci> 3 + 2
5
ghci> :type it
it :: Integer
ghci> :type 3 + 2
3 + 2 :: (Num t) => t
```

`3 + 2` could mean integers or real numbers, so `Integer` is the default. Without `ghci` evaluating the expression, it doesn't choose between real or integer, it just says they're "numeric".

## A Simple Program

Here is a simple program that counts the number of lines in its input. Don't worry if you don't understand it yet. Enter the code into a file named `WC.hs`:

```haskell
-- file: ch01/WC.hs
-- lines beginning with "--" are comments.

main = interact wordCount
    where wordCount input = show (length (lines input)) ++ "\n"
```

Find or create a text file, for example called "quux.txt":

```
$ cat quux.txt
Teignmouth, England
Paris, France
Ulm, Germany
Auxerre, France
Brunswick, Germany
Beaumont-en-Auge, France
Ryazan, Russia
```

Run the following shell command:

```
$ runghc WC < quux.txt
7
```

Congratulations: you've just written your first useful Haskell program that interacts with the real world! We'll continue doing this until we can write our own programs.

*Exercises omitted*

Page 16


