# Pipes in Java
- Pipe.open()
- pipeName.sink()
- pipeName.source()
- Need to allocate certain number of bytes for the buffer, cannot have dynamic buffer size in java
- buffer.flip() sets the position to zero so that we can read from it next for producer
- buffer.clear() used by consumer thread to set position to zero
- sourceChannel.read(bufferSizeVariable)
- String response = new String(buffer.array(), 0 , bufferSize)
# Multithreading in Java
- Java provides thread class and concurrency utilities
- first extend the thread class, and override the run function to input what you want the thread to do
- can create new instance of the myThread class to create new thread
- can add synchronized keyword to methods in a class to ensure that two methods are not ran at the same time. prevents concurrent access to shared data
# Best Practices for Synchronization in Java
- Use synchronized keywork in a function that uses shared variable
- Avoid unnecessary blocking in threads, using wait() to block a thread until a condition is met
	- use notify() to signal other threads that a conditionchanges
- clean up thread resources, for example use try-finally blocks  to prevent leaks
- close or release resources used by threads to deallocate memory