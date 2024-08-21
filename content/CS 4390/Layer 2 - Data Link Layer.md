# Layer 2 Duties
1. Hop-to-hop delivery
2. Packetizing
3. Addressing
	- [[#MAC Addresses]]
4. Error control
5. flow control
6. Medium access control
	1. Some level of prioritization is given to users on a network in order to limit throughput to avoid overloading a system, or "crowding up the pipe" so to speak.
# Packetizing / Framing
- Physical layer determines how bits are to be sent / encoded [[Layer 1- Physical Layer#Bit Encoding]] on the electrical level. i.e. what voltage represents 1 and what voltage represents 0
- Layer 2 is responsible for how these 1s and 0s represent data and formatting information.
## Framing
- Break bit stream from physical layer into frames for error detection and recovery
- Data is always flowing, so we need ways to determine when the bits we are sending are synchronization information or data or null characters etc.
- Four framing methods
	1. Character count
	2. Flag bytes with byte stuffing
		- Character Stuffing:
			- an escape character is inserted ahead of flag byte to denote that the next 8 bits are actually part of the data, even though the sequence matches the flag sequence
		- Start and end flags are added to each frame that is sent. This shows where the data starts and ends to hopefully avoid reading the wrong range. 
		- The flag is some sequence of 8 bits, however it is possible our data will have this same pattern of 1s and 0s, in which case we put a escape character before the flag to denote that the next 8 bits are data and not a flag.
			- This escape character is not kept within the data, it is only used to denote the next 8 bits are part of data, and the escape character is thrown away
			- This escape character sequence can also appear into data, in which case just like the flag sequence, we precede the 8 bit sequence with an escape character
			-  EXAMPLE: If our "data starts here: " or "data ends here: " flag is 1111_1111, it is possible we want to send some data that has 8 1s in a row. in which case we would send { start flag } { Some Data } { escape character } 1111_1111 { Some data } { end flag }
	3. starting and ending flags with bit stuffing
		- We have starting and ending flags where we have {many 1s} inserted 0 { data } inserted 0 { many 1s }
			- In real world the flag is usually 0111_1110 hence the 6 1s being typical marker that we need to break up.
		- In bit stuffing we consider that some amount of 1s ( I will assume that the chosen standard is 6 1s indicated idle) in a row or more is considered "idle". any time where we have consecutive 1s we are not sending data
		- Very similar to the previous method, there are exceptions we need to account for where data may have many 1s in a row
		- Any case we have 5 consecutive 1s we insert single 0 to break up the 1s and avoid idle message
		- EXAMPLE:
			- Start 00 1111 1111 1110 0000 1110 1111 Stop gets converted into
			- { idle }0 00 111110 111110 100000 1110 1111 0{ idle }
	4. Physical layer coding
# MAC Addresses
- 48 bits that is burned into the motherboard, much like how a car has a unique VIN number
- the first 24 bits are the Organizational Unique Identifier, the next 24 are Vendor Assigned
	- OUIs are purchased from IEEE Registration Authority, meaning all Dell computers for example will have the same 24 OUI bits, The next 24 Dell can assign them however they wish
- Computers over public wifi, will receive all messages sent. Then, when following laws and protocol, will receive the messages, and determine if the requested MAC address matches that of your computer. If the message is meant for someone else, you throw away the message. A malicious actor could eavesdrop on public networks if the messages aren't encrypted.
# Medium Access Control
- Problem: When two or more notes transmit at the same time on a LAN, their frames will collide and the link bandwidth is wasted during collision. as we can't know in a string of 1s and 0s which are meant to be from sender A and which are from sender B
	- In the case someone is sending a very large file, we don't want the whole network to be blocked for everyone while we wait for one person to send their large video file for example
## Random Access Protocols
- No station is superior and the control is assigned randomly to different computers at any given time.
- Simple and easy to implement
- In low traffic, packet transfer has low-delay, however limited throughput and in heavier traffic, packet delay has no limit
- in some cases a station may never have a chance to transfer its packet if it is unlucky
### ALOHA
- All frames must be a fixed length
- let users transmit whenever they want,, but when a collision occurs, my computer will wait a random amount of time and retransmit. Hoping that the two colliding computers will retransmit at different instances.
- When the transmission is sent, the sender waits for an acknowledgement from the receiver confirming the arrival of the message, if after 2* the propagation delay there is no acknowledgement, the sender considers message lost and will try again after some random delay
- If the sender fails to send multiple times in a row the send task will be aborted
- Critical Time:
	- As all frames are a fixed length we can know the time when a collision is possible. If for example, the transmission time of a frame is 5 milliseconds, then we don't want to send any messages less the 5 seconds before or after a message as they would overlap and corrupt the data.
- Efficiency ~ 18%
### Slotted ALOHA
- Time is divided into slots, and stations can only transmit during these set periods, not whenever they want.
	- This completely prevents partial collisions as all transmissions are synchronized in a sense. If two stations try to send at once, they will randomly choose a period in the future
- Efficiency ~ 37%
## Carrier Sense Multiple Access (CSMA)
- The main concept is to avoid transmission that are certain to cause collisions
- On LAN networks, propagation time is very small, so if a frame is sent, all stations know to wait before they send their own message
- Collisions can still occur if two stations start a transmission within the time it takes for Station B to know that Station A is already sending a message ( propagation delay )
- Three types of CSMA Protocols to determine when station is idle vs busy
	1. Non-Persistent CSMA
		- A station with frames to send, should send it medium is idle, otherwise wait a random amount of time and repeat
	2. 1-Persistent CSMA
		- If the medium is idle, transmit immediately, if it is busy continuously listen until medium is idle, then transmit immediately.
	3. 2-Persistent CSMA
		- Time is divided into slots where each slot typically equals the propagation delay
		- If the medium is busy continuously listen, but only once per time slot.
		- If the medium is busy transmit with some probability, or wait for next slot
### CSMA Collision Detection
- if a collision has occurred the channel is unstable until all colliding packets have fully transmitted
- while transmitting, the sender listen for collisions, if there is a collision, stop sending my packet so that the channel is filled for less time, reducing wasted time.
### Hidden and Exposed Terminals
- Hidden Terminals
	- A cannot send to C, but B can send and receive from both A and C.
	- This can cause a collision because C sends to B, not in range of A, and A can send without knowledge of C. So collision detection fails as B is the only node with knowledge of both
- Exposed Terminal
	- BB sends to A, and C wants to send to another terminal outside the reach of B
	- While B is sending, C detects the medium as busy, so C waits for the transmission, even though it could send in the other direction towards D without issue. This causes an issue of unnecessary waiting time for C.
### How does a node detect a collision
- Transceiver: the signal will rise in amplitude relative to the rest of the signal showing that there was an constructive interference where two signals are passing through the medium at once.
- If a node finds a  collision, it should abort and transmit a jam signal for 48 bits to notify other nodes that the medium has had a collision.
- Jamming
	- Typically 4 bytes ( 32 bits ) of random signals 
### Exponential Backoff Algorithm
- When ethernet detects a collision it waits some amount of time typically 2* propogation delay, maybe a few microseconds.
- if that fails, the wait will increase 2 fold, so that the delay exponentially increases
- After 16 failed wait periods, the process gives up and reports failure
## Controlled Access Protocols
- provides an in order access to shared medium so that every station has a chance to transfer
- eliminates collision completely
### Reservation
- Stations take turns transmitting a single frame at a full rate bits per second
- Each cycle has a reservation interval that has a slot for each station. When a station needs to send data, it edits its corresponding bit to denote reservation
- By listening to reservation interval all stations know who wants to send
- The reservation cycle propagates when all stations have either made their reservation or denoted that they do not intend to send data. The interval for a station to send is fixed
### Polling
- Stations take turns accessing the medium
- Centralized polling
	- One device is assigned as primary station and the others are secondary
	- All data exchanges are done through the primary
	- when the primary has a frame to send it sends a select frame that incldues the address of the intended secondary
	- When the primary station is ready to receive it sends a poll packet to each station and they have the option to denote that they want to send something to the primary, if yes data will transmit
- Distributed polling
	- no primary or secondary
	- stations have a known polling order and cycle
### Token passing network
- Implements the distributed polling system, in a sort of round robin network
- Station interface is in two states Listen and transmit
	- listen state: Listen to the arriving bits and check the destination address to see if it is its own address. If yes the frame is coped to the station otherwise it is passed through the output port to the next station
	- Transmit state: station captures a special frame called the free token, and transmits its frames. Sending station is responsible for reinserting the free token in to the ring medium and for removing the transmitted frame from the medium.
### Channelization Protocols
- Available bandwidth of a link is shared in time, frequency, or through code
- can think of a highway where a station is limited to one lane in the highway, in your channel and another station is limited to its own lane so they don't interfere
### Frequency-Division Multiple Access (FDMA)
- The width of possible frequencies is divided into bands for each station to limited to
- Each station can transmit continously on its designated band, without fear of interfering with another station. At any moment in time, N number of channels can be sent at once
- Cannot be used for non-optical wired connection
### Time-Division Multiple Access (TDMA)
- At any moment in time only one station can send, but they are designated every 4th set of 10ms for example, so that there is no collision with the stations on the other channels.
- This means even if only 1 station wants to send, it must wait for its time slot until it is allowed to send. in worst case that could be a whole cycle of stations
- non optical wired connection
### Code-Division Multiple Access (CDMA)
- one channel carreies all transmission simultaneously
- The stations encode their data so that the data can be recovered from the the desired station.
-  Cannot be used for non-optical wired connection

# Error Detection / Control
- Three types of errors
	- Single bit
	- Multi-bit
	- Burst error
## Error Detection
- Use redundancy before the data to in some way know if the data has changed in any way
- Vertical Redundancy Check
	- Every 7 bits add a redundancy bit
	- If the total number of 1s is even sends a preceding 0, if the number of 1s is odd send a preceding 1. The receiver can then check if a odd number of bits have switched.
- Longitudinal Redundancy Check
	- 1 redundancy byte for 4 data bytes
	- For each of the data bytes, each position is checked. if it is even we put a 0 in the redundancy byte. If the position has an odd number of 1s, the respective position in the redundant byte is 0
	- *10101010*   10101001   00111001   11011101   11100111
	- LRC increases probability of detecting burst errors, and in order for two bits to not be detected they have to be two within the same position in two different bytes
- VRC and LRC can be used concurrently in a 2d array sort
	- Each data byte, and the LRC redundancy byte is limited to 7 bits, with the final 8th bit being the VRC check byte
- Cyclic Redundancy Check
	- Tells us that an error has occurred by providing some divisor, and if the remainder is not zero we know there is some error
		- From Sender
			- If the remainder is zero, then we add k - 1 zeros for check sum bits
			- If the remainder is not zero we add this non-zero value to the end of the data
		- From Receiver
			- we do the division, with some divisor, and if the remainder is zero we know that the data got sent without any errors in transmission
	- Cannot fix the error in any way
	- The divisor is determined by a polynomial that is an institutional standard, if the term exists insert a 1, other wise 0
	- CRC-12 x^12+x^11+x^3+x+1 correlates to a 13 bit divisor as follows:
		- 1 1000 0000 1011
	- When dividing add k - 1 zeros to the end of dividend, with k being the length of the divisor.
### Hamming Distance
- The distance between two words is the number of differences between corresponding bits. A sort of bitwise XOR of the two strings of bits
	- 101 and 100 have a hamming distance of 1
- Minimum hamming distance is the smallest hamming distance between all possible pairs in a set of words.
- To guarantee the detection of up to s errors, the minimum hamming distance is s + 1
- Can be used with a 4 row table that has each combination of 2 bits assigned a 5 bit transmission. So in the case a 5 bit string is not among the table, we know there is an error, and based on the hamming distance to each row in the table we can predict what the data was supposed to be.
### Hamming Codes
- Redundant bits are added in between the data bits. Namely, in the slots that are powers of 2. (1,2,4,8,16)
- position 1, is based on ensuring there is an even number of 1s in the slots who have a 1 in the 1s digit
	- 1 ( 0001 ), 3 ( 0011 ), 5( 0101 ), . . . 11( 1011 )
- position 2, is based on ensuring there is an even number of 1s in the slots who have a 1 in the 2s digit 
	- 2 ( 0010 ), 3( 0011 ), 6 ( 0110 ) . . . 11( 1011 )
- Repeat for every power of two that is needed to cover all your data.
# Hop-to-Hop Delivery
- Hop from computer to router, from router to WAN, to another router, to another computer
- Bridge hop to hop is used to send within a local area. These can work with routers to send to longer distances like other states or countries.
- But the bridge is always listening just like the computers on the network, and when they see that the destination is not one of the computers on the network, it sends the signal to the other network/bridge it is connected to. Ex. 5 computers to one bridge -> another bridge -> 5 computers. or 5 computers connected -> bridge -> 5 separate, yet interconnected computers
- Mac addresses and ip addresses are assigned by the routers or bridges when they recieve a message from a computer for the first time.
	- These ip addresses will time out if not used for some number of minutes and the bridge will assume you have disconnected
	- If computer A sends a message to computer B and B is not on the bridge currently, the router will flood the network, just sending the message to everyone, when the response if found by B, the router will document B for future use
	- Bridges can use port numbers to delineate computers into a few small groups, where all messages are sent to the group.