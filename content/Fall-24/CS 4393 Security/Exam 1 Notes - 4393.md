# Software Security - Buffer Overflow
- What is a buffer overflow vulnerability
	- Memory locations beyond the boundary of a buffer allocation, are accessed and written too
- If they have enough privilege in a system they can use the SETUID keyword to change process values in a system.
- Buffer overflow method was the first form of malicious attack used in 1988 by Morris Internet Worm using the "fingerd"
- Goal of Buffer Overflow
	- Take control over a program by changing its control flow to run malicious code
	- inserting some malicious code that takes information or steals resources, and reroutes the pointer of the program to run the injected code.
	- Could also take control of host by using the root shell with the user's privileges
- The easiest way to change the pointer  of a program, when injecting a virus or whatever is to change the return address of a function to where you have inserted your code
## Evolution of Buffer Overflow Methods
### Stack Smashing
- The CPU of course keeps its stack for the current line that is being ran in a program. It uses this as we learned in other classes to keep track of function calls and local variables.
- Stack smashing takes advantage of this by writing enough to change the value of the return address. The CPU will be none the wiser and start reading in the new, illegitimate location
- The most challenging part is the find the address of your malicious code, so that you can place the correct return address onto the stack
	- In practice you can never really find exactly where your code starts, only the vicinity
### NOP Sled
- Every processor has a NOP Instruction, which makes the processor idle for that instruction
- The idea being to attach many NOP instructions before your malicious code, that way if your guess on the address is anywhere within the range of the first NOP instruction until the actual first line, your malicious code will still execute properly.
