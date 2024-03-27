Less goooo.

Why OS?
	1. What if there is no OS?
		a. Bulky and complex app. (Hardware interaction code must be in app’s code base)
		b. Resource exploitation by 1 App.
		c. No memory protection. Any app can hack the entire memory and make your hardware useless. Let's suppose you store your credentials and important information somewhere in the memory, then some other application can easily steal it.
		d. Violation of DRY principle because every app will have a bulky code to interact with the hardware.
	2. What is an OS made up of?
	- Collection of system software.
		- System software operates and controls the computer system and provides a platform to run application software.
		
An operating system's function -
- Access to the computer hardware.
- interface between the user and the computer hardware
- Resource management (Aka, Arbitration) (memory, device, file, security, process etc)
- Hides the underlying complexity of the hardware. (Aka, Abstraction)
- Facilitates execution of application programs by providing isolation and protection.
	- Isolation means that an application software has no idea about the memory of another application software. Their memory is being managed in isolation. This in turn helps in the protection of any mishaps with the running application like another application killing its process.

Operating System Definition:
- An operating system is a piece of software that manages all the resources of a computer system, both hardware and software. It provides an environment in which the user can execute his/her programs conveniently and efficiently by hiding the underlying complexity of the hardware and acting as a resource manager.

Goals of OS:
1. Maximum CPU utilization: The CPU should never sit idle.
2. Less process starvation: A process should not starve because of other processes. Suppose we write while(1); then the OS should not work on that process till eternity.
3. Higher priority job execution: Suppose if a job like antivirus scan comes, then every other job should wait and this job shall be given the first priority.
Types of Operating System:
1. Single process Operating system: 
	- Only 1 process executes at a time from the ready queue, i.e. the oldest process.
	- It violates all three goals of OS.
	- eg - MS DOS.
2. Batch-Processing Operating system:
	- Firstly, user prepares his job using punch cards.
	- Then, he submits the job to the computer operator.
	- Operator collects the jobs from different users and sort the jobs into batches with similar needs.
	- Then, operator submits the batches to the processor one by one.
	- All the jobs of one batch are executed together.
	- It is similar to the single process os but it just works in batches. Batches consist of multiple jobs. It violates all the OS goals as well.
	- 
3. Multiprogramming Operating system:
	- Has a single CPU.
	- In multiprogramming operating system, we have a memory(PCB) that stores the jobs (code and data). Suppose CPU is executing Job A. Now Job A got busy with I/O operations, the CPU will become idle if it sits and waits for Job A to return. But the CPU will save the state of Job A in PCB(Process control block) and take out the state of Job B and start executing it instead of waiting for Job A.
	- The above is an example of context switching.
	- CPU is never idle in the case of multiprogramming OS.
	- Also gives priority to high priority tasks.
	- All three OS goals are achieved but not in the most optimal manner.
4. Multitasking Operating system:
	- Single CPU
	- Context switching
	- Time sharing: A process only runs till a certain time limit.
	- Because of time sharing and context switching we are able to run more than one task simultaneously.
	- High priority tasks.
	- All three OS goals are achieved more optimally than multiprogramming OS.
5. Multiprocessing Operating System:
	- More than one CPU.
	- Reliability: If one CPU fails, the others can work.
	- Less process starvation.
	- Is perfect for all three OS goals.
6. Distributed Operating System:
	- A distributed operating system is a software system that manages a collection of independent computers and makes them appear to the user as a single coherent system. It provides a consistent and transparent interface for users to access shared resources and perform tasks across multiple machines.
	- Loosely connected autonomous, interconnected computer nodes.
		- Loosely connected: The connection might be through anything, LAN, MAN, WAN, etc.
		- Autonomous: Each computer node operates independently of the others.
		- Interconnected computer nodes: Computer devices connected to each other.
7. RTOS (Real Time Operating System):
	- Real-time error-free, computations within tight-time boundaries.
	- Used in air Traffic control systems, ROBOTS, Nuclear power plants, army etc.
Multi-Tasking vs Multi-Threading:
- Program: A Program is an executable file that contains a certain set of instructions written to complete a specific job or operation on your computer. It’s a compiled code. Ready to be executed. It is stored in Disk.
- Process: Program under execution. Resides in the Computer’s primary memory (RAM).
- Thread:
	- Single sequence stream within a process. Basically a light-weight process.
	- An independent path of execution in a process. E.g., Multiple tabs in a text editor(When you are typing in an editor, spell-checking, formatting of text and saving the text are done concurrently by multiple threads.)
	- Used to achieve parallelism by dividing a process’s tasks which are independent path of execution.
	- Another example -> Suppose you wrote a logic to convert 100x100 png to jpg. You are given 200x100 png. You can break the png into two sets of 100x100 and assign them to two independent threads. Those threads will use the same logic written by you but work in parallelism. After the work is done you will get their combined effect and have a jpg of 200x100.
	- Above is an example of multithreading.
	- It makes the process faster.
	- It works efficiently only when the number of CPUs are greater than 1 because each thread is processed by a separate CPU.
	- There is a new concept of hyperthreading that even the interviewers don't know about. Hyperthreading is a technology developed by Intel that allows a single physical CPU core to act like two logical cores. It does this by sharing the core's computational resources between two separate sets of registers and execution units within the core. Hyperthreading allows a second thread to utilize the otherwise idle resources within the core by switching to the second thread's instructions whenever the first thread is stalled.
Multitasking vs Multithreading:
Multitasking
Multithreading
The execution of more than one task is called multitasking.
The execution of more than one thread is called multithreading.
Concept of more than 1 process being context switched.
Concept of more than 1 thread being context switched.
Number of CPU 1
Number of CPUs >= 1
Isolation and memory protection exist. OS must allocate separate memory and resources to each program that the CPU is executing.
No isolation and memory protection, resources are shared among threads of that process. OS allocates memory to a process; multiple threads of that process share the same memory and resources allocated to the process.
Thread Scheduling:
Threads are scheduled for execution based on their priority. Even though threads are executing within the runtime, all threads are assigned processor time slices by the operating system.
Difference between Thread Context Switching and Process Context Switching:
Thread Context switching
Process context switching
OS saves current state of thread & switches to another thread of same process.
OS saves current state of process & switches to another process by restoring its state.
Doesn’t includes switching of memory address space. (But Program counter, registers & stack are included.)
Includes switching of memory address space.
Fast switching.
Slow switching.
CPU’s cache state is preserved.
CPU’s cache state is flushed.
Now let us look at the components of OS

1. Kernel: A kernel is that part of the operating system which interacts directly with the hardware and performs the most crucial tasks.
	a. Heart of OS/Core component
	b. Very first part of OS to load on start-up.
2. User space: Where application software runs, apps don’t have privileged access to the underlying hardware. It interacts with kernel.
	a. GUI
	b. CLI
3. Shell: A shell, also known as a command interpreter, is that part of the operating system that receives commands from the users and gets them executed.

Functions of Kernel:
1. Process management:
	a. Scheduling processes and threads on the CPUs.
	b. Creating & deleting both user and system process.
	c. Suspending and resuming processes
	d. Providing mechanisms for process synchronization or process
	communication.
2. Memory management:
	a. Allocating and deallocating memory space as per need.
	b. Keeping track of which part of memory are currently being used and by
	which process.
3. File management:
	a. Creating and deleting files.
	b. Creating and deleting directories to organize files.
	c. Mapping files into secondary storage.
	d. Backup support onto a stable storage media.
4. I/O management: to manage and control I/O operations and I/O devices
	a. Buffering (data copy between two devices)
	b. caching
	c. spooling(a process in which data is temporarily held to be used and executed by a device, program, or system)


Types of Kernels:
1. Monolithic kernel
	a. All functions are in kernel itself.
	b. Bulky in size.
	c. Memory required to run is high.
	d. Less reliable, one module crashes -> whole kernel is down.
	e. High performance as communication is fast. (Less user mode, kernel mode overheads)
	f. Eg. Linux, Unix, MS-DOS.
2. Micro Kernel
	a. Only major functions are in kernel.
		i. Memory mgmt.
		ii. Process mgmt.
	b. File mgmt. and IO mgmt. are in User-space.
	c. smaller in size.
	d. More Reliable
	e. More stable
	f. Performance is slow.
	g. Overhead switching b/w user mode and kernel mode.
	h. Eg. L4 Linux, Symbian OS, MINIX etc.
3. Hybrid Kernel:
	a. Advantages of both worlds. (File mgmt. in User space and rest in Kernel space. )
	b. Combined approach.
	c. Speed and design of mono.
	d. Modularity and stability of micro.
	e. Eg. MacOS, Windows NT/7/10
	f. IPC also happens but lesser overheads
4. Nano/Exo kernels...

Q. How will communication happen between user mode and kernel mode?
Ans. Inter process communication (IPC).
1. Two processes executing independently, having independent memory space (Memory protection), But some may need to communicate to work.
2. Done by shared memory and message passing.

System calls:
How do apps interact with Kernel? -> using system calls.
Eg. Mkdir
- Mkdir indirectly calls kernel and asked the file mgmt. module to create a new directory.
- Mkdir is just a wrapper of actual system calls.
- Mkdir interacts with kernel using system calls.

Eg. Creating a process.
- User executes a process. (User space)
- Gets system call. (US)
- Exec system call to create a process. (KS)
- Return to US.

Transitions from US to KS done by software interrupts.
Software interrupts: A software interrupt is a signal to the processor initiated by software, indicating an event that needs immediate attention. So stop everything because there is a high-priority job to be executed.

System calls are implemented in C.

System call definition: A system call is a mechanism using which a user program can request a service from the kernel for which it does not have permission to perform. User programs typically do not have permission to perform operations like accessing I/O devices and communicating with other programs.
System Calls are the only way through which a process can go into kernel mode from user mode.

Types of System Calls:
1) Process Control
	a. end, abort
	b. load, execute
	c. create process, terminate process
	d. get process attributes, set process attributes
	e. wait for time
	f. wait event, signal event
	g. allocate and free memory
2) File Management
	a. create file, delete file
	b. open, close
	c. read, write, reposition
	d. get file attributes, set file attributes
3) Device Management
	a. request device, release device
	b. read, write, reposition
	c. get device attributes, set device attributes
	d. logically attach or detach devices
4) Information maintenance
	a. get time or date, set time or date
	b. get system data, set system data
	c. get process, file, or device attributes
	d. set process, file, or device attributes
5) Communication Management
	a. create, delete communication connection
	b. send, receive messages
	c. transfer status information
	d. attach or detach remote devices

What happens when you turn on your computer?
i. PC On

ii. CPU initializes itself and looks for a program called BIOS stored in BIOS Chip.
- BIOS Chip: Basic input-output system chip is a ROM chip found on mother board that allows to access & setup computer system at most basic level.
- In modern PCs, CPU loads UEFI (Unified extensible firmware interface) instead of BIOS.

iii. CPU runs the BIOS which tests and initializes system hardware. Bios loads configuration settings from memory area. If something is not appropriate (like missing RAM) error is thrown and boot process is stopped. This testing and showing of error process is called POST (Power on self-test) process.
- The configuration settings that Bios load is backed by CMOS battery. CMOS battery is always on even when the CPU is not on. This is helpful in storing clock(time), and configuration settings so that even if the CPU is not on, these things can still be called to boot up the CPU. This is also how when you turn on your computer, the computer still shows the correct time.
- (UEFI can do a lot more than just initialize hardware; it’s really a tiny operating system. For example, Intel CPUs have the Intel Management Engine. This provides a variety of features, including powering Intel’s Active Management Technology, which allows for remote management of business PCs.

iv. BIOS will search the MBR to find the bootloader and handoff responsibility for booting your PC to your OS’s bootloader.
- MBR (master boot record): It is a special boot sector at the beginning of a disk. The MBR contains code that loads the rest of the operating system. That code is known as a “bootloader.” The BIOS executes the bootloader, which takes it from there and begins booting the actual operating system—Windows or Linux, for example. In other words, the BIOS or UEFI examines a storage device on your system to look for a small program, either in the MBR or on an EFI system partition (in case of UEFI), and runs it.

v. The bootloader is a small program that has the large task of booting the rest of the operating system (Boots Kernel then boots User Space). Windows uses a bootloader named Windows Boot Manager (Bootmgr.exe), most Linux systems use GRUB, and Macs use something called boot.efi

32-Bit vs 64-Bit OS (interview question):
1. A 32-bit OS has 32-bit registers, and it can access 2^32 unique memory addresses. i.e., 4GB of physical memory.
2. A 64-bit OS has 64-bit registers, and it can access 2^64 unique memory addresses. i.e., 17,179,869,184 GB of physical memory.
3. 32-bit CPU architecture can process only 32 bits of data and information. So basically if my ROM is 4GB< then a 32-bit CPU cannot even process that ROM. 
4. 64-bit CPU architecture can process 64 bits of data & information.

- Register: Registers are a type of computer memory built directly into the processor or CPU (Central Processing Unit) that is used to store and manipulate data during the execution of instructions. A register may hold an instruction, a storage address, or any kind of data (such as a bit sequence or individual characters).

Advantages of 64-bit over the 32-bit operating system:
a. Addressable Memory: 32-bit CPU -> 2^32 memory addresses, 64-bit CPU -> 2^64 memory addresses.
b. Resource usage:Installing more RAM on a system with a 32-bit OS doesn't impact performance. However, upgrade that system with excess RAM to the 64-bit version of Windows, and you'll notice a difference.
c. Performance: All calculations take place in the registers. When you’re performing math in your code, operands are loaded from memory into registers. So, having larger registers allow you to perform larger calculations at the same time. 32-bit processor can execute 4 bytes of data in 1 instruction cycle while 64-bit means that processor can execute 8 bytes of data in 1 instruction cycle. (In 1 sec, there could be thousands to billons of instruction cycles depending upon a processor design)
d. Compatibility: 64-bit CPU can run both 32-bit and 64-bit OS. While 32-bit CPU can only run 32-bit OS.
e. Better Graphics performance: 8-bytes graphics calculations make graphics-intensive apps run faster.

Some Storage Devices:
1. Register: Smallest unit of storage. It is a part of CPU itself. A register may hold an instruction, a storage address, or any data (such as bit sequence or individual characters). Registers are a type of computer memory used to quickly accept, store, and transfer data and instructions that are being used immediately by the CPU.
2. Cache: Additional memory system that temporarily stores frequently used instructions and data for quicker processing by the CPU.
3. Main Memory: RAM.
4. Secondary Memory: Storage media, on which computer can store data & programs.

Comparison:
1. Cost:
	a. Primary storages are costly.
	b. Registers are most expensive due to expensive semiconductors & labour.
	c. Secondary storages are cheaper than primary.
2. Access Speed:
	a. Primary has higher access speed than secondary memory.
	b. Registers has highest access speed, then comes cache, then main memory.
3. Storage size:
	a. Secondary has more space.
4. Volatility:
	a. Primary memory is volatile.
	b. Secondary is non-volatile.
	
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


Intro to Process Scheduling | FCFS | Convoy Effect:
1. Process Scheduling
	a. Basis of Multi-programming OS.
	b. By switching the CPU among processes, the OS can make the computer more productive.
	c. Many processes are kept in memory at a time, when a process must wait or the time quantum expires, the OS takes the CPU away from that process & gives the CPU to another process & this pattern continues.
2. CPU Scheduler
	a. Whenever the CPU becomes ideal, OS must select one process from the ready queue to be executed.
	b. Done by STS.
3. Non-preemptive scheduling
	a. Once the CPU has been allocated to a process, the process keeps the CPU until it releases CPU either by
	terminating or by switching to wait-state.
	b. Starvation: a process with a long burst time may starve less burst time process.
	c. Low CPU utilization.
4. Preemptive scheduling
	a. CPU is taken away from a process after time quantum expires along with terminating or switching to wait-state.
	b. Less Starvation
	c. High CPU utilization.
5. Goals of CPU scheduling
	a. Maximum CPU utilization
	b. Minimum Turnaround time (TAT).
	c. Min. Wait-time
	d. Min. response time.
	e. Max. throughput of system.
6. Throughput: No. of processes completed per unit time.
7. Arrival time (AT): Time when process is arrived at the ready queue.
8. Burst time (BT): The time required by the process for its execution.
9. Turnaround time (TAT): Time taken from first time process enters ready state till it terminates. (CT - AT)
10. Wait time (WT): Time process spends waiting for CPU. (WT = TAT – BT)
11. Response time: Time duration between process getting into ready queue and process getting CPU for the first time.
12. Completion Time (CT): Time taken till process gets terminated.


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