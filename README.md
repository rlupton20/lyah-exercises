# Exercises for learn you a haskell

At each chapter, it's worth considering how you might improve your solutions from previous chapters with what you've encountered since.

## Introduction

### Exercise (basic)

`stack` is the preferred build tool for modern Haskell. Install it on your system and familiarize yourself with it's usage. You may also want to configure your editor for Haskell development.

## Starting out

### Exercise (basic)

Implement the fizz buzz list. If x is the i'th element of `fizzBuzz` then
  - if i is (only) divisible by 3 then x is the string `"Fizz"`;
  - if i is (only) divisible by 5 then x is the string `"Buzz"`;
  - if i is divisible by 3 and 5 then x is the string `"FizzBuzz"`;
  - otherwise x is the string `""`;
`fizzBuzz` should be an infinite list. You may define it any way you please.

### Exercise (intermediate)

Write a function that implements an approximation to sqrt for integers.
`floor . sqrt` will do this.  Find another way.

### Exercise (advanced)

Write an expression defining the fibonnaci sequence as an infinite list. You will want to use `tail`, `zipWith` and recursion.

## Believe the type

### Exercise (basic)
In GHCi, how can you find out what typeclasses a type belongs to?

## Syntax in functions

### Exercise (basic)

Write a function to generate the fibonacci numbers
  - Write a function, `fibPairs`, so that when given an integer `n`, `fibPairs` returns a pair of integers representing the (n - 1)th and nth fibonacci numbers. You can take the -1st element of the fibonnaci sequence to be 0, and the 0th to be 1.
  - Write a function `fibonnaci` that maps an integer n to the nth fibonacci number.

## Recursion

Spoiler warning -- Don't look in the Data.List module until you have working answers to this chapter's exercises.

### Exercise (isPrefixOf)
```haskell
-- | The 'isPrefixOf' function takes two lists and returns 'True'
-- iff the first list is a prefix of the second.
isPrefixOf              :: (Eq a) => [a] -> [a] -> Bool
```
* Find a **recursive** implementation of `isPrefixOf`
* Why do we enforce the typeclass constraint `(Eq a)`?

### Exercise (intersperse)
```haskell
-- | The 'intersperse' function takes an element and a list and
-- \`intersperses\' that element between the elements of the list.
-- For example,
--
-- > intersperse ',' "abcde" == "a,b,c,d,e"
```
* Implement `intersperse` recursively

### Exercise (transpose)
```haskell
-- | The 'transpose' function transposes the rows and columns of its argument.
-- For example,
--
-- > transpose [[1,2,3],[4,5,6]] == [[1,4],[2,5],[3,6]]
```
* Implement `transpose`recursively

## Higher Order Functions

### Exercise (primes)

Attempt this exercise after reading the **Maps and Filters** section.

Write functions that produce:
* The 1st n primes
* The nth prime
* The first prime above a given integer

For each following section of this chapter, rewrite your functions with some new syntax you have learned.  Observe how the syntactic sugar melts away as you approach the end of this chapter.

## Modules

### Exercise (Making our own Modules)
Put all your functions to handle primes into a module, exporting the three listed above.
Load this module into ghci.

## Making our own Types and Typeclasses

## Functors, Applicative Functors and Monoids

### Exercise (write a basic parser)

Let's write some basic parsing tools. Let's start with a result type, indicating how a parse is progressing - it either failed, or has so far produced an `a` with some remaining input.

```haskell
data Result a = Partial a String | Fail deriving (Eq, Show)
```

Now we can write down the type for a Parser. Conceptually, a parser for `a` is a function `String -> a`, but it might fail, and since we want to compose pieces of parsers to make bigger parsers, it might not consume all of the `String`. So we instead define:

```haskell
newtype Parser a = Parser { runParser :: String -> Result a }
```

We can run a parser `p` on a string `s` with `runParser p s`.

#### Part 1

Write a `Parser` `character c` which matches a character if and only if that character is `c`. Test cases:
  - `runParser (character 'a') "a" == Partial 'a' ""`
  - `runParser (character 'a') "b" == Fail`
  - `runParser (character 'a') "" == Fail`
  - `runParser (character 'a') "ab" == Partial 'a' "b"`

Write a `Parser` `anychar` which matches any character. Test cases:
  - `runParser anychar "ab" == Partial "a" "b"`
  - `runParser anychar "" == Fail`

Write a `Parser` `tag t` which matches precisely when the start of the string is `t`. Test cases:
  - `runParser (tag "test") "test" == Partial "test" ""`
  - `runParser (tag "test") "tes" == Fail`
  - `runParser (tag "test") "testing" == Partial "test" "ing"`

#### Part 2

Write a `Functor` instance for `Parser`. Test cases:
  - `runParser (fmap length $ tag "foo") "foobar" = Partial 3 "bar"`
  - `runParser (fmap (=='a') anychar) "bb" = Partial False "b"`
  - `runParser (fmap (=='a') anychar) "ab" = Partial True "b"`

#### Part 3

Write an `Applicative` instance for `Parser`. Test cases:
  - `runParser ( (,) <$> tag "foo" <*> tag "bar" ) "foobar" == Partial ("foo", "bar") ""`
  - `runParser ( (,) <$> tag "foo" <*> tag "bar" ) "goobar" == Fail`
  - `runParser ( (,) <$> tag "foo" <*> tag "bar" ) "foogar" == Fail`
  - `runParser ( (,) <$> tag "foo" <*> tag "bar" ) "foobarbaz" == Partial ("foo", "bar") "baz"`

#### Part 4 (Extension)

Build a basic parsing library.
