# DHCP
- assigns IP to devices on address changes, every time you connect to a network you get a new one
- there is a lease time, and if you leave after that lease time the ip will be reclaimed and can be reassigned to another computer
- a newly booted hosts sends a DHCPDISCOVER message to the broadcast address.
- Because having a DHCP server for every business or floor of an administration is laborious, you can use relays that sole job is to relay discover messages to DHCP server
- DHCP is derived from BOOTP
# Error Reporting (ICMP)
- defines a collection of error messages that are sent back to the source host whenever a router or host is unable to process an IP datagram successfully.
- along with defining many error messages that can tell hosts what the problem is, it also provides the debugging tools of traceroute and ping
# Address Resolution ARP
- each host has a table that translates MAC addresses into IP addresses
- entries time out roughly every 15 minutes.
- the currently stored map is known as ARP cache or table
- ARP sends a broadcast request for an IP and if it matches the host that received it, the receiver replies with the corresponding MAC address.
- if the IP is already in the table, ARP refreshes timer
# UDP
- does not guarantee delivery of packet to the destination. uses "best effort"
# Subnetting
- network numbers managed by ICANN
# TCP
## TCP Congestion Control
Overall principle is for each node to gauge how much traffic is in the network and try to not surpass the network's limit
- uses the arrivals of ACKs to count the number of packets it has sent vs received to keep it constant
	- This "self clocking" paces the packets
- How do we know the networks limit? since it is always changing
	- Additive increase, multiplicative decrease
		- every time a packet is successful, it adds 1 to its congestion window size count
		- every time a packet is lost, the congestion window is 1/2
	- slow start
		- when a packet is successful, the congestion window is doubled
		- usually used at start of connection when no data is known of limit
		- after some point it returns to a linear growth and retraction
	- Fast retransmit and fast recovery
		- if at any point the sender notices three ACKs of the same packet, it resends, even if that is before the RTT
- can have problem over wireless connections where bit errors are detected as packet losses, which causes the TCP to slow to a crawl unnecessarily
# Quality of Service
- as data sizes increase we need more data to  be able to transfer at faster rates
	- time sensitive applications are call real-time applications
	- real-time apps need assurance from the network that data is going to arrive on time
- A network that can provide different levels of service (real time vs best effort) is said to support QoS
- non-real time also called traditional data apps or elastic
	- email, web browsing
- intolerant real time apps can get guaranteed serrvice
# routing
 ***The purpose of routing is to create a routing table***
- routing can be boiled down to a  graph theory problem
	- the caveats being that some edges can disappear and new ones can be created
	- as well as the weights of the edged changing over time
## Link State 
- each node is able to find the distance to its neighbors, and their state.
- Link state relies on eventually every node being able to create a complete map of the network
- each node floods the network with its id, list of connected neighbors, sequence no, and TTL
- uses periodic hello packets to lets its neighbors know it is still active. 
- LSP packets carry TTL, meaning old state information is automatically ignored from the network, preventing false images of the network being created
- Once the network image "converges" the router can perform dijkstra's algorithm to find the shortest path to all nodes.
- stabalizes quickly from removed nodes, and doesn't generate an abundance of traffic
	- requires high storage on each node to keep track of the topology
### OSPF
- Most widely used Link state protocol
- OSPF has an authentication message to ensure the routers getting and sending information are to be trusted
- allows somain to be partitions into areas, this means a router doesn't need to know how to reach nodes outside its area, only how to reach said area
- OSPF allows different paths to have the same weight and thus distribute the load across said paths.
## Distance vector routing
- each node constructs an array / vector containing the distances to all other nodes it knows of, and distributes that to its neighbors.
	- a down node is given infinite distance
	- similar to the bellman ford / floys warshall algorithms that we discussed in data structures.
- each node automatically sends an update periodically "keep alive"
- triggered update happens when a node notices a node has been disconnected
### Routing Information Protocol
- RIP is the most common used protocol implementation of Distance vector.
- it was distributed iwth BSD version of unix.
- send a keep alive every 30 seconds
- RIP supports subnet masks
- limited to hop distance of 15, and thus considers 16+ hops to be non-reachable
# BGP
- The Exterior Gateway routing protocol
- BGP is a protocol used between ASes
- BGP is a form of Distance vector, but rather than using minimum distance, policy is used to pick the route. 
	- BGP also has to keep track of the specific route that it took along with the cost associated with it, something RIP didn't have to do intradomain
- 
# IPv6
# Virtual Circuit
- provides a logical point-to-point connection along a leased connection line
- part of the path is stored in the routing table so that it doesn't have to searched for again every time we want to send a packet.
- each packet carries an identifies telling which cirtual circuit it belongs to, and upon arrival the vc dissipates
# [[Mobility]]