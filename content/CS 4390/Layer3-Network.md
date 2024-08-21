
- Transferring data from one network to a destination on another network
# Internetworking
- Many networks interconnected, but provide the illusion of having a single network
- Each network has its own addressing and routing
- To transfer from one network to another, need an additional layer (IP)
	- Can send a message to any host through intermediate routers
	- Supports heterogeneity: different hardware, OS, network type, topology etc.
- Ethernet or wifi based on standards has a set position for Types. Based on these types the message will be sent through the IP protocol, or ARP, or RARP, or any number of level 3 protocols
## Internet Protocol (IP)
- Network-level protocol for the internet that operates on all hosts and routers 
- Does not gaurantee that the packets are delivered in a certain time, or in the correct order, or without delay. Only that you can send any file size, and IP will ensure it is sent.
### IPv4 Address Model
- 32-bit address
- Hierarchical
	- Each network has a unique id in the world
	- Each host within each network has a unique id within the network
- And address points to a unique network adaptor
	- Hosts have typically one address
- Three Class Model
	- Class A
		- 0 followed by 7 bits for the Network, then 24 for the Host
	- Class B
		- 10 followed by 14 bits for the Network, then 16 for the Host
	- Class C
		- 110 followed by 21 bits for the Network, then 8 for the host
- Decimal dot notation of 4 integers separated by periods
	- The first integer can tell you if the ip is part of a Class A, B, or C network
- DNS translates the internet domain names (google.com) into IP addresses
- Datagram forwarding
	- Hosts and routers maintain forwarding tables
		- Contains list of IP - next IP hop pairs
		- often has a default route, on routers this table is dynamic
	- How to forward a packet
		- Send directly to the destination if the destination is within one of your networks
		- Send indirectly through another router to the destination if on a different network.
		- Use ARP to get hardware address of host/router
- Adds its own header to each message containing lots of information such as IP Version, Time-To-Live, IP source and destination address, length of message, checksums for error correction.
- Host Configuration needs
	- Default router IP
	- DNS Server IP
### Fragmentation and Reassembly
- Problem
	- Different physical layers provide different limits on frame length
	- There is some maximum transmissable unit, and the source does not know the value of this maximum of the destination, especially if alone synamic routes where many routers are touching the connection.
- Solution
	- When necessary, split IP packet into acceptably sized packets prior to sending over physical link
	- Each fragment has an identifier attached to know what packet the fragment is apart of, with a flag bit to know if there are more fragments following the current one.
	- Only reassemble the packets at the final destination to avoid break down assemble cycles happening multiple times over the path of connection.
	- If one fragment is broken or lost, must drop all related packets.
# ARP & RARP
- Address Resolution and Reverse Address Resolution Protocol
- ARP uses a Logical address to find a MAC address
	- When requesting a mac address the Operation slot denotes if the message is a request or a reply
- RARP uses a MAC address to find a logical (IP) address
	- very rare usage, as its uncommon to know a MAC address without knowing the IP address
## [[Subnet Mask]]
# IP Routing
- Network topology and traffic can change over time so it must be dynamic
- Goal is to calculate the fastest path between each pair of nodes
- Problems
	- Unbounded amount of information
		- The graph connectivity can change, and the queue delay subsequently expands
	- With the billions of devices in the world we need a path from essentially any two computers while still updating periodically for when a computer is shut down or moved to a new network
- Solution
	- Dynamic
		- periodically recalculate route
	- Distributed
		- no single point of failure
	- Abstract Metric
		- Distance in hops, or time 
## Link State routing
- Each router has complete network picture
	- topology and link costs (distance / time)
- Each router reliably floods information abouts its neighbors to other routers, cooperative
	- the router will tell its neighbors who it is directly connected to, and who it has a path to. This can act as a hello message, or there can be an initial hello message. These routers bounce back and forth updating their tables, and they slowly converge on the actual totality of the network
- Each router independently calculates the shortest path from itself to every other router
	- Dijkstra's
- OSPF uses Link State. Port #89
### Link State Strategy
- Send to all nodes, not just neighbors, information about directly connected links
	- causes a gradual build up of Link State Packets
	- uses the broadcast ip, which all connected routers and hosts will be able to see
- LSP ( Link State Packet )
	- id of node that created the LSP
	- a list of the costs of the links
	- sequence number
	- time to live for this packet
		- used to deal with sequence number wrap around
		- prevents packets from living forever in the network
		- Who is connected to my network is constantly updated if the packets from a while back are removed
- Reliable Flooding
	- generate a new LSP periodically
	- forward a received LSP to all neighbors except the sender
	- store only the most recent LSPs of feach node
	- SequenceNo resets on reboot
	- Periodically decrement TTL of each SEQNO
		- Discard when the TTL is 0
### Sending LSP
1. The initial Node sends a broadcast stating its IP and that it is connected.
	- All the routers directly connected to the Node A will receive the broadcast and add Node A to their tables.
2. Then Node A with its table filled out will send broadcast to its neighbors " I am Node A and have a direct connection to Nodes 2,7,8".
3. Node A will also receive a similar message, as such it can know "I have a direct connection to 8, which has a connection to 6"
4. After some time Node A will see that the LSPs are giving no new information, at which point it will perform Dijkstra's algorithm to find the shortest path between the nodes to any of the nodes that have a connection to A.
	1. This state of no new information is called "Convergence"
## OSPF
- Last updated in 2008 with RFC 5340
- It is by far the most popular routing protocol and is currently up to IPv6 Version 3
- implementation of link state
## Distance Vector
- Much like the graph theory we studied in data structures, instead of having a whole image of the network each node only knows its distance to another based on its own connections. 
	- When A leaves the network, C will only know because B will update its distance to A by 1 and they will subseqintally paddle back and forth incrementing the distance until it is realized A is missing
	- max hop count of 16, after which it is considered missing
- Each node has no idea of the path, only that to get to the destination C it must first send to the node B
### RIP - Routing Information Protocol
- max hop of 15, uses UDP services, meaning it is on layer 5 and used port 520
## BGP
- Internet is subdivided in to Routing Domains
	- Autonomous Systems
	- There are currently > 15,000 ASM's
- Each AS has a unique 16 bit ID
- Intradomain - Within an AS
	- can use an proprietery algorithm to handle communication
- Interdomain - Between ASMs
	- uses standard global algorithm
- cuts down the amount of traffic every router must handle significantly, and allows for greater heirarchy of powers. Perhaps an office building with 100s of employees all connect to one router, and that entire building has one point of access to the world which means 1/100th of the traffic introduced to the network
### Standard Interdomain Routing Protocols
- Very complex, before 1990 internet had a backbone tree structure,
- Three categories of AS
	- Multihomed:
		- A AS that maintains more than one connection to an outside ASM
		- Try to avoid through traffic by not telling everyone who they are connected to
	- Stub AS
		- There is only one connection to the rest of the world, And the AS does not serve any hosts of its own. That way attacks are easier to control as there is only one point of failure
	- Transit
		- Acts as a router between other ASs
		- In between the hosts, and announces that it is connected to everyone so it can be used as a transit bus
	- Backbone Network
		- No end customers are connected to the ASM. 
		- However it is connected to several ASM to provide tranist
- We want to avoid loops because it is possible that the packet gets sent to the wrong ASM and then the packets wont ever reach their desired location as they go from ASM1->ASM2 -> ASM3
### BGP-4 Standard
- Assumption is that the internet is an arbitrarily interconnected set of ASMs
- Each AS has
	- Border routers ( 1 or more )
		- Connect an AS to the Internet
		- used for default external routing
		- uses the routing table to send messages from the AS to the internet
	- BGP Speakers
		- Routers that participate in the inter-domain routing protocol
		- Advertise for the network a full path list of the ASM's reach
		- Loops avoided by not choosing neighboring path if it has your own ID
		- These advertisments are echoed from neighbor to neighbor. Host 1 is connected to ASM1. ASM1 tells ASM2 and ASM3 that it has a path to host 1. ASM2 tells its neighbors it has a path to ASM1
- BGP uses TCP to guarantee delivery.
- Keep alive messages are sent periodically
	- If a path is withdrawn the ASM ought to explicitly declare the withdrawal
- BGP Import / Path/ Export
	- Import
		- Of the paths offered by my neighbors which will i accept
		- Entirely a business decision
	- Path
	- Export
		- Will i tell my neighbor that I have a path to a certain ASM or will i hide that information
		- Can be used as a business decision because I have competition with that ASM like AT&T vs Verizon or maybe they charge more for through traffic
# IPv6 Address
## Network Address Translation
- Some specific IPs are designated as non-unique and thus can be used many times over, expanding the number of devices that can have an IP
- Every home has a 192.168.0.0 address. and the router is given a unique address, or your company is given a unique address. And the router takes all input traffic and sends it to the computers within the network with these IPs that another home also has.
	- The send decides which computer by a port number. which can also repeat
## Problems with IPv6
- With globally unique addresses it removes some amount of safety.
- As well, there is less concern for the number of IPs are being used, and messages being sent. This means that every car sensor, every refrigerator, ever ring camera can have its own IP sending messages which crows the network.
## Benefits of IPv6
- No more NAT needed
- Easier address management, easy auto-configuration
- Room for several layers of hierarchy and routing aggregation