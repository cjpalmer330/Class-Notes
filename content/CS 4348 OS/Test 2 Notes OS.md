# Main Memory
- The CPU allocates space in the main memory for a process, and its very important the processes cannot overstep their boundaries of memory space
	- This location is defined by a base and limit registers. Base meaning the starting address, limit being the address + some range.
- CPU can simply verify the access is within this range to be a valid request
- Swapping
	- If there isn't enough space in the main memory, you need to take the current process entirely out of the main memory into a secondary storage like a hard drive and bring in the new process
	- Is a big part of the context switching overhead
- Contiguous Allocation
	- The main memory holds the OS, then the rest of the processes can line up back to back in the main memory
	- This simplifies knowing where in memory to look as all processes are guaranteed to be back to back
	- can cause fragmentation
- Segmentation
	- Instead of a process requesting one large block of memory for the whole program it can request several smaller sections of memory that don't necessarily have to be next to each other while still allowing the program to run properly
	- Perhaps stack gets some space, heap gets some space, variable names get some space etc.
	- Need to ensure that you do not request a memory location that is beyond the range of the allocated memory. (max memory location + 5 for example)
- Multiple-partition allocation
	- When a process arrives, it is allocated to memory from a hole large enough to accommodate it.
	- When it leaves, it leaves a hole, empty space for the next task
	- Dynamic Storage-allocation problem
		- Over time there may be multiple small holes in the memory, and none are large enough for the next processes if we don't move them properly
		- First fit: allocate the first hole that is big enough
		- Best-fit: allocate the smallest hole that is big enough
			- requires either ordering list, or searching entire list
		- Worse-fit: allocate the largest hole to the new process
		- If you only allocate blocks of say 4kb, its possible the process only uses 3kb and there is an internal hole in the reserved process memory space.
		- External fragmentation can be fixed with compaction. shuffling memory contents to remove small holes.
- From the user perspective we can have logical addresses for several different use cases to make it much simpler than having to remember the exact offset for every memory access call
## Page Table
- To avoid external fragmentation we create a logical address space called the page table. 
- We split the physical memory into frames, with the size being a power of 2
- We then create blocks of memory called pages which are logical address spaces than be assigned pointers to the frames in main memory. As these pages change we can move the pointers around to completely avoid external fragmentation by ensuring we use up all the adjacent frames
- a frame could be shared by multiple distinct logical pages, from entirely different processes
- Can also share pages in the way we talked about [[Test 1 Notes Operating Systems#Copy-On-Write (COW)]]
- In 64 bit systems, with a 4096 size pages, we would have an address register with 52bits for address, and 12 for the offset. 2^52 is to the order of petabytes which is obviously far beyond any amount of ram, so we need to cut down the size of this page table by many orders of magnitude
- Hierarchical paging scheme
	- instead of just having a logical address plus an offset, you can have say an outer level with some number of bits and another inner level with some more identifying bits.
	- Instead of 1-100, you would have 1.1 - 10.10
	- This brings the space needed for addressing down an order of magnitude, but it would take an additional lookup time for each page we add into the sequence.
- Hashed Page Tables
	- Virtual page number hashed into page table,
	- Each entry in the hash table points to a linked list, with every page that had the same modulus entry in the hash table, until you find the page you're looking for
- Inverted Page Table
	- rather than each process having a page table, track all physical pages
	- decreases memory needed but increases time to search
	- Practically the most efficient in terms of storage space, but quite poor in terms of search time
# Virtual Memory
- Over the life time of your program, some sections are going to be used very frequently, and some may only be ran once on start up and not touched later.
	- So when I start a process, some amount of the program, does not need to be immediately brought into the main memory for the program to run correctly
- Virtual memory that is much larger than physical memory
- floor(Logical address / page size ) = logical page #
- logical address % (page size) = offset within the page
## Memory Management Unit
- Hardware device that at run time maps virtual to physical address
- The user program deals with logical addresses not physical addresses, and the OS finds a hole in main memory for the process.
- Clean / Dirty Bit
	- When swapping out a page some will have been modified since the last time it was copied to the hard disk, and some have not
	- The pages that have not been modified are "clean"
	- This allows them to be overwritten without having to fetch from the hard disk which is much slower
- Valid / Invalid
	- Withing the pages entry in the table there is a bit indicating if the page is within the bounds of the logical address space.
	- invalid can indicate that the page is not within the logical space, either out of bounds of not ever used because don't have enough memory for address space.
	- Valid - In main memory, Invalid - not in main memory
### Demand Paging
- Bring process into memory at load time, and only bring a certain page into memory when it is needed by the OS.
- Page Fault:
	- The page you need is not in the main memory
	- There can be a case where you need a new page in memory and there are no free slots currently in main memory, so one page needs to become the "victim page" that will be sent back to main memory
- The pages in the main memory are considered the "working set", we want all the working set pages to be pages that we currently need, and to make room for the pages we will need in the future.
- Performance
	- Three major activities
		- Service the interrupt
		- read the apge
		- restart the process
	- page fault rate
		- p = 0 no page faults
		- p = 1 every reference is a fault
	- Effective Access Time
		- EAT = (1-p) * memory access + p(page fault overhead + swap in + swap out)
		- How often do we have to get a page from hard disk, and how long does that operation take, to find effectively what we can expect the average operation to take
- Basic Page replacement
	1. Find location of desired page
	2. find a free frame
		- if there is a free frame use it
		- if there is no free frame use a page replacement algorthim to serve the free frame
		- write victim frame to disk if its dirty
	3. Bring desired page into the free frame; update the page tables 
	4. continue the process by restart the instruction that caused the fault
### Page and frame replacement Algorithms
- Frame-allocation algo
	- how many frames to give each process
	- which frames to replace
- Page replacement algorithm
	- want lowest page fault rate
	- a page fault is when the process needs a page, but that page is not currently loaded into the RAM, meaning we have to wait for the much slower hard drive to retrieve the page
- FIFO
	- There is less page faults because we are guaranteeing that the memory is full of pages, but they are not chosen intelligently
- Optimal Algorithm
	- remove the page that will not be used for the longest period of  time in the future.
- Least recently used
	- look at the past rather than future
	- replace page that hasn't been used in most amount of time
	- can have many faults as the past may not be indicative of the future, better than FIFO but still not great.
	- frequently used.
- Second Chance
	- in a round robin goes through page table to look at recently used bit to find one that hasn't
	- Enhanced Second Chance
		- Uses the same method, but uses two bits to not only denote that the page has or has not been used recently, but also tells if the page is clean or dirty. Clean pages of course much more preferable because you don't need to write to hard disk.
- Thrashing
	- If a process does not have enough pages, the page fault rate is very high
		- page fault to get page
		- replace existing frame
		- but quickly need replaced frame back
	- A process is bust swapping pages in and out
### Buddy System
- allocates memory from fixed size segment consisting of contiguous pages
	- allocated in powers of 2
- Array of pointers, where each element in array points to blocks of size 2^n
- starting from the smallest index, increment looking for a block size that is large enough for your process.
	- upon finding one that is exactly the right size you can just claim it
	- if you find a block that is larger than your process, you can split it:
		1. split the next largest block in half
		2. update the pointer array to reflect the state of two smaller blocks
		3. continue splitting until you have a block the exact size you need
	- find only blocks that are smaller than what you need
- when two buddies become free again they join hands
# File Systems & I/O
