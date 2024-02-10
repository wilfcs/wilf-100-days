# OS->
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