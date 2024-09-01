# Definitions - 8/19

- Algorithm - a well defined computational procedure that takes some value or set of values and produces some value in a finite amount of time.
- Loop Invariance - A form of correctness proof that relies on mathematical induction
  - Initialization
    - That the algorithm holds true before we start the iterations
    - For insertion sort: before we start the algorithm we have a single sorted element
  - Maintenance
    - That the algorithm remains true in the next iteration of the for loop
  - Termination
    - The algorithm terminates with the correct sequence that the algorithm sought to achieve
- Asymptotic Analysis
  - There are some bounds that can be drawn with constant leaders that can restrict the domain of f(n) from above and below

# Big O, Theta, and Omega Notation - 8/21

## Time Constraint Equivalencies

| Function  | Asymptotic Constraint |
| --------- | --------------------- |
| log^k n   | o(n^i)                |
| n ^ k     | o(c^n)                |
| n^(log c) | Θ(c^(log n))          |
| log(n!)   | Θ(n log n)            |
| sqrt(n)   | n^(sin n)             |
| --------- | --------------------- |

## Big O

- Some function c\*g(n) that is an upper bound to the described function f(n)
- Limit method for determining Big O
  - If lim (f(n))/(g(n)) exists, then f(n) = O(g(n)) if and only if that limit is < some constant c
  - If g(n) is "heavier" then the ratio will tend towards a constant, which means there is some constant that can be applied to g(n) to create an upper bound
  - It is possible that f(n) is still limited by O(g(n)) but the limit doesn't exist for example (2+(-1)^n)g(n)

## Little o

- a stricter upper bound of Big O. That not only is there some constant C that can upper bound f(n), but ANY constant \* g(n) can be used as an upper bound for f(n)
- f(n) = o(g(n)) if and only if lim f(n)/g(n) == 0

## Big Omega ( Ω )

- Ωg(n) there exists a constant c > 0 where c\*g(n) <= f(n) for all n > n0
- Used to indicate the minimum runtime of the algorithm
  - lower bound for the growth rate of f(n)
- limit method can also be used for Big Omega Ω
  - similar to Big O, but the limit is greater than some constant C. lim f(n)/g(n) > C

## Little Omega ( ω )

- similar to Little o in its application where we have a more restrictive lower bound for g(n), that for any constant c\*g(n) holds as a lower bound for f(n)
- f(n) = ωg(n) if and only if lim f(n)/ g(n) == ∞
- (n^2) = ω(n) because for any constant c, after some n0, n^2 will be lower bounded by n
  - lim -> n^2 / n == ∞

## Big Theta ( Θ )

- If there is some constant that both f(n) = O(g(n)) AND f(n) = Ω(g(n)) then it can be said that f(n) = Θ(g(n)) which indicates the same growth rate

## Reflexivity / Symmetric / Transitivity / Transpose

- Reflexivity
  - A function is always bounded by the upper and lower by itself
  - Holds for all functions, O, o, Θ, Ω, and ω
- Symmetric
  - Holds for Θ notation, where a function that holds for Θ(g(n)), it is true that g(n) = Θ(f(n))
- Transitivity
  - For all functions, if f(n) = O(g(n)) and g(n) = O(h(n)) => f(n) = O(h(n))
- Transpose
  - A big O, or little o relationship between f(n) and g(n) necessarily implies a Big Omega or little omega relationship in the opposite direction respectively

# Function Iteration

- functionally acts as a function of function, but rather than being F o g or F o h, we are recursively applying F o f.
- Written as f ^ i where i is the iteration
  - Definitionally f ^ 1 is just the normal definition of the functions
  - f ^ 2 => f(f^1) or in other syntax F o f(x)

## Iterated Logarithm

- One of these preset functions is logarithm with its own notation "star notation"
  - log \* n = min{ i >= 0 : log^i n <= 1}
  - log(log(log(log (log (2^65536))) = 5
    - log \* 2^65536 = 5

# Divide-and Conquer / Recurrence Relations

1. Generally we want to divide the problem into many smaller problems of the same sort.
   - Splitting n into two functions of size n/2
2. Conquer each subproblem recursively
3. Combine the solutions of subproblems to solve the origianl problem

## Merge Sort

1. Split the array into two halves ( left and right)
2. if the array size > 2 repeat step 1
3. else sort the two elements, return
4. recursively merge the sorted halves

- This sorts an array in O(n log n) which is the best possible time for a random input sorting algorithm

## Techniques for Solving Recurrences

- Recurrence Relations generally follow the form
  - T(n) = aT(n/b) + f(n)
  - n/b represents the size of the sub problem
  - a is the number of times each size is called in each sub problem
  - f(n) is the cost for the non recursive part
  - Merge sort would have the relation T(n) = 2T(n/b) + f(n)

### Substitution Method

1.  guess the form of the solution
2.  Prove the guess using induction

- We give some hypothesis, some guess, that the function T will be bounded by Theta( some function)
- Given T(n) plug your function in for T(n)
  - T(n) = 2T(n/2) + c becomes -> T(n) = 2(n log n/2) + c

### Recursion Tree

1. Draw the size and number of nodes labeled in reference to n
   - The first layer has one node n, layer two has two nodes of size n/2 etc.
   - the root node is the original problem
   - The leafs are the base cases. For merge sort that constant would be when the node is of size 1
2. each layer has a time complexity of the sum of each node
3. Solve for the height of the tree
4. The total work = height of the tree \* work per level

### Master Method

- If a < b then we know that the function is sublinear
- if b < a then the function is some function greater than n^1 ( aka linear)
- There are three cases in the master method that can be used to quickly find the growth rate of a recurrence relation
  1.  If f(n) is O(n^(log b a - i)) where i is some constant. then the leaves are growing faster than f, so asymptotically T(n) = Θ(n^log(b)a)
  2.  if f(n) is Θ(n^log(b) a) the leaves grow at the same rate as the base function, so the tree has O(log n) levels. yielding T(n) = Θ(n^log(b)a (log n))
      1. The work done by the root is the same amount of work done by the leaves, as is with merge sort. So the total work = height of the tree \* work per layer
  3.  f(n) is Ω(n^(log(b) a + i)) where i is some constant. in this case f grows faster than the leaves, which means that the work is dominated by the root.
      1. if af(n/b) <= cf(n) where c is some constant c < 1
      2. Then T(n) = Θ(f(n))

### Examples

- T(n) = 4T(n/2) + n
  - Each layer has 4 subproblems each of size n/2
  - n^(logba) = n^2
  - f = O(n^2)
  - T(n) = Θ(n^2)
- T(n) = 2T(n/4) + n
  - log4 2 < 1
  - f(n) = Ω (n^logba)
  - f(n) = Θ(n)
- T(n) = 4T(n/2) + n^3
  - n^log(2) 4 = n^2
  - f(n) = Ω(n^2)
  - T(n) = Θ(f(n)) = n^3

# Maximum Subarray

- Given an array find a non-empty contiguous subarray whose values have the largest sum
  - Example A = \[-2,1,-1,4,-1,2,1,-5,4]
  - Maximum subarray B = \[4,-1,2,1]
- Brute-Force Approach
  - With a nested for loop try every possible subarray keeping track of the largest sum from those subarrays. obviously this runs in O(n^2)
- Divide and Conquer Solution
  1.  Divide the array into two parts A\[1:mid] and A\[mid\:A.length]
  2.  The maximum subarray is either
      1. Entirely in first half
      2. Entirely in second half
      3. Or crossing the midpoint
  - The first two cases we can handle recursively in n log n
  - For the 3rd case we need to use a find-max-crossing-subarray function
    - We have two sums one fore the left and one for the right, initialized to -inf
    - starting from mid, we go left and right respectively, updating the current and max sum for each side
    - The max_left and max_right is typically the index because the actual sum doesn't matter just that the subarray is the maximum
    - O(n)
  - Compare the three test cases to find the total maximi
