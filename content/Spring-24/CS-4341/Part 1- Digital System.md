# Chapter 1
- How do we design things that are too complex to fit in one person's head?
	- Abstraction
		- Hiding details when they are not important. For example instead of drawing every transistor, just draw the gates.
	- Discipline
		- Intentionally restrict design choices
	- The Three 'Y's
		- Hierarchy
			- Designed into models and sub models 
		- Modularity
			- Create re-useable, well defined functions and interfaces
		- Regularity
			- Encouraging Uniformity, so that the modules can easily be reused.
			- A predictable format.
## Numbers Systems
- 1453 in base k to decimal
	- (1 * k^3) + (4 * k^2) + (5 * k^1) + (3 * k^0) 
- Two's complement
	- To convert positive binary to negative invert all the bits and then add 1
	- A negative twos complement can be read as -8 + positive of the rest for example
	- this allows us to use addition to calculate subtraction, which is very efficient for computers
	- -5 + 5 creates overflow, but that's fine because the remaining is 0
## Bit Width
- Extend a number from n -> M bits
	- Sign-Extension for twos complement
		- copy the leading bit to the left as far as you want the number to be
		- 1011 -> 1111 1011 or 0101 -> 0000 0101
	- Zero-Extension for unsigned numbers
		- regardless of polarity extend zeros from the leading bit to the left
		- If you try this with negative signed numbers it changes the value
## Logic Gates
- Perform logic functions
	- NOT, AND, OR, NAND, NOR, XOR, XNOR
- The small circle at tip means "NOT / Negation"
- you can extend some of the gates to more inputs, such as and, or, NOR, and NAND
- Allow some range for noise, if the target voltage is 5V, maybe anything  above 4V is considered a 1
- VIL the minimum value of the input must be higher than output minimum (VOL ) otherwise your margin is negative which doesnt make sense. Your range grows later in circuit as there is more noise
- NML = VIL - VOL             Voltage input - voltage output
- NMH = VOH - VIH           Voltage output high - voltage input high
### CMOS Transistors
- P-N junction
	- n-type has negative doping (free electrons)
		- cathode
	- p-type has positive doping (empty electron slots)
		- anode
	- A junction between a p-type and n-type semiconductor, forms a diode
		- This allows electrons to flow from the p-type to the n-type
	- The border electrons and holes between the p and n types form a depletion layer
		- If you supply electrons on the n-type side, the size of the depletion layer will shrink, until they move from n-type to p-type side, through wire, back to the supply
	- Reverse Bias
		- Add positive holes to the n region, the depletion layer will continue to grow. stopping any current flow in the diode
	- This means the Diode is UNI-DIRECTIONAL. current can only flow through one way. The positive charge goes from P -> N, the electrons move from out of N into P
- MOS ( Metal Oxide Silicon ) transistors
	- Polysilicon gate
	- oxide (SiO2) insulator
	- doped silicon
	- with a p-type substrate, and n-type source / drain.
		- creates effectively a peninsula diode where the current cannot move out of the desired channels.
		- If we charge the gate enough, the substrate creates a negative charge along the border. This creates a sort of channel between the source and drain channels.
	- Depending if the gate is recieving a signal or not, there is a channel between the source and the drain. this connection is considered "on" as the current can flow.
		- nMOS g= 0 -> off, g = 1 -> on
		- pMOS g = 0 -> on, g = 1 -> off
### Logic Gates
- Using the nMOS and pMOS transistors we can form the fundamental logic gates.
	- nMOS pass a "Strong" or accurate 0, but a "weak" or noisy 1
	- pMOS pass a "Strong" or accurate 1, but a "weak" or noisy 0
- AND operation is simply nMOS transistors in series / pMOS in parallel
- OR operation is nMOS transistors in parallel / pMOS in series
- NOT operation is just a single pMOS transistor
- The transistor level of the logic gates will not be on the exam, we can simply use the logic gate symbols.


# Exam 1 Review
- Data representation binary, octal, hex, decimal
	- Conversions
	- Base  -> Decimal, digit * digit value in decimal
	- Decimal -> Base, largest value of base that is within decimal, subtract repeat
	- 