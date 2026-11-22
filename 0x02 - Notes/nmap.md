#Tools 

```
nmap -sV -T4 -v -p- $IP
```

**`nmap`**: the network scanning tool, used to discover hosts, open ports, and services.

**`-sV`** (version detection): once nmap finds an open port, it sends probes to the service to work out _which_ software and version is running (e.g. `OpenSSH 8.9p1`, `Apache httpd 2.4.52`). This is useful because knowing the version lets you look up known vulnerabilities.

**`-T4`** (timing template): sets scan speed on a scale from 0 to 5 (`-T0` paranoid, `-T3` default, `-T5` insane). `-T4` is "aggressive": it shortens timeouts and speeds things up considerably, which suits fast, reliable networks like labs or CTFs. The downside is that it's noisier (easier for IDS/firewalls to detect) and on slow networks it can cause missed results.

**`-v`** (verbose): prints more information while the scan runs, for example showing open ports as they're discovered instead of waiting until the end. `-vv` gives even more detail.

**`-p-`**: scans **all 65535 TCP ports**. It's equivalent to `-p 1-65535`. Without it, nmap only checks the 1000 most common ports, so services on non-standard ports (e.g. a web app on 8443 or 31337) would go unnoticed.

**`$IP`**: a shell variable holding the target's IP address.

**Practical notes:**

- Without root privileges, nmap uses a TCP connect scan (`-sT`); with `sudo` it uses a SYN scan (`-sS`), which is faster and stealthier.
- A full-port scan with `-sV` can take several minutes. A common approach is to first run a fast port scan (`-p-` without `-sV`), then run `-sV -sC` only on the ports found, e.g. `-p 22,80,443`.
- Only use nmap on systems you own or have explicit authorization to test, such as lab or CTF machines: scanning other people's networks without permission may be illegal.
---
## Host discovery

```bash
nmap -sn 192.168.1.0/24          # ping sweep: find live hosts, no port scan
nmap -Pn $IP                     # skip host discovery (treat host as up, useful if ICMP is blocked)
nmap -sL 192.168.1.0/24          # just list targets, send no packets
```

## Port scanning

```bash
nmap $IP                         # top 1000 TCP ports
nmap -p- $IP                     # all 65535 TCP ports
nmap -p 22,80,443 $IP            # specific ports
nmap -p 1-1000 $IP               # port range
nmap --top-ports 100 $IP         # the 100 most common ports
nmap -F $IP                      # fast scan (top 100 ports)
sudo nmap -sU $IP                # UDP scan (slow)
sudo nmap -sU -sT -p U:53,T:80 $IP   # mix UDP and TCP
```

## Scan techniques

```bash
sudo nmap -sS $IP                # SYN scan (default as root, fast, "half-open")
nmap -sT $IP                     # TCP connect scan (default without root)
sudo nmap -sA $IP                # ACK scan (maps firewall rules)
sudo nmap -sN $IP                # NULL scan (no flags set)
sudo nmap -sF $IP                # FIN scan
sudo nmap -sX $IP                # Xmas scan (FIN, PSH, URG flags)
```

## Service and OS detection

```bash
nmap -sV $IP                     # service/version detection
sudo nmap -O $IP                 # OS detection
nmap -sC $IP                     # run default NSE scripts
nmap -sC -sV $IP                 # default scripts + versions (very common combo)
sudo nmap -A $IP                 # aggressive: -sV -sC -O --traceroute
```

## NSE scripts

```bash
nmap --script vuln $IP                       # check for known vulnerabilities
nmap --script http-enum $IP                  # enumerate web directories
nmap --script smb-enum-shares -p 445 $IP     # list SMB shares
nmap --script ftp-anon -p 21 $IP             # check anonymous FTP login
nmap --script "default and safe" $IP         # combine categories
```

Scripts live in `/usr/share/nmap/scripts/`, and `nmap --script-help <name>` explains any of them.

## Timing and evasion

```bash
nmap -T2 $IP                     # slower and quieter
nmap -T4 $IP                     # faster, common for labs/CTFs
sudo nmap -f $IP                 # fragment packets
sudo nmap -D RND:5 $IP           # decoy scan with 5 random decoys
sudo nmap --source-port 53 $IP   # spoof source port
```

## Output

```bash
nmap -oN scan.txt $IP            # normal text output
nmap -oX scan.xml $IP            # XML output
nmap -oG scan.gnmap $IP          # greppable output
nmap -oA scan $IP                # all three formats at once
```

## Useful combos

```bash
# Fast full-port scan, then detailed scan on only the open ports
nmap -p- --min-rate 5000 -T4 $IP -oN ports.txt
nmap -sC -sV -p 22,80,443 $IP -oN detailed.txt

# Scan from a list of targets
nmap -iL targets.txt

# Exclude a host from a range
nmap 192.168.1.0/24 --exclude 192.168.1.1
```

## Reading the results

A port will be reported as one of these states:

- **open**: a service is listening
- **closed**: reachable, but nothing is listening
- **filtered**: a firewall is blocking probes, so nmap can't tell