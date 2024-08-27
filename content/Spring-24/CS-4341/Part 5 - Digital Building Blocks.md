# Arithmetic Circuits
## Adders
- Ripple Carry Addition
	- Slower
	- Chain 1-bit adders together, with the carry outs connecting them
	- delay = N(t for single bit add)
- Carry Lookahead Addition
	- Predict Ck earlier than Tc\*k
	- Compute Ck separately using 1-stage CMOS logic
	- 00 kill the carry
	- 11 generate the carry
	- 01 propogate the solution to next bit
	- C = AB + (A + B)Cin
	- 
## ALU
- Fixed Point number
	- point set, common notation 8.8, meaning 8 integer digits, and 8 fractional bits
	- U8.8 means unsigned
	- Q8.8 follows twos complement and can be negative
	- 
- Floating Point number
	- 1 sign bit, 8 exponential bits, 23 fractional bits
	- 2^#exponent bits / 2. so for 32 bit number, 127 bias
		- so an exponent of 2^5 would be represented with 132
	- mantissa has implicit 1
	- 0 11111111 all zero = infinity
	- 1 11111111 all zero = - infinity
	- Floating Point Addition
		1. Extract exponent and fraction bits
		2. prepend leading 1 to  form mantissa
		3. compare exponents
		4. shift smaller mantissa if necessary
		5. add mantissas
		6. normalize mantissa and adjust exponent if necessary
		7. round result
		8. assemble exponent and fraction back into floating-point format
- Counters and Shift Registers
	- Counters
		- Increments on each clock edge
		- cycle through numbers
	- Shift registers
		- Shift a new bit in, and one bit out on each clock edge
		- converts a serial input into a parallel output. 
- Memory Arrays
	- efficiently store large amounts of data
	- M-bit data value read written at each unique N-bit address
	- 3 Common types
		- Dynamic RAM - normal ram, read write
			- uses capacitors which need to be constantly refreshed
		- Static RAM - Cache memory
			- much faster than DRAM but it takes up more space, so can hold less data in the same amount of area
			- uses flip flops
			- bro is yapping
		- Read only Memory
			- nonvolatile, persists after shutting down
	- Bit cells
		- A word line and bit line
		- Word line uses decoder to select a row of bit cells, 16 or 32 or whatever
		- the bit lines, are then vertical columns which input to the bit cells
	- ROM Storage
		- program rom one time, read many
	- Logic Arrays
		- PLA ( Programmable logic arrays)
			- AND array followed by OR array
			- combination logic only
			- fixed internal connections
		- FPGA ( Field programmable gate arrays)
			- many logic elements
			- made of logical elements, IO elements, and programmable interconnections
			- combinational and sequential logic
			- programmable internal connections
			- 