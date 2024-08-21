- I am writing this separate page because it seems some students were struggling with Subnet mask concepts in class today.
## What is subnet mask
- As we have discussed in the class your personal computer is identified with a 32 bit IP number and depending on the class / subnet protocol, some number of those bits are used to identify the network, namely the wifi/ ethernet. The remaining bits are used to identify your unique laptop within the wifi network. 
- For Example, if you type "ipconfig" into command prompt you can see your own Subnet Mask, on campus we all have the same subnet mask of 255.255.224.0
## Why do we use these?
- AT&T or Whatever ISP does not give out a custom number of network vs host bits. You can buy either Class A,B, or C which we have discussed prviously. Where there are 8,16,24 bits designated for the network respectively.
- If a customer, the university for example wants to buy an IP address, they cannot buy one that hosts ~30,000 students, the closest is to buy a Class B which can facilitate up to 2^16 or 65,536 hosts. In which case the University decides to split the network into 4 or 8 subnets that maybe are designated for certain buildings, and thus one router only has to deal with 2^13 (~8100) hosts over a shorter range.
## How can I use subnet Mask
- This subnet mask, 4 integers ranging from 0 - 255 can be converted into binary, to represent this 32 bit IP address of our computer.
- Using a bitwise AND operation you can determine what bits of your IP address are a part of the network's identification vs your own computer's identification among the network.
- There is a specific notation that eludes the need for the Subnet mask and that is 
	- (IPv4 address)/(# of bits dedicated to the network address)
### Calculating Network Bits
- If you have the example IPv4 address (which can also be seen with ipconfig) of 10.182.88.202 and a subnet mask of 255.255.254.0 Then we have all the information to know how many bits my network is allocating for the host, and the network addresses.
	1. First convert both your IP and Subnet mask into binary
		1. IP :        0000_1010  1011_0110  0101_1000  1100_1100
		2. Subnet: 1111_1111  1111_1111  1111_1110  0000_0000
	2. Now perform a bitwise AND operation between the binary IP and Subnet Mask:
		- Output:  0000_1010  1011_0110  0101_1000  0000_0000
	3. What you will notice is that the first 23 bits are the exact same as the original IPv4 address, and the last 9, regardless of the original bits are now zero
	4. This means that our network currently uses the first 23 bits to define the network address, and 9 for my personal host address.
		1. Typically the host number 0 and all 1s are reserved both for the host and in the case the host wants to have a designated broadcast channel.
- This new found number, 23, can be represented in the other format as follows
	- 10.182.88.202 / 23
	- Where the 23 denotes what we calculated above
- If we are asked what is the subnet mask of the following: 192.168.54.132 / 19
	- You can find the subnet mask simply by finding the corresponding binary number that has 19 1s followed by 13 0s.