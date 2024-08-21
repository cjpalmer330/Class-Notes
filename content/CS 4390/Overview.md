
## Network Topologies
- Topology refers to the physical layout of the network
- Bus Topology
	- Uses a trunk or backbone to which all of the computers on the network connect
	- Cheap and easy, but network disrupted when it needs to be extended
- Ring Topology
	- Data travels in circular pattern from computer to computer
	- Easy to find faults in the system, expansion causes disruption
		- A single fault can break whole system
- Star Topology
	- All devices connect to a single host device / hub
	- a failure in a cable only takes down one computer
	- requires lots more cable and more expensive
- Mesh topology
	- Every computer connects to every other computer
	- redundancy in the case of cable failure and can be expanded without disruption
	- very expensive to implement in larger systems
## 7 Layer OSI Model
- Encapsulation
	- Each highest layer protocol creates messages and sends them via its lower layer protocol
	- Each protocol adds its own control information in the form of headers and trailers
- Multiplexing
	- Use protocol keys/numbers in the header to determine correct upper-layer protocol
### Purpose and History
### Layer 1: Physical
- Medium, interfaces, puts bits on medium
	- Bits, interfaces, hubs
- PDU: Bits
- Representation of the bits, and the movements of individual bits from one node to the next
- Deals with data rate, and synchronization of bits
### Layer 2: Data Link
- Media access control, physical address
	- Ethernet, Token Ring
- PDU: Frame / Packet
- Layer 2 has a Physical address ( MAC address)
- Deals with flow control to not overwhelm the receiver
- Deals with error control by added error detection/correction bits
- Deals with Access control, how multiple nodes share the same data
### Layer 3: Network
- End to end delivery, logical address
	- IP, IPX, ICMP
- Logical communication between hosts
- PDU: Packets
### Layer 4: The transport Layer
- Flow Control, Error detection/correction
	- TCP, UDP, SPX
- PDU: Segment
- Responsible of deliver from between processes
- Service Point Address, called port, used to track multiple sessions between the same systems. SPAs are used to allow a node to offer more that one services.
- Must specify between TCP and UDP
- Reassembles the segments of data 
### Layer 5: Session
- Communication between hosts
	- BOOTP, NetBIOS, DHCP, DNS
- PDU: Data
### Layer 6: Presentation
- Data representation
	- ASCII, JPEG, PGP
### Layer 7: Application
- Provides services to user apps
	- HTTP, FTP, SMTP