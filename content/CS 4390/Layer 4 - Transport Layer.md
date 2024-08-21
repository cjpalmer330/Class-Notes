# Responsibilities of Transport Layer
- Connect to many different hosts
- Different Round-Trip-Times of different sources, therefore need adaptive timeouts
- Tolerate long delays in the network, and have a way to handle very old messages
- Handle packet reorder
- Different capacity at destination with flow control
- Different network capacity with congestion control
- Layer 4 Manages traffic between hosts and ensures complete data transfer.
# Port Number
- 0 - 1023 are reserved for well known applications like echo, users, date, Nameserver etc.
- 1024 - 49,151 are registered commercial ports.
- 49,152 - 65,535 are private and can be used by anyone for local or nonpublic applications.
# UDP
- Purpose
	- is able to determine which window, or program in your computer is to receive the message based on the header.
- Uses IP protocol to transfer information
- Has a fixed size header, of 8 bytes, and up to 2^16 ( 65kb ) bytes of data
	- Header has source port number, destination port number, total length, and checksum each being a 16 bit field
- Provides unreliable transmission of datagrams, but much faster than TCP,
	- Datagram - A self-contained, independent unit of data.
- UDP adds is multiplexing and demultiplexing, simple, unicast and multicast
# TCP
-  End-To-End Protocols
	- TCP uses an IP network which can drop messages or re-order messages or duplicate copies in the case something goes wrong
		- There is no guarantee of all messages arriving, or the order.
- Demultiplexing
	- Several processes may use the same end-to-end protocol
	- How can we distinguish between them?
		- OS process ids
			- natively supported, but changes between different operating systems
		- Port Numbers
			- Better because they are OS independent
			- TCP gives the application an unused port number, the application may request a random or specific port #
-  TCP Header
	- Source and destination port #
	- sequence and acknowledgement numbers
	- Flags
	- checksum

## TCP Benefits / Services
- Connection oriented
	- TCP first establishes a connection before sending any packets, which is different than UDP which will send regardless
- Full duplex
	- Can send a receive messages simultaneously
- Flow control:
	- If the receiving end is getting messages too fast, there is a means to ensure no messages are lost in the congestion. From the end point / router perspective
- Congestion Control:
	- From the network perspective, ensuring we don't overload the network, as to avoid losing packets.
- Reliability
	- Sequence numbers and Acknowledgements
	- Time out mechanism
	- Flow Control
	- Congestion Control
## Data Transfer with TCP
- Each TCP identified with 4-tuple,
	- Source IP, Source port, Destination IP, destination port
- Acknowledgement
	- Sequence number, Advertised Window
- The Time out is based on some multiple of the Round-Trip-Time, perhaps (just as example) after 3 * RTT we consider the message lost and try again
- Sequence Number
	- Byte stream number of first byte in segments data
	- The sequence number increased by number of  bytes in the message
		- If the previous sequence number was 42, and A sends "ABC", the next sequence number sent to B will be 45. +1 for each byte in the message.
- Acknowledgement Number
	- The acknowledgement number means " I got all the bytes up to message 78, I am ready for 79"
		- Cumulative Acknowledgement, I got up to the number of bytes I am saying. " i got up to 78"
	- If a message is missing, the acknowledgement will only alert for the continuous segment. if 76, 77, 79 are sent. the acknowledgement will only return 77, because 78 is missing.
	- The acknowledgement is the same as the next sequence number the host expects to receive from the other end.
- Premature timeout
	- The sender incorrectly determines a packet to have been lost, causing a resend of a packet, when in actuality If it had waited some few milliseconds more, the acknowledgement would have arrived as expected.
### Sliding Window
- Window is simply the buffer of sent data on both sides of the message. The sender has a buffer it gets from the windows and is getting ready to send. And the receiver has a buffer to ensure it has them in order before it sends the data to its windows.
- If we look at the buffer there is some amount of data that has been acknowledged by the recipient and some amount of data that has been sent but not acknowledged, and some amount of data soon to be sent. In that order
	- If you imagined looking at this buffer as a video this middle portion would be sliding to the right, as the old bytes are acknowledged and overwritten, and the future bytes become the bytes being sent.
- Preventing Buffer overflow
	- MaxSendBuffer and MaxRcvBuffer
	- It is possible that we are sending packets too fast, and with the randomness of gaps, the receiver may not be able to keep its buffer clear, and there are backlogs.
- Silly Window Syndrome
	- Maximum Segment Size (MSS) largest packet TCP will create
	- If we have a message that is < MSS should we wait for the message to grow till it is closer to the MSS? or should we send anyway and risk congestion. If we wait, these small segments might hang around a long period causing more gaps on the receiver side.
- Wrap Around
	- There is of course some limit to the Ack number (2^32), so at some point we need to wrap back to 0 and restart our counting
	- It is possible on high speed networks, think gigabit systems, that this wrap around may be needed in a matter of seconds. In the perspective of the receiver the first and second Sequence # 0 are the same. so we need to protect these collisions from happening
	- Based on the bandwidth and RTT, we need to increase the buffer size, to allow adequate processing time, and to avoid this wrap around error. could be up to 20 MB on fast networks.
- Optimal window size
	- Based on the bandwidth of the sender and receiver but also the RTT. as the RTT gets really small you are clearing your window more often so you can scrape by with a smaller window to act as a buffer
- Initial window size
	- If the window is too small we are wasting bandwidth and too large we cause congestion.
## Three way handshake
- When initially connecting to a server, or another host, there is a three way connection to ensure that both hosts are receiving and sending properly, and agree on the sequence number.
1. Client host send TCP SYN segment to server
	1. Specifies initial sequence #
		- initializes with a random number to prevent hackers knowing you are just starting a connection, that they could then exploit
	2. no data
2. Server host receives SYN #, replies with acknowledgement segment
	1. server allocates buffers
	2. specifies server initial seq #
3. client receives acknowledgement, replies with acknowledgement segment of its own to server. which may contain data.
## Connection Tear-down
- Allow unilateral close
	- TCP must continue to receive data even after closing the sending connection "graceful tear-down"
- Timed-wait at the end of the connection
	- Cannot forget about the connection immediately
	- Its possible a connection is ended, but before the client receives the acknowledgement it creates a new connection, in which it will receive the finish acknowlegment after the second has started. So we add a timed wait.
## Congestion / Flow Control
- Round Trip Estimation
	- Wait atleast one RTT before retransmitting
	- RTT must adapt with the current traffic of the network
	- Can use the Exponential back off [[Test 1 Notes Operating Systems#Shortest Job First]] algorithm
		- a(RTT) +  (1-a)(Old RTT)
	- Retransmission Ambiguity
		- Accurate determination of the RTT can be tricky. If you send a message, and then retransmit before the ack is received then you may think there is a very short response time in reality you were getting a response to your previous message
		- If you have to retransmit a message, do not use the RTT for the sample, because you can't be sure if the timing is accurate.
	- Jacobson noticed that in high load moments the variance is too high for the exponential algorithm to be reliable. He suggests Timeout = RTT + 4(Variance average)
- Flow control Problem
	- Fast send can overrun a receiver, causing packet loss
	- Could send at a pre-negotiated rate that is known to be manageable by the receiver
	- Could have packet preemption like discussed in 4348 OS
- Hard vs soft state
	- Soft state, when a message is sent it might take a slightly different path through different routers and cell towers or networks
	- Hard state, once a path is shown to connect the two hosts, that path will be the ONLY path used for the duration of the interaction.
- AIMD
	- Linearly increases the number of test messages sent to see the limits of the receiver to know the maximum bandwidth that it can handle. 
	- When the send misses an ACK it backs down by half. then continues
	- send 1,2,3. . . 29,30. once it fails: 15,16 . . .
- Slow Start
	- Similar methodology to AIMD but rather than an additive increase, we have a multiplicative increase in the rate packets are sent so we reach the bandwidth in an exponential amount of time. 
	- At some point you switch back to linear increase to avoid too many lost packets
		- This SSThreshold could be any number, one method is half of the window size
- Fast Retransmit
	- If the sender receives three ACKs in a row that all have the same message (I got 30, I got 30, I got 30), then assume that packet 40 was lost because you are not getting that ACK and send it before the timeout ever expires.
# Quality of Service

# Check [[Networks Project Details#Socket Programming]]
