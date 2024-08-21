# Operating Systems Overview
- Most important resource is the CPU processing
- Most important metric is the throughput of tasks *From the user perspective*
- Context Switching
	- When you switch from one task to another, there is some cost involved, namely having to load from memory and book keeping to ensure you execute everything properly.
- Process Management
	- Creating and deleting both user and system processes
	- Providing mechanism for process synchronization
	- Providing mechanisms for process communication
	- Providing mechanisms for deadlock handling
- What is a process? task ? Program?
	- Process is an instance of a program
- Operating system creates and manages threads to prioritize what tasks need to be completed when to maximize efficiency. Prioritizing processes.
- Memory Management
	- When to move data from main memory to RAM and to the cache
# Chapter 2
- When you open or read a file, the kernel returns a FD ( File descriptor ) which is a index into a file table, gives file data into the declared buffer, which is in the user area of memory, away from the kernel
	- File descriptor is unique to the instance of the open operation. So that in the case you want to have each opened instance to have different permissions they don't conflict.
- Write will overwrite the memory space, and if the write goes beyond the length of the file, it will add to the length of the file, making it longer
# Chapter 3 Processes
- Hard disk is divided into blocks, and we read and write from the hard disk in whole blocks, perhaps 512 bytes or 1 kilobyte etc
- Functions go in the stack memory so that the variable scope is maintained, where after the functions is completed, we can return to the next line with the previous scope of the variable X for example
- depending on the context switch time, it may be more efficient to have longer periods before switching between processes in order to minimize the total time we are switching. But of course this is pushed from the other direction where it could be more efficient to switch and work on another task while one is being processes or waiting for memory to be fetched
- Interactive tasks like writing from the keyboard need fast context switching so that we see the key press show on the computer immediately
- Compute Intensive tasks vs I/O Intensive task
	- Web browsing for example, most of the time is spent doing IO related tasks, like fetching HTML, taking inputs from users
	- Finding the millionth digit of Pi is compute intensive, there is not much input but much time is spent calculating
	- Relative in terms of the computer or server or platform you have as one computer might have a SSD vs Hard disk for example so IO doesn't take as long
	- Parallel and sequential tasks themselves are sequential. if there is a 80% 20% problem the sequential must be ran in whole then the parallel
- Multiprogramming - After the cpu finishes the compute of task 1, start task 2 compute while the I/O of task 1 is completed by the hard disk
## Definitions
- Page
	- A page or memory page is a virtual block of fixed-length, continuous size of memory. Described by a single entry in a page table that the CPU can access
- Cascading Termination
	- When we want to kill a process, we first kill its lowest hanging descendant. Then move up one level, to the parent of the just deleted process, and repeat until all children are killed, in which case we can terminate the desired process.
## Fork processes
- A parent process can create a child process with the fork() call, and the child has the exact same image and function as the parent. The child is given it's own id of 0, so that it can know it is the child and be given specific commands.
	- The parent can have the pid of the child assigned to a variable in order to kill the child process once it is complete.
- exec() can be used after a fork() to replace the child process' memory space with a new program
- The child is given pointers to the pages that the parent has in the moment that the fork is created, this means the parent can fork a child, then change it's pointers to new pages, then upon creating a second child, the two children will have access to different pages in the memory
## Argument utilization
- When you input a command like Cat xyz.c, the command is Cat and the argument is the file name
	- argv is the array/vector of all the arguments. Where in the above example argv\[0] would be "Cat" and argv\[1] would be "xyz.c"
	- argc is the number of arguments in the entered command
- argv can be used in the actual code to say "printf(argv\[3])" for example
## Copy-On-Write (COW)
- Initially, both parent and child processes share the same pages in memory
	- If either process modifies a shared page, only then is the page copied
- Only modified pages are copied
- Each process does not store an entire copy of the pages, rather it only stores a pointer to the pages in the physical memory which is shared between all the processes. This means that when process 2 for example needs to modify Page A, it will create a copy of Page A in the physical memory to a new location A'. Then process 2 will move its pointer to A', while process 1 maintains it's pointer to page A, where the data is left unchanged.
	- When the child process is done with it's own pages, such as Page A', it will delete the additional pages to make more room for the next task. The OS only deletes pages that have only 1 pointer, namely the process that is being killed. So as not to remove access from another process. In the case a page has two pointers, the pointer count is decremented by 1 so that an accurate tally is kept
## Process termination
- Process executes exit() system call, and returns the data from the child to the parent via wait()
- process can force terminate the child with the abort() system call
	- can be useful if the child exceed resource allocation, or task is no longer required, or the parent is exiting so the child must be removed first.
- Clean up of the child process is a responsibility of the parent process. If the parent does not invoke wait(), the child may terminate but not fully clean up by parent, in which case the killed child is called a zombie process, which means the memory has not be deallocated, but the process is no longer running.
- If we say create a child, and then forget the pid, and the child process never terminates on its own What will happen?
	- In the event I want to close the forgotten process, the logout or shut down sys call is considered the grandfather of the process, in which case all the orphan processes will be terminated before logging out through cascading termination
- If a process is executed with "nohup" command, the process will not be terminated even when the parent is terminated, in which case the init process will adopt the orphan. It will persist until the computer is shut down.
- Sys admins can sometimes run an accounting software that will automatically delete programs running for too long, regardless if they were ran with "nohup" or not.
## Interprocess Communication (IPC)
- Two methods are message passing and shared memory
### Message Passing
- Send -> enqueue into the pipe. Message receive -> dequeue from the pipe.
- There is a separate queue for each direction of messaging between two processes. These queues use the built in queue dequeue process in the kernel. 
- Message size can be fixed or variable
#### Pipes
- When a pipe is created it has two file descriptors, fd\[0] is the insertion end of the pipe, fd\[1] represents the exit.
- If the pipe's intention is for sending from the parent to the child, we must close parent's access to fd\[1] and child's access to fd\[0] to ensure one way communication and avoid read-write errors.
- forking must be done after one or both of your pipes are created, otherwise the child will not be able to see the pipes.
### Shared Memory
- Some location in main memory that both processes can read and write to.
	- This means writes can be overwritten, which may or may not be desired. Where in a case of two consecutive writes, the first write cannot be read by anyone.
# Chapter 4 - Threads
## Motivation
- Can simply code and increase efficiency
- can allow concurrent tasks to be occurring. These can spread up compute time
- sort of a "light weight" processes, as they allow multi instances, but all on their own compute
## Benefits
- Responsiveness - allows continue exec if part of process is blocked
- resource sharing - threads share resources of process, easier than shared memory or message passing
- economy - cheaper than process creation, thread switching lower overhead than context switching
- scalability - process can take advantage of multiprocessor architectures
## Multicore Programming
- Parallelism:
	- doing multiple tasks at the same time
- Concurrency
	- Supports more than one task making progress
	- A scheduler could provide concurrency with a single core that does some compute while it wait for memory for another compute sequence.
- Amdahl's Law
	- speedup <= 1/(S + (1-S)/N)
	- As the number of cores increases, the speedup from implementing parallel vs serial approaches 1 / Serial speed
### User Threads vs Kernel Threads
- user thread management done by user-level threads library
- kernel threads are supported by the operating system
- Many user threads mapped to a single kernel thread.
	- That means if one of the user threads blocking causes all the user threads to block
- Eventually each user level thread maps to a kernel thread, which allows for more concurrency but if the user creates a large number of threads it can burden the operating system
- Two-Level Model:
	- M user threads are mapped to N kernel level threads
- Implicit Threading
	- Growing in popularity as numbers of threads increase, program correctness more difficult with explicit threads
	- Creation and management of threads done by compilers and run-time libraries rather than programmers
- Thread Pools
	- Create a number of threads in a pool where they await work
	- Usually faster than creating a new thread for each task
	- Allows the number of threads in the application to be bound to the size of the pool
### Fork() and exec() in threads
- exec works the exact same in nearly all cases
- Does fork() duplicate only the calling thread or all threads?
	- In some versions of unix there are two versions of fork(). One that will copy all the threads of a process, and another that will analyze the use of the child process, and in the case the child is only executing some small function, only a single thread will be copied
### Signal handling
- Signals are used in UNIX systems to notify a process that a particular event has occurred
- A signal handler is used to process signals
	1. Signal is generated by particular event
	2. signal delivered to a process
	3. Signal is handled by one of two signal handlers
- Every signal has default handler that kernel runs when handling signal
	- User-defined signal handler can override default
	- For single-threaded, signal delivered to process
- If there are multiple threads, where should signals arrive?
	- Perhaps you have one thread tasked with handling all signals
### Thread Cancellation
- Termination a thread before it has finished
- Thread to be canceled is target thread
- Two general approaches
	- Asynchronous cancellation: terminated the target threat immediately
	- deferred cancellation: allows the target thread to periodically check if it should be cancelled
### Linux Threads
- linux uses the term tasks rather than threads
- Thread creation is done through clone() system call
- clone allows a child task to share the address space of the parent task if you want, but can be controlled
	- CLONE_FS: file system info is shared
	- CLONE_VM: same memory space is shared
	- CLONE_SIGHAND: signal handlers are shared
	- CLONE_FILES: the set of open files is shared
- struct task_struct points to process data structure
# Chapter 6 - Process Synchronization
- Processes can execute concurrently,  may be interrupted at any time, partially completing execution
- We want to interleave processes to maximize CPU utilization, while trying to solve race conditions where two or more processes access a shared block of memory at the same time, causing inconsistent results in the final output.
- Concurrent access to shared data may result in data inconsistency
	- Concurrency doesn't necessarily mean at the same time, could be done with interweaving of tasks
- Goals
	1. Mutual Exclusion
		- At any given time only one process should be in its critical section
	1. Progress
		- If no process is executing in its critical section and there exists a processes who is ready, then the selection cannot be postponed indefinitely
	2. Bounded Waiting
		- A bound must exist on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted
	3. Handle Race conditions
## Critical Section
- A piece of code that is shared between multiple processes such as variables and data structures, and must only be operated by one process at a time.
- No two processes should be in their critical sections at the same time.
### Taking Turns
- An example of handling this problem is to have a do while-loop, where each process is arbitrarily assigned a turn variable, i.e. turn = i. Then a while loop is running continuously with the condition of turn == some variable. In the while contains the critical sections, then after completion the turn switches.
- This only really works if there is a regular switching between multiple processes
	- If A wants to do a lot of work in the critical section, it does some task, then must wait for B to do something before it can enter again, even if B doesn't need the critical section for a while
### Peterson's Solution
- Solution to the Taking turns problem where A must wait for B to take its turn first
- Two processes share two variables, a turn variable and a Boolean flag array used to indicate that a process wishes to enter the Critical Section
- How a process enters critical section
	- First Process i raises its flag, to wish to enter
	- Sets turn to j, as in giving the turn to the other processes
	- If j is not interested, i.e. its flag is false, process i continues into critical
	- on exiting the critical section, process i will lower its flag
- What if both processes raise flag at the same time?
	- Because both processes set the turn to the other threads turn, it is not possible for the while loop to be false for both processes, meaning which ever one was set to turn slightly later, will enter the critical section and then pass turn.
## Synchronization Hardware
The following are ways to try and prevent read write collisions but they have their own drawbacks
### Definitions
- Deadlock
	- Two or more processes are waiting indefinitely for an event that can be caused by only one of the waiting processes.
- Starvation
	- A process may never be removed from the semaphore queue in which it is suspended
- Priority Inversion
	- Scheduling problem when lower-priority processes holds a lock needed by higher-priority process
	- Solved with priority-inheritance protocol
- Locking
	- Protecting critical regions via locks
### Test-and-set
- A boolean, atomic function where a pointer is passed as a parameter
- A boolean variable is set to the value of target
- target is set to true
- When you call set, the value of the function will be set to 1 and the previous value will be returned in an atomic operation. calling test will see if the value has changed
- the original value of target is returned
- Problem is that the next thread is randomly selected. So its possible if you have multiple threads trying to set the lock, one gets unlucky and isn't ever chosen.
### Compare-and-Swap
- Has three parameters, value pointer, an expected value, and a new value
- set a temp variable to the value of the pointer
- If the value is == to expected value, set the value to the new value
- return the old value
- also has problem of lack of fairness
- Essentially when trying to change a value the compare and swap will grab hold of the memory location and ensure that the value hasn't changed in this time, and then set it to the desired value before releasing the memory
### Bounded-Waiting Mutual Exclusion
- Implements [[#Test-and-set]] to ensure only one person has the lock at any given time, and that the critical section is accessible by whomever needs it
- However instead of the next process being chosen randomly, there is a circular queue with every process in the queue
	- When a process is done, the OS moves along the queue looking sequentially who is raising their flag, if one is raised they pass the lock
	- If no flag is raised, meaning no one is waiting, the lock is set to false
	- at which point the original, waiting process is free to use the resources again if they so desire as long as no one else is waiting
### Semaphore
- Semaphore S is an integer variable, acts much like a flag in the other methods
- Can only be accessed via two atomic operations
	- wait and signal
- wait(S) { while S <= 0; S--;}
- Signal(S) { S++ }
- Set up in a format of wait(S) , critical section, signal(S)
- Avoid busy wait constraint of other methods, in the wait the process is placed into a queue and can do other things until it is signaled
- Counting Semaphore
	- The Semaphore is an integer value that can range over an unrestricted domain
	- If the value is 0, you are blocked, otherwise it is decremented and you are allowed to enter. This creates a queue or priority queue
	- Can be used when there are a finite number of resources that can be used at once. 
- Binary Semaphore
	- The semaphore is an integer that can only be 0 or 1
	- If the process does a down semaphore, and the semaphore is already 0, you block. If the value is 1 when you down, you are allowed into the critical section. Upon finishing, you increment semaphore by 1
## Classic Problems of Synchronization
### Bounded Buffer Problem
- N buffers, each holding one item
- binary Semaphore "mutex" initialized to 1
	- "lock" and "unlock" states
- counting Semaphore "full" initialized to 0
	- Essentially the count of number of elements in the buffer, wait can be used to decrement, signal to increment
- counting  Semaphore "empty" initialized to the value n
	- The number of empty locations in the buffer, opposite use as the full semaphore
### Readers and Writers Problem
### Producer Consumer Problem
- Consumer blocks if the queue is empty, and the Producer blocks if there is a full queue by some limit
- But we cannot allow the producer and consumer to access the queue at the same time, because it can create a race condition
### Dining-Philosophers Problem
- There are 5 Philosophers sitting around a circular table, there are 5 plates in front of each philosopher, and a chopstick in between each philosopher. The philosopher can have three states, thinking, eating, and hungry.
- The philosopher needs both chopsticks either side of them in order to eat, and we want to maximize the time the philosophers spend thinking, and minimize the time they spend hungry
- The problem is if all of them become hungry at the same time, at which point we say that they should always try to grab the higher index chopstick first, then through a cascading fall all will get to eat.
- This problem can create deadlocks if not implemented correctly, where if every philosopher grabs one chopstick, no one has both, so no one can eat. But none of the philosophers are going to let go of the chopstick so they are all infinitely waiting for the guy to their right to release the chopstick.
# Chapter 5 - CPU Scheduling
## CPU Intensive vs IO Intensive
- There are some tasks that spend more time calculating ( finding the millionth digit of PI), and some that spend most of their time handling inputs and outputs (word processors).
- As you add more cores, or threads, or the CPU gets more advanced your CPU time of calculating gets exponentially faster, and at some point you may switch what was once a CPU intensive task into an IO intensive task, at which point there is no benefit in speeding up the CPU any further
## Shortest Job First
- Associate with each process the length of its next CPU burst
- SJF is optimal - gives minimal average waiting time for a given set of processes
	- The difficulty is knowing the length of the CPU request
	- Can possible starve long jobs as short jobs keep popping into the queue
- Uses a priority queue, that pushes the shortest tasks to the top
- How do we know the length of the task?
	- Could count lines of code, not accurate with multitasking and memory limitations
	- Could ask programmer, but this allows programmer to lie
	- Could guess based on the previous bursts
	- Exponentially Weighted Moving Average
		- More recent task times are given a heavier weight in the calculation for the length estimate the next task will take.
		- Tau(n+1) = At + (1-A)Tau(n)
			- If alpha is 0, the prediction will never change, the recent task has no affect on the prediction
			- If alpha is 1, the prediction will be length of the previous burst, regardless of the previous prediction
## Shortest Remaining Time First
- Is a pre-emptive mode of scheduling
- Non pre-emptive vs pre-emptive
	- If a task is running, and has an estimated 8 units of time left, and a new task arrives needing only 6 units of time should we continue our task or switch?
	- Pre-emptive way of scheduling would stop the process and execute the new task
## First in First out
- Which ever is assigned first, is done first
	- Simply processes are placed in a queue
	- Can have a problem where one task takes 5 seconds to complete, causing a task that would take 2ms to wait for an unnecessary amount of time.
	- Very simple and guarantees that every task will get completed in a fair amount of time
## Round Robin
- Each process gets a small, but even amount of CPU Time. usually 10-100ms, in a circular queue
	- If a process needs more than the quantum given, they must wait for all other processes to get their turn before they get another quantum of time.
- To avoid excessive context switching we can look at the length of tasks, and pick a quantum that covers ~80% of tasks for example so that most tasks will not have to wait for a second time slot
### Multilevel Queue
- We have two separate queues
	- Round Robin queue for the high priority - foreground / interactive tasks
	- FIFO queue for the lower priority - background tasks
- This ensures that interactive tasks are constantly getting work done as to be responsive as possible, but tasks that don't have to be done immediately can be done FIFO to minimize context switching cost.
	- How do we know if a task is interactive or not?
		- Starting assumption is that every process is interactive, and placed into a small quantum round robin queue
		- If the process is not finished, then the process is placed in a slightly longer quantum round robin queue
		- If the process is still not done, then it is determined to not be a interactive task, and is in fact a CPU intensive task and is placed in a final third FIFO queue which can be ran in the background.
	- This third level First Come First Serve queue is only taking up CPU power when there is available resources not being used by the two RR queues
## Multi-Processor Scheduling
Of course modern CPUs do not only have a single core that can only do one task at once. modern CPUs have 4-16  cores and a dozen or more threads that can work concurrently
- Need Load Balancing
	- Periodic task checks load on each processor, and CPUs which are overwhelmed have tasks pushed to available CPUs
	- Should the full computer look for a free computer? or should the free computer look for one that is overwhelmed
		- When most computers in a data center are loaded, it is better to have Pull based migration, as the likelihood that the empty CPU will be able to find an overwhelmed CPU in the first or second try.
## Real Time CPU Scheduling
- Soft real-time systems
	- no guarantee as to when critical real-time process will be scheduled
- Hard real-time systems
	- task must be serviced by its deadline
- Two types of latencies affect performance
	- interrupt latency
		- Time from arrival of interrupt to start of routine that services interrupt
	- dispatch latency
		- time for schedule to take current process off CPU and switch to another
- For hard real-time systems, CPU utilization is not the most important metric, rather meeting the deadline is paramount
### Rate Monotonic
- Higher frequency / shorter periodicity of a task, causes the task to have a higher priority  in the scheduling
- If processes cannot be scheduled by rate monotonic scheduling, there is no other fixed-priority, systematic scheduling algorithm that can complete all the tasks within a given period
### Earliest Deadline First Scheduling
- Priorities are assigned according to their deadlines.
- The processes could run "out of order" meaning P1, P2, P2 depending on how the deadlines line up
# Chapter 7 - Deadlocks
Deadlocks are of course when two or more processes are unable to progress with their task by any means because they are either in a cyclical resource request with another process, or they are both requesting the same resource with neither letting go.
Some operating systems, namely Linux, UNIX, MacOS, and Windows often times just ignore deadlocks because the methods to detect and fix deadlocks is somewhat costly, and with the rarity of deadlocks the compute cost often isn't worth the loss in resources.
## Prevention
- Restrict the ways resource requests can be made.
- Mutual Exclusion - not required for sharable resources, but locks resources
	- This can be very inefficient where a process locks resource 1 and holds that lock while it waits to get the lock on resource 2. Meaning 1 is sitting idle
- Hold and Wait - must guarantee that whenever a process requests a resource it does not hold any other resource
- No Preemption
	- If a process locks 1 resource and is waiting on others, the OS will release R1 and force the process to wait for all of the resources to be free
- Circular Wait
	- Impose a total ordering of all resource types, and require that each process requests resources in an increasing order of enumeration.
	- This guarantees that two processes can never form a cycle, because they always try to grab the resources in the same order. i.e. R1, R2, R3. So there will never be a process holding R3, waiting for R1.
## Avoidance
- Less restrictive than Prevention, but still proactive
- Safe State
	- When a process requests some resource the OS determines if immediate allocation of R1 would put the system at the possibility of a deadlock.
	- A Safe state is that all processes have a way to get their resources without a deadlock
- Avoidance does not allow transactions that move into an unsafe state
- When you have a single instance of a resource we can use a graph method to detect cycles
	- Resource-Allocation Graph
	- dashed arrows mean that the process will in some point in the future request a resource. Solid pointing to the process means it has locked that resource. pointing to the resource means a request has been made.
### Safety Algorithm
1. Let work and finish be vectors of length M and n 
		1. Work = Available
		2. Finish \[i] = false for i = 0,1,2... n -1
	2. Find and i such that both:
		1. If a process is not done, and its needs are less than what is currently available, let that process finish its work before looking for another task. A sort of mutation of SJF
		2. Finish \[i] = false
		3. Need(i) <= Work
		4. if no such I exists, go to step 4
	4. Work = Work + Allocation(i)
		1. Finish \[i] = True
		2. go to step 2
	5. If Finish \[i] == True for all i, then the system is in a safe state
### Banker's Algorithm
- 5 processes, 3 resource types A,B,C with their own number of instances
- Across all processes add up how many instance of each resource type is being allocated, and subtract that from the total number available, and you will know how many remaining instances of A,B,C or any other resource you have left to ration.
- When the process starts it tells the OS what its maximum needs are. The Maximum needs for an individual process can never exceed the maximum resources available if we were to allocate it 100% of resources.
- Need = Maximum possible - currently allocated resources
	- If we complete the processes with the lowest possible need first then we will have more resources that we can ration to the higher need tasks
- P1 has 1 0 1 needs 1 2 3
- P2 has 2 2 1 needs 6 3 4
- P3 has 0 1 0 needs 3 5 3
	- If we only have in total 6A 5B and 4C we can complete P1 first to free up some resources
## Detection and Resolution
- Reactive method of fixing deadlocks
- If there is only one copy of a resource then a cycle in a resource allocation graph is not only necessary but also sufficient to guarantee a deadlock. 
	- This RAG can also be "compressed" into a wait time graph when there is only one copy of a resource. Where Every resource that has both an allocation to a process, and a request by a different process, you can draw the requesting process extending an arrow directly towards the process which is is waiting upon to release its resource
	- This allows faster calculation time in order to find if there are any cycles
- Process Termination
	- If you reach a deadlock you could kill one of the processes that is blocked. How are you to decide which process to terminate?
		- You most likely want to terminate the process that has done the LEAST amount of work.