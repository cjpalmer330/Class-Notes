# Overview of Layer 1
- The OSI Physical layer provides the means to transport across the network media the bits that make up a Data Link layer frame
- Purpose is to create representation of the bits, and to pass those bits and their signals up to the Data Link Layer
- The deliver of frames across the local media requires the following Physical layer elements:
	- The physical media and associated connectors
	- A representation of the bits on the media
	- Encoding of data and control information
	- Transmitter and receiver circuitry on the network devices
- The transmission medium can be guided or unguided
	- Guided: twisted cable, coaxial cable, where the waves are guided through a path
	- Unguided: satellite TV, Wi-Fi
# Definitions / Bit Rate
- Baseband: all signals are modulus some base frequency, i.e. lower range
- Broadband: Broad range of frequencies sent in cable
- Modem: A device that accepts digital signals and outputs a modulated carrier wave, and vice versa
	- It is used to interconnect the digital computer to the analog telephone network
- Baud Rate: The number of transitions in the signal per second(how often can you change the amplitude)
- Bit rate: how many binary bits can you transmit per second
	- bit rate = floor(log(# of combinations)) * baud rate
		- if we had 16 symbols (4 amplitudes and 4 frequencies), then bit rate = 4 * baud rate
	- two combinations get unused in the case of 18 combinations as bit rate multiplier must be power of 2
# Bit Encoding
- There are different coding techniques to avoid the desynchronization of clocks between nodes when many of the same bit is sent, for example 1000 0s in a row. These are some different encoding of high/low voltage to correspond to 1s and 0s in data
	- ![[Pasted image 20240606125829.png]]
	- NRZ is the intuitive, high voltage for 1, low voltage for 0
	- NRZ Invert, doesn't send high or low based on 1 / 0 but rather maintains the state when the bit is 0, and inverts when the bit is 1
	- Manchester encoding, sends 10 for 1, and 01 for 0. Forcing the inversion, which ensures that no matter the combination the receiver can ensure clocks are in sync. Has down side of halving throughput
	- 4b/5b every 4 bit sequence is translated to a 5 bit code that ensures alternations that can be read to ensure time sync, but is 80% efficient, opposed to Manchester which is only 50% bit efficient
	- ![[Pasted image 20240606125158.png]]
# Bit synchronization
- Asynchronous
	- send a short bursts of bits followed by a synchronization sequence on a regular basis to ensure correctness
	- The "packets" for lack of better term, has to be surrounded by specific 1s and 0s to ensure the clocks are linked. 
	- Is slower and therefore cheaper than synchronous method because you have to wait for the start and stopping of data transfer between sent bytes
- Synchronous
	- transmit many bits at a time
	- use codes that allow the clock to be recovered, and adjusted after every bit that is sent
	- No space between bytes, so the sender and receiver's clock need to be running at exactly the same time
# Physical Layer Packets
- Packets of bits start with seven bytes called the "preamble" and one Start Frame Delimiter (SFD) byte.
- The preamble pattern of alternation 1 and 0 bits allowing devies on the network to easily synchronize clocks on the bit level, The SFD provides a way to mark new incoming frame and byte level synchronization
- 