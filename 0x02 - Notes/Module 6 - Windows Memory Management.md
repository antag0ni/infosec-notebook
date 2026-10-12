#### Virtual Memory & Paging
In modern operating systems memory is not mapped directly to physical memory. 
Instead, [[Virtual Memory]] addresses are used by processes. Virtual memory can be mapped to physical memory (RAM) but can also be stored on disk. With virtual memory addressing processes cane share the same physical address while having a unique virtual memory address. Virtual memory relies on a concept called [[Memory Paging]] which divides memory into chunks of 4kb called "pages".

#### Page State
The pages residing within a process's virtual address space can be in one of these 3 states:
1. **Free** - The page in neither committed nor reserved. The process cannot access the page. The page is available to be reserved, committed or simultaneously reserved and committed. Attempting to read from or write to a free page can result in an access violation.
2. **Reserved** - The page has been reserved for future use. It cannot be used by other allocation functions. The page is not accessible and has no physical storage associated with it. It is available to be committed.
3. **Committed** - The physical storage has been allocated to a region of process's virtual address space. The page is accessible and access is controlled by one of the memory protection constants. The system loads each committed page into physical memory only during the first attempt to read or write that page. When the process terminates, the system releases the storage for committed pages.

#### Page Protection Options
