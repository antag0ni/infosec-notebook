`$ find / -perm -4000 -type f 2>/dev/null`

**What SUID means:**  
Normally, when you run a program, it executes with _your_ permissions. If a binary has the SUID bit set, it instead runs with the permissions of the **file's owner**, regardless of who launches it.

**Why this matters for privilege escalation:**  
If you find a binary that:

- has the SUID bit set, **and**
- is owned by `root`, **and**
- lets you execute arbitrary commands or read/write arbitrary files in some way

...then when you run it, that arbitrary command executes with root privileges. It's a shortcut to get a root shell without needing another exploit.