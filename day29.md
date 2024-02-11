Introduction to Process:
- Program: Compiled code, that is ready to execute.
- Process: Program under execution.
- OS converts the program into a process.

STEPS of process creation by OS:
	a. Load the program and static data into memory.
	b. Allocate runtime stack to store local variables, function arguments, and return values, etc.
	c. Heap memory allocation for dynamic memory allocation.
	d. IO tasks.
	e. OS handoffs control to main ().
	
The architecture of process:

Stack: contains local variables, function arguments and return values. We must apply base condition in our recursive function calls so as to avoid the overflow of stack.
Heap: contains dynamically allocated variables. We must deallocate unnecessary objects to avoid else you will get an out of memory error.
Data: Contains global and static data.
Text area: Contains compiled code loaded from disk.

Process control block (PCB): 
- All processes are tracked by OS using a table like data structure and each entry in that table is called PCB.
- PCB stores information of a process such as: process id, program counter, process state, priority, etc.
PCB Structure:

- Process ID: Unique identifier
- Program counter: Next instruction address of program
- Process state: Stores state like new, wait, run.
- Priority: Based on this priority a process gets CPU time
- Registers: it is a data structure. When a process is running and its time slice expires, the current value of the process's registers would be stored in the PCB register. When the process is scheduled to be run again, the PCB register values are read from the PCB and written to the CPU registers. This is the main purpose of the registers in the PCB.
- Open file list: List of all open files.
- Open devise list: List of of open devices.

Process States | Process Queues:
- Process States: As process executes, it changes state. Each process may be in one of the following states:
		a. New: OS is about to pick the program & convert it into a process. OR the process is being created.
		b. Run: Instructions are being executed; CPU is allocated.
		c. Waiting: Waiting for IO.
		d. Ready: The process is in memory, waiting to be assigned to a processor.
		e. Terminated: The process has finished execution. PCB entry removed from process table.

- Process Queus:
		a. Job Queue:
			i. Processes in new state.
			ii. Present in secondary memory.
			iii. Job Schedular (Long term schedular (LTS)) picks process from the pool and loads them into memory for execution.
		b. Ready Queue:
			i. Processes in Ready state.
			ii. Present in main memory.
			iii. CPU Schedular (Short-term schedular) picks process from ready queue and dispatch it to CPU.
		c. Waiting Queue:
			i. Processes in Wait state.

- Degree of multi-programming: The number of processes in the memory.
		a. LTS controls the degree of multi-programming. The faster the the LTS the more the degree fo multi-programming.
		
- Diagram to understand LTS and STS work:

- We already know how LTS and STS work, but there is one more scheduler know as MTS(Medium Time Scheduler).
- MTS: The primary function of the Medium Term Scheduler is to manage the degree of multiprogramming. In other words, it controls how many processes are in the ready queue at any given time. Lets understand this:
	- So let's say that you have increased the degree of multiprogramming by a lot by inserting a lot of processes in Ready Queue. There will be some processes which will be of a very huge size and will take up the space of the ready queue and exhaust the space.
	- To deal with this we swap out these huge processes and place them in another area of memory(like a hard disk or any secondary storage, let's call them swap-space). This will save the memory of the ready queue.
	- When some processes from the ready queue go for execution and the space is vacant we then swap in the processes stored in swap-space.
	- This entire process is called swapping.

Swapping | Context-Switching | Orphan process | Zombie process:
1. Swapping
	a. Time-sharing system may have medium term schedular (MTS).
	b. We remove processes from the ready queue to reduce degree of multi-programming.
	c. These removed processes can be reintroduced into the ready  queue, and its execution can be continued where it left off. This is called Swapping.
	d. Swap-out and swap-in is done by MTS.
	e. Swapping is necessary to improve process mix or because a change in memory requirements has overcommitted available memory, requiring memory to be freed up.
	f. Swapping is a mechanism in which a process can be swapped temporarily out of main memory (or move) to secondary storage (disk) and make that memory available to other processes. At some later time, the system swaps back the process from the secondary storage to the main memory.

2. Context-Switching
	a. Switching the CPU to another process requires performing a state save of the current process and a state restore of a different process.
	b. When this occurs, the kernel saves the context of the old process in its PCB and loads the saved context of the new process scheduled to run.
	c. It is pure overhead because the system does no useful work while switching.
	d. Speed varies from machine to machine, depending on the memory speed, the number of registers that must be copied, etc.

3. Orphan process
	a. The process whose parent process has been terminated and it is still running.
	b. Orphan processes are adopted by the init process.
	c. Init is the first process of OS.
	
4. Zombie process / Defunct process
	a. A zombie process is a process whose execution is completed but it still has an entry in the process table.
	b. Zombie processes usually occur for child processes, as the parent process still needs to read its child’s exit status. Once this is done using the wait system call, the zombie process is eliminated from the process table. This is known as reaping the zombie process.
	c. It is because parent process may call wait () on child process for a longer time duration and child process got terminated much earlier.
	d. As entry in the process table can only be removed, after the parent process reads the exit status of child process. Hence, the child process remains a zombie till it is removed from the process table.