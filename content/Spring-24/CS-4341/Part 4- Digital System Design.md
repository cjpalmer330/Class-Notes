# Hardware Design Languages
- Verilog
	- module declaration similar to function curly brackets. with a declared name
	- ~ NOT
	- & AND
	- | OR
	- ^ XOR
	- use "assign" to change variable value
	- function params can be bit format ex.
		- (input logic \[7:0] a, 
			output logic y)
	- function
		- can have internal variables "logic b, c;"
		- funcName instanceFuncName(param, param, output);
	- Combinational Logic
		- can use ternary operators similar to python
		- can use always_ff to check for reset and clock rising edge (posedge clk)