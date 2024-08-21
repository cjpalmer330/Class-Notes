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
- (n^2)  = ω(n) because for any constant c, after some n0, n^2 will be lower bounded by n
	- lim -> n^2 / n == ∞
## Big Theta ( Θ )
- If there is some constant that both f(n) = O(g(n)) AND f(n) = Ω(g(n)) then it can be said that f(n) = Θ(g(n)) which indicates the same growth rate
## Reflexivity / Symmetric / Transitivity / Transpose
- Reflexivity
	- A function is always bounded by the upper and lower by itself
	- Holds for all functions, O, o, Θ, Ω, and ω
- Symmetric
	- Holds for Θ notation, where a function  that holds for Θ(g(n)), it is true that g(n) = Θ(f(n))
- Transitivity
	- For all functions, if f(n) = O(g(n)) and g(n) = O(h(n)) => f(n) = O(h(n))
- Transpose
	- A big O, or little o relationship between f(n) and g(n) necessarily implies a Big Omega or little omega relationship in the opposite direction respectively