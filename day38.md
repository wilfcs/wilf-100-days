The Dining Philosophers' problem

1. We have 5 philosophers. All of them are dumb.
2. They spend their life just being in two states:
	a. Thinking
	b. Eating
3. They sit on a circular table surrounded by 5 chairs (1 each), in the center of the table is a bowl of noodles, and the table is laid with 5 single forks.
4. Thinking state: When a philosopher thinks, he doesn’t interact with others. In fact the philosophers never interact with each other.
5. Eating state: When a philosopher gets hungry, he tries to pick up the 2 forks adjacent to him (Left and Right). He can pick one fork at a time.
6. One can’t pick up a fork if it is already taken.
7. When a philosopher has both forks at the same time, he eats without releasing forks. But since the philosopher never releases the fork hence the others will not get a chance to eat.
8. Solution can be given using semaphores.
	a. Each fork is a binary semaphore.
	b. A philosopher calls wait() operation to acquire a fork.
	c. Release fork by calling signal().
	d. Semaphore fork[5]{1};
9. Although the semaphore solution makes sure that no two neighbors are eating simultaneously but it could still create Deadlock.
10. Suppose that all 5 ph. Become hungry at the same time and each picks up their left fork, then all fork semaphores would be 0.
11. When each ph. Tries to grab his right fork, he will be waiting forever because each philosopher has acquired a fork and all are waiting for others to release a fork (Deadlock)
12. We must use some methods to avoid Deadlock and make the solution work. Here are the three methods to avoid deadlock:
	a. Allow at most 4 ph. To be sitting simultaneously. In this way even if each philosopher grabs a single fork there will be at least one extra fork. So one philosopher can grab that fork, start eating and release both forks and then other ph. can start eating too.
	b. Allow a ph. To pick up his fork only if both forks are available and to do this, he must pick them up in a critical section (atomically).
	c. Odd-even rule. an odd ph. Picks up first his left fork and then his right fork, whereas an even ph. Picks up his right fork and then his left fork. In that case every philosopher will at least have 2 forks or 0 forks.

13. Hence, only semaphores are not enough to solve this problem. We must add some enhancement rules to make a deadlock-free solution.

DEADLOCK:
1. In a Multi-programming environment, we have several processes competing for a finite number of resources
2. Process requests a resource (R), if R is not available (taken by another process), the process enters a waiting state. Sometimes that waiting process is never able to change its state because the resource it has requested is busy (forever), called DEADLOCK (DL)
3. Two or more processes are waiting on some resource’s availability, which will never be available as it is also busy with some other process. The Processes are said to be in Deadlock.
4. DL is a bug present in the process/thread synchronization method.
5. In DL, processes never finish executing, and the system resources are tied up, preventing other jobs from starting.
6. Example of resources: Memory space, CPU cycles, files, locks, sockets, IO devices etc.
7. Single resource can have multiple instances of that. E.g., CPU is a resource, and a system can have 2 CPUs.
8. How does a process/thread utilize a resource?
	a. Request: Request the R, if R is free Lock it, or else wait till it is available.
	b. Use
	c. Release: Release resource instance and make it available for other processes

9. Deadlock Necessary Condition (Must learn these conditions because it is asked in interviews): 4 Condition should hold simultaneously.
	a. Mutual Exclusion
	- Only 1 process at a time can use the resource, if another process requests that resource, the requesting process must wait until the resource has been released.
	
	b. Hold & Wait
	- A process must be holding at least one resource & waiting to acquire additional resources that are currently being held by other processes.
	
	c. No-preemption
	- Resource must be voluntarily released by the process after completion of execution. No resource preemption, i.e. OS cannot forcefully deallocate a resource, it must be voluntarily released by the process after execution.
	
	d. Circular wait
	- A set {P0, P1, ... ,Pn} of waiting processes must exist such that P0 is waiting for a resource held by P1, P1 is waiting for a resource held by P2, and so on. It is basically an extension of hold and wait.

	The following example is an example of deadlock as all four conditions satisfy ->


10. Methods for handling Deadlocks:
	a. Use a protocol to prevent or avoid deadlocks, ensuring that the system will never enter a deadlocked state. We will see the prevention techniques later.
	b. Allow the system to enter a deadlocked state, detect it, and recover.
	c. Ignore the problem altogether and pretend that deadlocks never occur in system. (Ostrich algorithm) aka, Deadlock ignorance. We call this method the Ostrich algorithm because when an Ostrich detects danger, it hides its face inside the soil and things that the danger is over. Similar to that, in this method OS tells the programer to handle things on their own if deadlock occurs.

11. To ensure that deadlocks never occur, the system can use either a deadlock prevention or deadlock avoidance scheme.
12. Deadlock Prevention: 
If we ensure that at least one of the necessary conditions for deadlock is not there then we can prevent deadlock.
	a. Mutual exclusion
		i. Use locks (mutual exclusion) only for non-sharable resource.
		ii. Sharable resources like Read-Only files can be accessed by multiple processes/threads.
		iii. However, we can’t prevent DLs by denying the mutual-exclusion condition, because some resources are intrinsically non-sharable.
		iv. So, in short, we must only use locks in the critical section that actually needs locks. If it doesn't need locking we must avoid it.

	b. Hold & Wait
		i. To ensure H&W condition never occurs in the system, we must guarantee that, whenever a process requests a resource, it doesn’t hold any other resource.
		ii. Protocol (A) can be, each process has to request and be allocated all its resources before its execution. It should not hold a resource before there is actually the use of that resource.
		iii. Protocol (B) can be, allow a process to request resources only when it has none. It can request any additional resources after it must have released all the resources that it is currently allocated.
		iv. So in simple words before using new resources as indicated in protocol A, we must release used resources as well.

	c. No preemption
		i. If a process is holding some resources and requests another resource that cannot be immediately allocated to it, then all the resources the process is currently holding are preempted. The process will restart only when it can regain its old resources, as well as the new one that it is requesting. (Live Lock may occur).
		eg-> P1 has R1 and it wants R2. R2 is busy. So P1 must release R1 so that it doesn't affect other processes that can take place right now. After both R1 and R2 are free then and only then it can take both the resources.
		ii. If a process requests some resources, we first check whether they are available. If yes, we allocate them. If not, we check whether they are allocated to some other process that is waiting for additional resources. If so, preempt the desired resource from waiting process and allocate them to the requesting process.
		eg-> P1 is waiting for R2. R2 is allocated to P2 but P2 is waiting for R3, so P2 is not able to process because of this. In that case, we can deallocate R2 from P2 because P1 needs it more than P2 right now. And then we allocate it to P1 and P1 can do its work.

	d. Circular wait
		i. To ensure that this condition never holds is to impose a proper ordering of resource allocation.
		ii. P1 and P2 both require R1 and R1, locking on these resources should be like, both try to lock R1 then R2. By this way whichever process first locks R1 will get R2.
		
		Resource allocation graph:
		
		
		Live lock: