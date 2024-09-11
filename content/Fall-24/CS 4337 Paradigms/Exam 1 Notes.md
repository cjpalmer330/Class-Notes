# Functional Programming

## Introduction

- Imperative languages are based on von Neumann architecture. Examples are C and Java
  - Programs written in an imperative language have states, that change throughout the execution of the process.
- Functional languages are based off of mathematical functions, a mapping of members of one set to another set. called the domain and range sets respectively
- Functional languages use recursion and conditional expression, rather than sequencing and iterative repetition
- because functional languages do not have state we don't have variables in the way that one uses variables in an imperative language. during a Cube(x) function for example we cannot change the value of x in the function body. x is constant.
- no variable assignment

## Functions

- Lambda expressions are nameless functions.
  - ( λ (x) x _ x _ x )
- Functional Form / higher order function
  - it takes one or more functions as parameters or yields a function as a result or both
  - Function composition used for finding the "recursive" value of a function f(g(x))
- to all functions
  - very similar to the map function in python or java

## LISP

- Only two categories of data objects in LISP, Atoms and List
- Atoms
  - are symbols or numeric literals
  - EX: A, B, 12, Apple
- List
  - pairs where the first part is the data, and the second part is a pointer which can point to an Atom, another List, or another element
- Single quote can be used to denote to the interpreter that you don't want it to attempt and evaluate it. For example. If I am creating a list ( 1 3 5 ). I don't want the interpreter to mistake this as a function so as a safety measure I can input the list as ( '1 '3 '5 )
- Functions
  - List has some built in functions already reserved
    - SIN, MAX, MIN, ROUND, etc.
    - DEFINE
      - Can be used to give variables names, for ex:
        - ( define pi 3.14)
  - Numeric Predicate Functions
    - Used to evaluate numerica paramaters
      - ( = '3 '4) -> False
      - ( < '3 '4) -> True
    - <, >, =, <>, <=, EVEN?
    - All follow the format below
      - ( func 'x 'y) -> z
    - EQ evaluates if the two parameters have the same **_pointer_**
      - x = (a b c) & y = (a b c)
      - ( EQ? x y ) -> False
      - The two variables have the same list as a value but they have different memory locations
    - EQV evaluates if the two parameters have the same **_value_**
    - Other functions
      - LIST?, NULL?
  - Selector Functions
    - Two-way selector
      - essentially an if statement in any other language but just need to keep in mind the limits to syntax
      - ( IF predicate then else )
    - Multiple selector named COND
      - Functions like a switch case block
      - (COND
        - (predicate expression)
        - (predicate expression)
        - (\[ELSE expression]))
  - List functions
    - CAR and CDR Functions
      - Both require a list as a single parameter, other wise they return an error
      - CAR returns the first element of its list param, but the list stays intact
      - CDR returns every element EXCEPT the first
        - useful to remove the first element
      - These can be applied recursively, from the right to left equating to inner to outside of the function
        - CAADAR is treated as CAR(CAR(CDR(CAR(\[x]))))
    - CONS
      - given two parameters return a new list with the two elements
      - The first parameter is inserted as a single element into the list / variable in parameter 2
      - \\#F can be used at the end of a predicate to say " If this predicate is true return False"
        - If you do not append the \#F it is assumed that True -> return true and / or go to the next predicate

## Execution

- Compilers take the written code and convert the C/Java/Rust etc. into pure binary machine code that the computer can understand
  - Then separately you can run the executable
- An Interpreter goes line by line and for each line compiles to machine code and executes
- Scheme is a form of interpreters that uses an infinite ( REPL ) Read-Evaluate-Print Loop
  - Each of the parameter expression is evaluated, but we want the interpreter to skip over constants.
  - A Scheme program is a collection of function definitions

# Chapter 3 - Syntax

- The syntax is the formatting of the language
- Terminology
  - Sentence / Statement : A string of characters from some alphabet
  - Language: A set of sentences
  - Lexeme: lowest level syntactical unit
    - every character in a program.
    - x = 3 \* 4 has 5 lexemes
  - Token: a category of lexemes
- Languages can be formally defined in two ways
  - Language recognizer
    - determines whether the programs are syntactically correct
  - Language generator
    - generates all possible, valid sentences, and then determines if the set contains the sentence given.

## Backus-Naur Form (BNF)

- Fundamentals
  - A metalanguage
  - created in 1959 by John Backus
  - Uses abstractions for syntactic structures
- Left Hand Side (LHS) is the abstraction being defined
  - LHS is always non terminal
- Right Hand Side (RHS) consists of terminals and references to other abstractions
- EX. Total = x + y
  - Total is a non terminal abstraction ( variable)
  - '=' is a terminal symbol
  - X and y are other abstractions

## EBNF
