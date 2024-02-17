# CN->
Critical Section Problem and How to address it:
1. Process synchronization techniques play a key role in maintaining the consistency of shared data

2. Critical Section (C.S)
- The critical section refers to the segment of code where processes/threads access shared resources, such as common variables and files, and perform write operations on them. Since processes/threads execute concurrently, any process can be interrupted mid-execution.
- eg. Imagine that you have a badly made bank account (just imagine). Someone gave 100 rupees to you and at the same time, someone else gave 100 rupees to you as well. There is a possibility that because of the same timing of both the transactions the only increment in your bank account if of 100 rupees. In this case your bank account is the critical section.

3. Major Thread scheduling issue: Race Condition
- A race condition occurs when two or more threads can access shared data and they try to change it at the same time. Because the thread scheduling algorithm can swap between threads at any time, you don't know the order in which the threads will attempt to access the shared data. Therefore, the result of the change in data is dependent on the thread scheduling algorithm, i.e., both threads are "racing" to access/change the data.
- eg. Relate that to the example provided above. In the above example, both transactions are "racing" to access/change the data and that is the race condition.

4. Solution to Race Condition
	a. Atomic operations: Make Critical code section an atomic operation, i.e., Executed in one CPU cycle.
	b. Mutual Exclusion using locks.
	c. Semaphores

5. Can we use a simple flag variable to solve the problem of race condition?
ans. No.

6. Peterson’s solution can be used to avoid race condition but holds good for only 2 process/threads.

Peterson's solution:
This solution is very humble. The key idea behind Peterson's solution involves the use of two variables: turn and an array flag of size 2. The flag array is used to indicate the intention of each process to enter the critical section, and the turn variable determines whose turn it is to enter the critical section.
Here's a brief overview of how Peterson's solution works:
1. Each process sets its flag to indicate its intention to enter the critical section. Flag indicates wheter the process wants to enter the critical section or not.
   
2. Then the process sets turn to the index of the other process (0 or 1). If there are two processes, let's say P0 and P1, and P0 is currently in the critical section, then P1's turn will be set to 0, indicating that it's P0's turn to enter the critical section next.
   
3. The process checks if the other process has set its flag and has the turn to enter the critical section. If so, it waits until the conditions are met.
4. If no other process has its turn set to 1, then the current process enters the critical section, executes its code, and then clears its flag to signal that it has exited the critical section.


7. Mutex/Locks
- Locks can be used to implement mutual exclusion and avoid race condition by allowing only one thread/process to access critical section at a time. When that thread is done with its work then the critical section is unlocked for other threads.
- Disadvantages:
		i. Contention: one thread has acquired the lock, other threads will be busy waiting, what if the thread that had acquired the lock dies, then all other threads will be in infinite waiting.
		ii. Deadlocks
		iii. Debugging
		iv. Starvation of high-priority threads.