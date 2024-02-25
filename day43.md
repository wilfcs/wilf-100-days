What is Virtual Memory? || Demand Paging || Page Faults 
1. Virtual memory is a technique that allows the execution of processes that are not completely in the
memory. It provides user an illusion of having a very big main memory. This is done by treating a part of
secondary memory as the main memory. (Swap-space)
2. Advantage of this is, programs can be larger than physical memory.
3. It is required that instructions must be in physical memory to be executed. But it limits the size of a
program to the size of physical memory. In fact, in many cases, the entire program is not needed at the
same time. So, we want an ability to execute a program that is only partially in memory would give
many benefits:
a. A program would no longer be constrained by the amount of physical memory that is
available.
b. Because each user program could take less physical memory, more programs could be run at
the same time, with a corresponding increase in CPU utilization and throughput.
c. Running a program that is not entirely in memory would benefit both the system and the
user.
4. Programmer is provided very large virtual memory when only a smaller physical memory is available.
5. Demand Paging is a popular method of virtual memory management.
6. In demand paging, the pages of a process which are least used, get stored in the secondary memory.
7. A page is copied to the main memory when its demand is made, or page fault occurs. There are various
page replacement algorithms which are used to determine the pages which will be replaced.
8. Rather than swapping the entire process into memory, we use Lazy Swapper. A lazy swapper never
swaps a page into memory unless that page will be needed.
9. We are viewing a process as a sequence of pages, rather than one large contiguous address space, using
the term Swapper is technically incorrect. A swapper manipulates entire processes, whereas a Pager is
concerned with individual pages of a process.
10. How Demand Paging works?
a. When a process is to be swapped-in, the pager guesses which pages will be used.
b. Instead of swapping in a whole process, the pager brings only those pages into memory. This,
it avoids reading into memory pages that will not be used anyway.
c. Above way, OS decreases the swap time and the amount of physical memory needed.
d. The valid-invalid bit scheme in the page table is used to distinguish between pages that are
in memory and that are on the disk.
i. Valid-invalid bit 1 means, the associated page is both legal and in memory.
ii. Valid-invalid bit 0 means, the page either is not valid (not in the LAS of the process)
or is valid but is currently on the disk.

e.
f. If a process never attempts to access some invalid bit page, the process will be executed 
successfully without even the need pages present in the swap space.
g. What happens if the process tries to access a page that was not brought into memory, access 
to a page marked invalid causes page fault. Paging hardware noticing invalid bit for a 
demanded page will cause a trap to the OS.
h. The procedure to handle the page fault:
i. Check an internal table (in PCB of the process) to determine whether the reference 
was valid or an invalid memory access.
ii. If ref. was invalid process throws exception.
If ref. is valid, pager will swap-in the page.
iii. We find a free frame (from free-frame list)
iv. Schedule a disk operation to read the desired page into the newly allocated frame.
v. When disk read is complete, we modify the page table that, the page is now in 
memory.
vi. Restart the instruction that was interrupted by the trap. The process can now access 
the page as through it had always been in memory.

i.
j. Pure Demand Paging
i. In extreme case, we can start executing a process with no pages in memory. When 
OS sets the instruction pointer to the first instruction of the process, which is not in 
the memory. The process immediately faults for the page and page is brought in the 
memory.
ii. Never bring a page into memory until it is required.
k. We use locality of reference to bring out reasonable performance from demand paging.
11. Advantages of Virtual memory
a. The degree of multi-programming will be increased.
b. User can run large apps with less real physical memory.
12. Disadvantages of Virtual Memory
a. The system can become slower as swapping takes time.
b. Thrashing may occur.
