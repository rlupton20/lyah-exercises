# Exercises for learn you a haskell

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

### Exercise (advanced)

Write an expression defining the fibonnaci sequence as an infinite list. You will want to use `tail`, `zipWith` and recursion.

## Believe the type

### Exercise (basic)
In GHCi, how can you find out what typeclasses a type belongs to?

## Syntax in functions

### Exercise (basic)

Write a function to generate the fibonacci numbers
  - Write a function, `fibPairs`, so that when given an integer `n`, `fibPairs` returns a pair of integers representing the (n - 1)th and and nth fibonnacci numbers. You can take the -1st element of the fibonnaci sequence to be 0, and the 0th to be 1.
  - Write a function `fibonnaci` that maps an integer n to the nth fibonnacci number.
