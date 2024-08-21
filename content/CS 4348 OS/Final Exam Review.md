# Main Memory
## 8.1 Background
- The cpu only has direct access to its registers, if it needs some data that isn't in the registers the machine code must first move it to the registers in order for the CPU to receive it
- processes are given base and limit addresses to denote their designated space
- we use logical address space to denote a location within a process, and physical to mean a specific location in the hardware
	- the mapping from virtual to physical memory is done by the MMU
-  dynamic loading
	- a routine is not loaded until it is called
	- all routines kept on disk in a relocatable format
- static loading vs dynamic loading
	- static loading, the header file or routine is brought into memory at the beginning of the program as if it was written into the class itself
	- Dynamic loading treats the header files like routines are in dynamic loading where they are only brought into main memory when they are called. unused headers aren't brought to MM
## memory allocation
- the operating system is placed in the lower memory slots from 0, up to the size of the OS. Then user processes when using their logical addresses add the relocation register to find the physical mem location
- contiguous allocation just has one block, where some is the os, and some is the user
- could have multiple partitions, where each user process is assigned one of the partitions, and can never overstep it
- Paging
	- Break physical memory into frames and logical memory into pages
	- instead of the process being slotted in anywhere that it will fit, the process will take up a page at a time, and these pages can be slotted in any available frame, adjacent or not. 
	- Paging completely removes external fragmentation but we expect about 0.5 empty pages per full page of internal fragmentation. smaller pages thus create less wasted space
- TLB
	- in modern computers, the page table could be upwards of a million entries long, and no matter the searching algorithm, that will take a while to search
	- So the Translation lookup table holds <1000 specific page table entries so their frames can be found nearly instantly by the CPU.
		- If there is a TLB miss, the OS tries to put that page table entry in the TLB in hopes it will be requested again soon.
- Effective access time
	- How long does the average frame take for the CPU to access
	- EAT = % TLB hit * TLB access time + % TLB miss * MM access time
## 8.6 Segmentation
- need a separation between the user's view of memory and the phyiscal memory.
- from the user pov we don't care how the memory is stored. we think of programs in terms of functions and classes. whether a sqrt function is stored separate from the data doesn't concern me
- segments have base and limits like pages
	- and each entry has the segment number as well as the offset
# 9 - Virtual Memory
In  the most essentialist terms virtual memory is the separation of memory into logical and physical partitions
This allows for shared memory space as sever processes can map to a space in physical memory as a library
## Demand Paging
- we can limit the amount of transfers that need to happen upon the program running by only calling for the pages when they are needed
- This also means that if a page isn't used, it won't have to be loaded into main memory even though it is declared in the process
- We use valid invalid bits to determine if the requested page is not only within the allowed logical space, but also currently within main memory
- save the current state of the process, so upon page fault, we can go fetch the page from hard disk and restart the process from the state where the memory was  missing
- EAT = (1-p) x ma + p x page fault time
	- p =  probability of page fault
	- ma = memory access time