# CN ->
Conditional Variable and Semaphores for Threads synchronization:
1. Conditional variable
	a. The condition variable is a synchronization primitive that lets the thread wait until a certain condition occurs.
	b. Works with a lock
	c. Thread can enter a wait state only when it has acquired a lock. When a thread enters the wait state, it will release the lock and wait until another thread notifies that the event has occurred. Once the waiting thread enters the running state, it again acquires the lock immediately and starts executing.
	d. Why to use a conditional variable?
	- To avoid busy waiting.
	e. Contention is not here.
	
2. Semaphores
	a. Synchronization method.
	b. Semaphores is an integer that is equal to the number of resources. Used when we have multiple resources.
	c. Multiple threads can go and execute critical section concurrently.
	d. Allows multiple program threads to access the finite instance of resources whereas mutex allows multiple threads to access a single shared resource one at a time.
	e. Binary semaphore: value can be 0 or 1. Aka, mutex locks.
	f. Counting semaphore
		i. Can range over an unrestricted domain.
		ii. Can be used to control access to a given resource consisting of a finite number of instances.
	g. To overcome the need for busy waiting, we can modify the definition of the wait () and signal () semaphore operations. When a process executes the wait () operation and finds that the semaphore value is not positive, it must wait. However, rather than engaging in busy waiting, the process can block itself. The block operation places a process into a waiting queue associated with the semaphore, and the state of the process is switched to the Waiting state. Then control is transferred to the CPU scheduler, which selects another process to execute.
	h. A process that is blocked, waiting on a semaphore S, should be restarted when some other process executes a signal () operation. The process is restarted by a wakeup () operation, which changes the process from the waiting state to the ready state. The process is then placed in the ready queue.
	So in short->
- Types:
	- Binary Semaphore (Mutex): Has only two states (0 or 1), effectively functioning as a lock.
	- Counting Semaphore: Can have a range of values, managing access to multiple instances of a resource.
- Operation:
	- Wait and Signal: A thread performs a wait() operation to enter a critical section, decrementing the semaphore. If the semaphore's value is negative, the thread is blocked. The signal() operation increments the semaphore, potentially unblocking a waiting thread.
	- Blocking vs. Busy Waiting: Instead of busy waiting, blocked threads are placed in a queue, improving efficiency.

Producer Consumer Problem and its Solution:
The Producer-Consumer problem is a classic example of a multi-process synchronization problem, an example of the bounded-buffer problem. It demonstrates how synchronization tools like semaphores can be used to avoid race conditions in concurrent processes or threads.

The Problem:

1. Scenario:
- Producers: These are processes or threads that generate data and put it into a buffer.
- Consumers: These are processes or threads that take (consume) data from the buffer.
2. Buffer:
- Can be of fixed size (bounded buffer).
- Only a certain number of items can be held in the buffer at a time.
3. Constraints:
- Producers should not produce data when the buffer is full. They need to wait until there is space.
- Consumers should not attempt to consume data when the buffer is empty. They need to wait for data to be produced.
4. Race Condition Risk:
- If the buffer is shared between producers and consumers, there's a risk of race conditions. Both producers and consumers need to modify the buffer state (adding or removing data), which is a critical section in the program.

The Solution:

The solution typically involves using synchronization mechanisms like semaphores or mutexes to ensure safe access to the buffer.
1. Semaphores:
- Mutex Semaphore (Binary Semaphore): This ensures mutual exclusion when accessing the buffer. It's used to protect the critical section (code that accesses the buffer).
- Empty Count Semaphore: This tracks the count of empty spots in the buffer. Initialized to the size of the buffer.
- Full Count Semaphore: This tracks the count of full spots in the buffer. Initialized to 0.
2. Process:
- Producer:
	- Waits for the empty count (indicating there is space to produce).
	- Once it can produce, it locks the buffer using the mutex semaphore, produces the item, adds it to the buffer, then releases the mutex.
	- Signals (increments) the full count semaphore.
- Consumer:
	- Waits for the full count (indicating there's something to consume).
	- Once it can consume, it locks the buffer using the mutex semaphore, consumes an item from the buffer, then releases the mutex.
	- Signals (increments) the empty count semaphore.
3. Avoiding Deadlocks and Starvation:
- Proper semaphore usage ensures that deadlocks and starvation are avoided. The system is designed such that neither producers nor consumers are permanently blocked from accessing the buffer.

Reader-Writer Problem and its solution:
The Reader-Writer problem is another classic synchronization problem, often encountered in systems where a data structure or resource is shared among multiple threads. The challenge is to allow concurrent read access to the resource while maintaining exclusive access for writes.

The Problem:

1. Scenario:
- Readers: Processes or threads that only need to read data from the shared resource.
- Writers: Processes or threads that need to write or modify data in the shared resource.
2. Constraints:
- Multiple readers can read from the resource at the same time without any issues.
- Writers require exclusive access. When a writer is writing to the resource, no other reader or writer should be able to access the resource.
- Priority needs to be managed to avoid starvation (where readers or writers are indefinitely waiting).
3. Race Condition Risk:
- Since both readers and writers need to access the shared resource, there's a risk of race conditions, especially when a writer is modifying data while readers are accessing it.

The Solution:

Solutions to the Reader-Writer problem involve using synchronization tools like semaphores, mutexes.
1. Using Semaphores and Mutexes:
- Mutex: A general mutex ensures mutual exclusion when accessing the resource.
- Read Semaphore: Controls access for readers.
- Write Semaphore: Controls access for writers.
2. Implementation:
- Writer's Access:
	- Requests exclusive access by waiting on both read and write semaphores.
	- After writing, releases these semaphores to allow others to access the resource.
- Reader's Access:
	- Multiple readers can enter the critical section simultaneously but need to ensure a writer is not currently writing.
	- This can be managed by a counter for active readers, protected by a mutex.
	- When the first reader enters, it locks the write semaphore, and the last reader to exit releases it.