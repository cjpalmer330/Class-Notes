 ## What is Sequential Logic
- Changes between states of a circuit
- Outputs of sequential logic depend on the current state AND THE PAST, it has memory of the input values
- Combinational logic is parallel
	- no node visited more than once per input
	- fast
	- no storage
- Sequential logic is Serial
	- output is connected somewhere to the input, to keep state value
	- has storage to keep the state, but adds delay
## State elements
- State is defined as bits at a  given time.
	- e.g. green for the north road, and red for the east road. This will switch eventually
- Usually just 1 bit, and contains all the information of the past needed to get to this position
- Bistable Circuit
	- Circuits needs to read, store, and write data. Circuits that do this with binary options are called Bistable circuits
	- e.g. Back to back inverters can create a constant value on the output/input
	- Memory component that keeps a certain value at all times
- SR Latch
	- Slightly bigger than the Bistable circuit, but it allows you to have inputs to control the value that is being stored in the Latch
	- Two NOR gates where the outputs of each NOR gate is connected to one of the inputs of the other NOR gate
	- Can have an output of 1, 0, or be used as memory for the previous state
	- The input of R and S determine the stable value of the Latch
		- When one input is 1 and one input is 0 then there will be a stable Q = 1, and Q' = 0
		- When both inputs are 0 for R and S, the state remains constant with what the previous state was.
		- When both inputs are 1, for R and S, both Q and Q' = 0, 
			- Called "Invalid State" because both Q and Q's opposite are 0
- D Latch
	- Two inputs: Clock and Data
		- Clock controls when the output changes
		- Data controls what the output changes to
	- Function
		- When Clock = 1. D passes through to Q
		- when Clock = 0. Q holds its previous value
		- At all times that the clock = 1 the value of Q can change based on the changing value of Data
- D Flip-Flip
	- Edge triggered
	- When clock rises from 0 to 1, the value of D passes through to Q
	- Two internal D Latches controlled by complementary clocks
	- The value of Q can only change for the small instant that the clock changes from 0 to 1, otherwise any changes in the value of D is ignored.
- Variations
	- A 4-Bit Register is simply 4 D Flip-Flops in parallel sharing one clock.
	- Enabled Flip-Flops
	- Resettable Flip-Flops
		- Synchronized, resets at the clock rising edge
		- Asynchronous, resets the value immediately
	- Settable Flip-Flops
## Synchronous Sequential Logic
- Breaks cyclic paths by inserting registers
- registers contain state of the system
- state changes at clock edge, synchronized to clock
- Rules of synchronous sequential circuit composition
	- Every circuit element is either a register or a combinational circuit
	- at leat one circuit element is a register
	- all register receive the same clock
	- revery cyclic path contains at least one register
## Finite State Machines
- Consists of 
	- State registers
	- Combinational logic
- Next state determined by the current state and the inputs
- Moore
	- Outputs depend only on the current state
- Mealy
	- outputs depend on current state and inputs
- Design Procedures
	1. Identify inputs and outputs
	2. sketch state transition diagram
	3. write state transition table and output table
		- Moore FMS: Write separate tables
		- Mealy FSM: Write combined state transition and output tables
	4. Select state encodings
	5. Rewrite state tables with encodings
	6. write boolean equations for next state
	7. sketch the circuit schematic
- Break complex FSM into smaller interacting FSMs
- 
## Timing of Sequential Logic
- Flip-Flop samples D at clock edge
- D must be stable when sampled
- We may need to delay the signal from a component to ensure that they reach the desired gate at the right time
	- Propagation delay: tpd = max delay from input to output
	- Contamination delay tcd = min delay from input to output
- Setup time - time before clock edge data must be stable
- hold time: time after clock edge data must be stable
- aperture time: time around clock edge data must be stable ta = tsetup + thold
### Clock Skew
- 
## Parallelism
- Spatial Parallelism
	- Duplicate hardware to perform multiple tasks at once
- Temporal Parallelism
	- Task is broken into multiple stages
	- also called pipelining
- Token
	- Group of inputs processed to produce group of outputs
- Latency
	- Time for one token to pass from start to end
- Throughput
	- number of tokens produced per unit time