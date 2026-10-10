A processor inside a machine running the Windows operating system can operate under two different modes: [[User Mode]] and [[Kernel Mode]]. Applications run in user mode, and operating system components run in kernel mode. When an application wants to accomplish a task, such as creating a file, it cannot do so on its own. The only entity that can complete the task is the kernel, so instead applications must follow a specific function call flow.

1. **User Processes:** An application executed by the user such as Notepad.
2. **Subsystem DLLs:** [[DLL]]s that contain [[API]] functions called by user processes like [[kernell32.dll]].
3. **[[Native API (Ntdll.dll)|Native API]]:** A system-wide DLL which is the lowest layer available in user mode. This is a special dll that creates the transition from user mode to kernel mode. Also referred to as **Native API** or **NTAPI**.
4. **Executive Kernel:** this is the Windows Kernel, it calls other drivers and modules available within kernel mode to complete tasks. It is partially stored in a file called [[ntoskrnl.exe]] under `"C:\Windows\System32"`

#### Function Call Flow
Flow of an application that creates a file:
1. The user application calls the [CreateFile](https://learn.microsoft.com/it-it/windows/win32/api/FileAPI/nf-fileapi-createfilea)  WinAPI function which is available in the [[kernell32.dll]]. Kernel32.dll is a critical DLL that exposes applications to the [[WinAPI]] and can be loaded by most applications.
2. [CreateFile](https://learn.microsoft.com/it-it/windows/win32/api/FileAPI/nf-fileapi-createfilea) calls its equivalent NTAPI function [NtCreateFile](https://learn.microsoft.com/en-us/windows/win32/api/winternl/nf-winternl-ntcreatefile) provided by [[Native API (Ntdll.dll)|Native API]].
3. [[Native API (Ntdll.dll)|Native API]] then execute an assembly [[syscall]] (or sysenter (x86)) which transfer execution to kernel mode.
4. The kernel [NtCreateFile](https://learn.microsoft.com/en-us/windows/win32/api/winternl/nf-winternl-ntcreatefile) function calls kernel drivers and modules to perform the task.

#### Directly Invoking The Native API (NTAPI)
Application can invoke syscalls directly without going through the Windows API. The Windows API simply act as a wrapper for the Native API. The native API is more difficult to use because it's not officially documented and Microsoft advises against the use of the Native API