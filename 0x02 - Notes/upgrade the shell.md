Upgrading a bare netcat reverse shell to a full interactive TTY is a standard step in pentesting/CTF work (the raw shell lacks job control, tab completion, arrow keys, and breaks on Ctrl+C). Here's the common approach:

## 1. Spawn a PTY with Python

On the target (victim) shell:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

If Python isn't available, try `python`, `python2`, or alternatives like `script -qc /bin/bash /dev/null`, or `perl -e 'exec "/bin/bash";'`.

## 2. Background the shell and fix terminal settings

On your **attacker** machine, press `Ctrl+Z` to background the netcat session, then:

```bash
stty raw -echo; fg
```

Press Enter twice after `fg` to return to the shell.

## 3. Set TERM and window size

Back in the (now backgrounded-then-foregrounded) shell:

```bash
export TERM=xterm
```

Then, in another local terminal, check your terminal size:

```bash
stty size
```

And set it in the shell:

```bash
stty rows <rows> cols <cols>
```

## Result

You now get:

- Tab completion
- Arrow keys / command history
- Ctrl+C without killing the shell
- `vim`/`nano`/`less` working properly
- Proper job control

## Alternative one-liners

- **socat** (if available) gives you a full TTY from the start: `socat file:$(tty),raw,echo=0 tcp-listen:4444` on attacker, matching socat exec on target.
- **rlwrap** on the listener side adds history/editing without touching the target shell at all: `rlwrap nc -lvnp 4444`.
