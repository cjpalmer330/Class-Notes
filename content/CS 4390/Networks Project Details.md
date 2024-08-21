# Five measures of a protocol
1. What layer is my protocol going to sit at / on top of
2. What services does it provide
3. design the header
## Part 1 of Project
- Explain in a word doc all your plans for the project, what protocol you will use,
- about 10 days
## Part 2 of Project
- Have until August to complete the programming of the project. 16-18 days
- No GUI required, can just use the linux terminal or windows PowerShell.y
# Socket Programming
- Socket is the API to access transport layer
- has a ip number port concatenation
	- Ex. 192.168.5.4:1234
- (local ip, local port, foreign IP, foreign port)
- TCP uses Stream socket interface, UDP uses datagram socket interface
- How to use socket
	1. Setup socket
		1. Where is the remote machine
		2. What service gets the data (port)
		- Both client and server need to setup a socket
			- Windows WSL (Windows Socket Library)
		- Client setup
			- AF_INET is the IP domain
			- Type - SOCK_STREAM to use TCP
			- Protocol - 0
			- EX. int socket = (AF_INET, SOCK_STREAM, 0)
		- Server setup
			- port number > 49152 for private ports
	1. send and receive
		1. send -- write
		2. recv -- read
	2. close the socket
- Use threads to avoid the blocking problem
- two threads on each end, both having an infinite while loop. They wait and loop until either you send or receive a message. When you type "quit" the loop hits a break; statement to exit the loop
- There is an ip call to get my current ip address
- loop around address 127.0.0.1
## API Calls
- Blocking API Calls
	- the program does not continue until the API is resolved
	- system.in or cin stops the program until the user inputs something on the keyboard
	- sleep is always a blocking call and can be used after a non-blocking API call to force the function to block so that other functions or processes can work while we wait for input
- Non-Blocking API calls
	- non blocking API calls will continue regardless if the function is answered