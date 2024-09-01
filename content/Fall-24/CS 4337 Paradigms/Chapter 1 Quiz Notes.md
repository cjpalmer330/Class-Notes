Depending on your programming domain you need to use a language that may support different utilities. for example a mathematical program or scientific program may need a language that supports certain number formats. You also need to think about the efficiency of the language when programming something low level like an operating system or systems programming. Languages can be evaluated by a few criteria, names readability, writability, reliability, and cost.
**Readability:**
Readability is defined by the languages simplicity and orthogonality. The simplicity just implies there is little feature multiplicity / overlap, and few symbols with multiple operations. Orthogonality is a count of the number of privatives, with fewer primatives it is harder to read and understand the data structures being used.
**Writability:**
like with readability simplicity and orthogonality can contribute to the languages writability. If the language has a smaller number of primitives and the primitives are consistent with a single purpose the language is much easier to write. Support for abstraction, and support for expressivity makes the languages more intuitive and less convoluted to write.
**Reliability:**
Features like type checking and exception handling are considered to increase the reliability because the language is checking your code for you in certain manners to ensure it doesn't break when in production. Also if the language allows aliasing / pointers your reliability is actually lower because its possible that the user can make a mistake when shuffling around memory. And more fundamentally if the language is more readable and writable then you as the programmer are less likely to make any crucial error and thus have a more reliable program.
**Cost:**
Cost of the language can be many things, for example the cost of compiling, executing, training employees to use the language, cost of writing the program, if its unreliable there is more cost in testing and maintenance.
There of course is some trade offs between some of these criteria, a sort of mutual exclusion. For example there is a compile time cost that is increased when we provide type checking and error handling,
**Language Categories:**
Imperative: central features are variables, assign statements, and iteration. There is some state of the program and each statement or line executed sequentially changes the state of the variables.
Functional: based on mathematical functions, Importantly functional languages do not use variables or assign statements.
Logical: rule based language, rules are in no specific order. It will output all possible answers
Markup: like HTML used to format webpages
Implementation methods:
There are two primary components of computers, namely memory and processor. For most languages we have a compiler which translates our code into machine language that the computer can understand. But we also could use another program that interprets the language.
Compilation:
Compilation runs in a few steps
1. Lexical analysis
	1. Converts the code in the source program into a list of all the lexical units ( variable names, operators, parenthasis etc.)
2. Syntax Analysis
	1. Transforms the lexical units into a parse tree to show the order of operations between lexical units. The structure of sorts
3. Semantics analysis
	1. generate assembly code from the parse tree
4. Code generation
	1. use the assembly code to  generate the machine code
5. symbol table
	1. has all the types of variables and keywords used in the program for the comp
