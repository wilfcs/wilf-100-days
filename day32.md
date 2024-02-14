# OS->
Some scheduling algorithms:
1. FCFS (First come-first serve):
	a. Whichever process comes first in the ready queue will be given CPU first.
	b. In this, if one process has longer BT. It will have major effect on average WT of diff processes, called Convoy effect.
	c. Convoy Effect is a situation where many processes, who need to use a resource for a short time, are blocked by one process holding that resource for a long time. This causes poor resource management.
	
2. Shortest Job First (SJF) [Non-preemptive]:1
	a. Process with least BT will be dispatched to CPU first. BT is calculated beforehand with estimation.
	b. Must do estimation for BT for each process in ready queue beforehand, Correct estimation of BT is an impossible task (ideally.)
	c. Run the lowest time process for all times then, choose the job having the lowest BT at that instance.
	d. This will suffer from convoy effect as if the very first process that came in Ready state has a large BT. The reason for it having a large BT might be because the estimation can also be wrong or because there is only one process in the ready queue but after it is assigned CPU some smaller process arrives.
	e. Process starvation might happen.
	f. Criteria for SJF algos: AT + BT.
	
3. SJF [Preemptive]
	a. Less starvation.
	b. No convoy effect.
	c. Gives average WT less for a given set of processes as scheduling a short job before a long one decreases the WT of the short job more than it increases the WT of the long process.

4. Priority Scheduling [Non-preemptive]
	a. Priority is assigned to a process when it is created.
	b. SJF is a special case of general priority scheduling with priority inversely proportional to BT.
	c. This will have a huge convoy effect as new jobs with higher priority will keep arriving and older fewer priority jobs will never get the CPU. This is called  Indefinite waiting aka extreme starvation: This is the biggest drawback of non-preemptive as well as preemptive priority scheduling.
5. Priority Scheduling [Preemptive]
	a. Current RUN state job will be preempted if the next job has higher priority.
	b. May cause indefinite waiting (Starvation) for lower priority jobs. (The possibility is they won’t get executed ever). (True for both preemptive and non-preemptive versions)
		i. Solution: Ageing is the solution.  Gradually increase priority of process that wait so long. E.g., increase priority by 1 every 15 minutes.
		
6. Round robin scheduling (RR)
	a. Most popular
	b. Like FCFS but preemptive.
	c. Designed for time sharing systems.
	d. Criteria: AT + time quantum (TQ), Doesn’t depend on BT.
	e. No process is going to wait forever, hence very low starvation. [No convoy effect]
	f. Easy to implement.
	g. If TQ is small, more will be the context switch (more overhead).
	h. Look at the diagram and you would understand the flow


MLQ | MLFQ:
1. Multi-level queue scheduling (MLQ)
	a. Ready queue is divided into multiple queues depending upon priority.
	b. A process is permanently assigned to one of the queues (inflexible) based on some property of process, memory, size, process priority or process type.
	c. Each queue has its own scheduling algorithm. E.g., SP -> RR, IP -> RR & BP -> FCFS.

	d. All processes:
	- System process: Created by OS (Highest priority).
	- Interactive process (Foreground process): Needs user input (I/O).
	- Batch process (Background process): Runs silently, no user input required.
	e. Scheduling among different sub-queues is implemented as fixed priority preemptive scheduling. E.g., foreground queue has absolute priority over background queue.
	f. If an interactive process comes & batch process is currently executing. Then, batch process will be preempted.
	g. Problem: Only after completion of all the processes from the top-level ready queue, the further level ready queues will be scheduled. This came starvation for lower priority process.
	h. Convoy effect is present.
2. Multi-level feedback queue scheduling (MLFQ)
	a. Multiple sub-queues are present.
	b. Allows the process to move between queues. The idea is to separate processes according to the characteristics of their BT. If a process uses too much CPU time, it will be moved to lower priority queue. This scheme leaves I/O bound and interactive processes in the higher-priority queue. In addition, a process that waits too much in a lower-priority queue may be moved to a higher priority queue. This form of ageing prevents starvation.
	c. Less starvation then MLQ.
	d. It is flexible.
	e. Can be configured to match a specific system design requirement.

 Introduction to Concurrency:
1. Concurrency is the execution of the multiple instruction sequences at the same time. It happens in the operating system when there are several process threads running in parallel.

2. Thread:
	• Single sequence stream within a process.
	• An independent path of execution in a process.
	• Light-weight process.
	• Used to achieve parallelism by dividing a process’s tasks which are independent path of execution.
	• E.g., Multiple tabs in a browser, text editor (When you are typing in an editor, spell checking, formatting of text and saving the text are done concurrently by multiple threads.)

3. Thread Scheduling: Threads are scheduled for execution based on their priority. Even though threads are executing within the runtime, all threads are assigned processor time slices by the operating system.

4. Threads context switching
	• OS saves current state of thread & switches to another thread of same process.
	• Doesn’t includes switching of memory address space. (But Program counter, registers & stack are included.)
	• Fast switching as compared to process switching
	• CPU’s cache state is preserved.

5. How each thread get access to the CPU?
	• Each thread has its own program counter.
	• Depending upon the thread scheduling algorithm, OS schedules these threads.
	• OS will fetch instructions corresponding to PC of that thread and execute instructions.

6. I/O or TQ, based context switching is done here as well
	• We have TCB (Thread control block) like PCB for state storage management while performing context switching.

7. Will single CPU system would gain by multi-threading technique?
	• Never.
	• As two threads have to context switch for that single CPU.
	• This won’t give any gain.

8. Benefits of Multi-threading.
	• Responsiveness
	• Resource sharing: Efficient resource sharing.
	• Economy: It is more economical to create and context switch threads. Also, allocating memory and resources for process creation is costly, so better to divide tasks into threads of same process.
	• Threads allow utilization of multiprocessor architectures to a greater scale and efficiency.