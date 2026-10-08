A processor inside a machine running the Windows operating system can operate under two different modes: [[User Mode]] and [[Kernel Mode]]. Applications run in user mode, and operating system components run in kernel mode. When an application wants to accomplish a task, such as creating a file, it cannot do so on its own. The only entity that can complete the task is the kernel, so instead applications must follow a specific function call flow.

1. **User Processes:** An application executed by the user such as Notepad.
2. **Subsystem DLLs:** [[DLL]]s that contain [[API]] functions called by user processes like [[kernell32.dll]].
3. **[[Ntdll.dll]]:** A system-wide DLL which is the lowest layer available in user mode. This is a special dll that creates the transition from user mode to kernel mode. Also referred to as **Native API** or **NTAPI**.
4. **Executive Kernel:** this is the Windows Kernel, it calls other drivers and modules available within kernel mode to complete tasks. It is partially stored in a file called [[ntoskrnl.exe]] under "C:\\Windows\\System32"

#### Function Call Flow
