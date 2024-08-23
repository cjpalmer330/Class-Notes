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
	- ( λ (x)   x * x * x )
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