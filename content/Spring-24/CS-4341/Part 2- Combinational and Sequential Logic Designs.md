# Chapter 2 - Combinational Logic

- Memory*less*
- output determined by current values
- Every element is combinational, every node is either an input or connects to exactly one output.
- The circuit cannot contain any cyclical paths

## Sequential Logic

- Has memory
- output has previous and current values

## Boolean Equations

- Complement: NOT value, the opposite.
- Literal: Variable or complement
- Implicant: Product of Literals
  - AC
- Minterm: Product that includes all inputs
  - AB'C, ABC'
- Maxterm: sum that includes all inputs
  - A + B + C, A' + B + C
- All Boolean equations can be written in SOP or POS form
  - Sum of Product, Product of Sum
  - SOP focus on output is 1
    - ORing ( sum ) minterms ( product ) where output 1
    - in SOP make the binary statement true (1)
  - POS focus on output is 0
    - ANDing ( product ) maxterms ( sum )where output 0
    - in POS make the binary equation false (0)
  - the minterm subscript is the binary representation of the inputs
  - each row has a minterm

### Example Problems

    Only going to the park if it is not raining, and we have sandwhiches
    RS
    00  0
    01  0
    10  1
    11  0
    SOP
    P = R'S
    POS
    P = (R+S)(R+S')(R'+S')

    Winner if you send million dollars or a small notepad
    MN  W
    00    0
    01    1
    10    1
    11    1
    POS = (M + N)

    You can eat delicious food if you make yourself or have personal chef and she is
    talented but not expensive
    E = M + CTX'

    You can Enter the building if you have a hat and shoes on or if you have a hat on
    E = H + HS

### Boolean Algebra Axioms

- Every variable and value is either 1 or 0
- 1 \* 1 = 1, AND operation
- 1 + 0 = 1, OR operation
- 1' = 0, NOT operation
- Theorems
  - B \* 1 = B Identity
  - B \* 0 = 0 Null
  - B \* B = B Idempotency
  - B'' = B Involution
    - Can use double negation to improve signal strength, and thus latency if a component has to drive many subsequent components.
  - De Morgan's theorem
    - The complement of the product is equivalent to the sum of the complements
    - The complement of the sums is equivalent to the product of the complements
    - (ABCD)' = A' + B' + C' + D'
    - Every time you split a term, you invert the operation and add a bar

### Logic to Gates

- Priority Circuit
  - The most significant output bit is asserted to be true, all else false
  - Can use SOP form, to see which gates are necessary to evaluate to the priority circuit in boolean equations
  ```
  |A1|A2|Y1|Y2|
  |--|--|--|--|
  |0|1|-|0|1|
  |0|0|-|0|0|
  |1|0|-|1|0|
  |1|1|-|1|0|
  ```
- Bubble pushing
  - The premise of De 'Morgan's theorem
  - NAND is equivalent to the OR operation on the notted inputs
  - NOR is equivalent to the AND operation on the notted inputs
  - Rules
    - Begin on the output and push towards the input
    - Try to make the bubbles, or NOTs cancel
    - Avoid an output bubble
- [K-maps](<./Karnaugh%20Maps%20(%20K-Maps%20).md>)
- X and Zs
  - Contention X
    - With the temperature, voltage, time, noise, etc. The result might be in the forbidden zone so we cannot determine if the answer is 0 or 1.
    - X can also be used when we want the input to be uninitialized, or arbitrary
    - If you can garuntee that some combinations never occur, we can just ignore a certain truth table, to simplify the boolean equations
  - Floating Z
    - High impedance, open,
    - value is somewhere between 0 and 1, the voltmeter wont tell you it is floating, but it could change output without change of input because it is in the middle zone
- Multiplexers
  - Selects between one of N inputs to connect to output
  - S being 0, means the top input will be selected, S being 1 means the bottom input will be selected.

## Decoders

- N inputs, 2^N outputs
  - 2 inputs, has 4 outputs, but must use 4 bits in order to represent that output because of the one hot principle
- One-hot outputs, one one output HIGH voltage at once.

## Timing

- Delay
  - We may need to delay the signal from a component to ensure that they reach the desired gate at the right time
  - Propagation delay: tpd = max delay from input to output
  - Contamination delay tcd = min delay from input to output
  - Delay cause by the capacitance and resistance in a circuit
  - Can write an equation for each the propogation and contamination delay which we say for example tcdAND + 3tcdOR to be the minimum delay that is required for any given input to reach the output
- Glitches
  - When the desired output momentarily switches because the delay between gates causes one signal to reach a gate faster than the other
  - Not common anymore because of design conventions
