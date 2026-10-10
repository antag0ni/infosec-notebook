### Core Framework: Techniques vs. Final Products

In malware development, **techniques** are the technical building blocks (the components), while **final products** are the assembled programs deployed to achieve a specific operational goal (the vehicles).

  

### Part 1: Maldev Techniques (The Components)

Techniques fall into three functional pillars:

  

- **Process Injection & Execution:** Running code inside target processes to evade surface-level monitoring.
    
      
    - _Classic DLL Injection:_ Allocating remote memory and calling `CreateRemoteThread` + `LoadLibraryA`.
        
          
        
    - _Reflective DLL Injection:_ Manually parsing PE headers and mapping a DLL into remote memory entirely from RAM.
        
          
        
    - _Process Hollowing:_ Spawning a legitimate process suspended, unmapping its original image, writing malicious code, and resuming execution.
        
          
        
    - _APC Injection (Early Bird):_ Queuing an Asynchronous Procedure Call to a suspended thread to execute payload code upon activation.
        
          
        
- **Defense Evasion:** Neutralizing AV and EDR sensors.
    
      
    - _API Hashing:_ Resolving function pointers at runtime using precomputed hashes to hide static API import strings.
        
          
        
    - _Userland Unhooking:_ Restoring the original, unhooked bytes of monitored system libraries (e.g., `ntdll.dll`) directly from disk or memory.
        
          
        
    - _Direct / Indirect Syscalls:_ Invoking the `syscall` instruction directly via custom assembly to bypass userland API hooks.
        
          
        
    - _Sleep Obfuscation:_ Encrypting in-memory payload regions and masking thread states during idle periods to defeat periodic memory scans.
        
          
        
- **Persistence:** Maintaining access across reboots.
    
      
    - _Registry & Tasks:_ Using Run keys, Scheduled Tasks, or COM hijacking to guarantee automatic reactivation.
        
          
        
    - _DLL Hijacking:_ Placing weaponized libraries in folders higher on the operating system search order.
        
          
        

### Part 2: Maldev Final Products (The Assembled Tools)

Final products combine the techniques above to serve distinct phases of an operation:

  

|**Category**|**Product**|**Primary Function & Characteristics**|
|---|---|---|
|**Delivery Vehicles**|**Loader**|Decrypts and executes payloads strictly in-memory without touching disk.|
||**Dropper**|Extracts a payload, writes it to disk, and executes it as a standalone process.|
||**Stager**|Minimal network payload designed to download the primary loader/payload over the network.|
|**Interactive Agents**|**Beacon / Implant**|Periodically polls a Command & Control (C2) server for staged operator tasks.|
||**RAT**|Maintains an active, continuous connection (such as an interactive shell) for manual control.|
|**Data Harvesters**|**Infostealer**|Quickly scrapes credentials, browser cookies, and tokens, exfiltrating before self-terminating.|
||**Keylogger**|Captures sequential user keystrokes via OS message hooks or low-level polling.|
|**Deep Subversion**|**Rootkit**|Operates at the kernel layer (Ring 0) to manipulate system calls and blind telemetry providers.|
|**Destructive**|**Ransomware / Wiper**|Uses OS cryptographic or low-level disk APIs to irreversibly deny data availability.|

### Learning Pathway

The standard development progression moves from **local execution loaders** to **remote injection**, integrates **API hashing/evasion**, and culminates in **direct syscalls and sleep obfuscation** paired with active C2 implants.