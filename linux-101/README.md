---
marp: true
theme: ecar-theme
_class:
    - invert
---

# Linux Command Line 101
## An introduction into the world of Linux

Raphael Guntersweiler
<raphael@guntersweiler.net>

---

# Content of this presentation
- First login
- File system
- Good to know programs
- The Administrator
- Installing new programs (the Linux way)

## ⚠️ Disclaimer
This presentation is geared towards people who haven't used Linux either at all or not a lot. Some best practises might not be followed, some things might be simplified.

We're using the _Debian / Ubuntu_ distribution (mostly due to personal preference). Other flavours exist but most of this presentation still applies.

---

<!-- _class: invert -->

# First login

---

# First login
```
raphael@debian-box:~$ _

|_____| |________| |
   |         |     |
   |         |     +-- Your current location (i.e. "~")
   |         +-------- Your machines name
   +------------------ Your username
```

---

## **First Login**: RTFM

For (almost) every command there exists a man-page (manual).

```
man <command>
```

![bg right contain](./images/man.png)

---

## **First Login**: What is in my directory?
```
raphael@debian-box:~$ ls
another-textfile.txt  file.txt
```

---

## **First Login**: What is in my directory?
```
raphael@debian-box:~$ ls -l
total 5
-rw-r--r-- 1 raphael raphael  0 Dec  7 02:20 another-textfile.txt
-rw-r--r-- 1 raphael raphael 20 Dec  7 02:19 file.txt

||_______|   |_____| |_____|  | |__________| |
|   |           |       |     |       |      |
|   |           |       |     |       |      +- File name
|   |           |       |     |       +-------- Last modified
|   |           |       |     +---------------- File size (bytes)
|   |           |       +---------------------- Owner group
|   |           +------------------------------ Owner
|   +------------------------------------------ File permissions
+---------------------------------------------- File type
```

---

## **First Login**: Navigate through file system
```
cd <destination>
```

```
raphael@debian-box:~$ ls -l
total 5
drwxr-xr-x 2 raphael raphael  3 Dec  7 02:26 Documents
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:22 another-textfile.txt

# Navigate to the Documents directory
raphael@debian-box:~$ cd Documents

raphael@debian-box:~/Documents$ ls -l  
total 5
-rw-r--r-- 1 raphael raphael 20 Dec  7 02:19 file.txt
```

Notice that in the directory listing (`ls -l`) "Documents" had a `d` in the File permissions column prefixed.

---

## **First Login**: Manipulating files
### Copy a file
```
cp <source> <destination>
```

```
raphael@debian-box:~$ ls -l
total 5
drwxr-xr-x 2 raphael raphael  3 Dec  7 02:26 Documents
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:22 another-textfile.txt

raphael@debian-box:~$ cp another-textfile.txt another-textfile.copy.txt

raphael@debian-box:~$ ls -l 
total 10
drwxr-xr-x 2 raphael raphael  3 Dec  7 02:26 Documents
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:30 another-textfile.copy.txt
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:22 another-textfile.txt
```

---

## **First Login**: Manipulating files
### Move / rename a file
```
mv <source> <destination>
```

```
raphael@debian-box:~$ ls -l 
total 10
drwxr-xr-x 2 raphael raphael  3 Dec  7 02:26 Documents
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:30 another-textfile.copy.txt
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:22 another-textfile.txt

raphael@debian-box:~$ mv another-textfile.copy.txt copied-textfile.txt

raphael@debian-box:~$ ls -l
total 10
drwxr-xr-x 2 raphael raphael  3 Dec  7 02:26 Documents
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:22 another-textfile.txt
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:30 copied-textfile.txt
```

---

## **First Login**: Manipulating files
### Move / rename a file II

```
raphael@debian-box:~$ ls
Documents  another-textfile.txt  copied-textfile.txt

raphael@debian-box:~$ ls Documents/
file.txt

raphael@debian-box:~$ mv copied-textfile.txt Documents/

raphael@debian-box:~$ ls 
Documents  another-textfile.txt

raphael@debian-box:~$ ls Documents/
copied-textfile.txt  file.txt
```

---

## **First Login**: Manipulating files
### Delete a file

```
rm <file>
```

⚠️ There is no "Recycle Bin". Once you use `rm`, the file is gone!

```
raphael@debian-box:~/Documents$ ls
copied-textfile.txt  file.txt

raphael@debian-box:~/Documents$ rm copied-textfile.txt 

raphael@debian-box:~/Documents$ ls
file.txt
```

---

## **First Login**: Manipulating files
### Delete a file II

⚠️ How to *destroy* your computer with one command
**Please never run this or everything on or attached to your computer is being deleted.**
```
rm -rf /
 |  || |
 |  || +- The root directory of your system (see FS explanation)
 |  |+--- Force everything, do not ask for confirmation
 |  +---- Recursive
 +------- Remove file / directory
```

---

## **First Login**: Manipulating files
### A few more commands

Create a new directory
```
mkdir <name>
```

Create a new blank file
```
touch <filename>
```

Open file in text editor
```
nano <filename>
```
```
vi <filename>
```

---

<!-- _class: invert -->

# The Linux File System

---

## Linux File System
- File system uses a common root: `/`
- Everything on your computer is somewhere in or under `/`
- On Linux, everything is a "file"; even your keyboard, your mouse, your webcam

- There are _no "drive letters"_; drives are mounted in directories under `/`

---

## **Linux File System**: Some of the most important directories
- `/bin` Essential binaries
- `/dev` Device files
- `/etc` Config files
- `/home` Home directories for all users
- `/lib` Shared libraries
- `/media` + `/mnt` Mountpoints for removable / temporary media / file systems
- `/root` Home directory
- `/sbin` Essential system binaries
- `/tmp` Temporary files
- `/usr` Secondary tree for files (usually also contains `bin`, `sbin`, etc.)
- `/var` Variable data (i.e. log files)

---

## **Linux File System**: `~` ("_tilde_")
The `~` directory is a shortcut to the current users home directory, usually `/home/<username>` (or `/root`).

---

<!-- _class: invert -->

# Permission System

---

## Unix File Permissions

```
raphael@debian-box:~$ ls -l
total 10
drwxr-xr-x 2 raphael raphael  3 Dec  7 02:35 Documents
-rw-r--r-- 1 raphael raphael 12 Dec  7 02:22 another-textfile.txt
-rw-r--r-- 1 root    raphael 19 Dec 10 23:02 test.sh
 |_______|   |_____| |_____|
     |          |       +------ Owner Group
     |          +-------------- Owner User
     +------------------------- File Permissions
```

**`r`**/**`4`** Read | **`w`**/**`2`** Write | **`x`**/**`1`** Execute

---

## Unix File Permissions

```
-rw-r--r-- 1 root    raphael 19 Dec 10 23:02 test.sh
 |_||_||_|
  |  |  +----- Other / Everyone
  |  +-------- Owner Group (i.e. raphael)
  +----------- Owner User (i.e. root)
```

---

## Unix File Permission: Octal Representation
```
-rw-r--r-- 1 root    raphael 19 Dec 10 23:02 test.sh
 |_||_||_|
  |  |  +----- 4
  |  +-------- 4 ==> 644
  +----------- 6
```

---

## Changing File Permissions
```
raphael@debian-box:~$ ls -l file.txt
-rw-r--r-- 1 raphael raphael 0 Dec 13 17:07 file.txt

raphael@debian-box:~$ chmod 600 file.txt
```
`600` => Owner: `rw`; Group: `-`; Everyone: `-`
```
raphael@debian-box:~$ ls -l file.txt
-rw------- 1 raphael raphael 0 Dec 13 17:07 file.txt
```

_In this example, only the owner will have permission to read and write `file.txt`. Everyone else will not._

---

## Changing File Permissions
```
raphael@debian-box:~$ ls -l file.txt
-rw-r--r-- 1 raphael raphael 0 Dec 13 17:07 file.txt

raphael@debian-box:~$ chmod +x file.txt
```
`+x` => add "Execute" for all parties
Alternative: `$ chmod 755 file.txt`
```
raphael@debian-box:~$ ls -l file.txt
-rwxr-xr-x 1 raphael raphael 0 Dec 13 17:07 file.txt

raphael@debian-box:~$ chmod -x file.txt
```
`-x` => remove "Execute" for all parties
```
raphael@debian-box:~$ ls -l file.txt
-rw-r--r-- 1 raphael raphael 0 Dec 13 17:07 file.txt
```

---

## Change File Ownership
```
root@debian-box:/home/raphael# ls -l file.txt
-rw-r--r-- 1 raphael raphael 0 Dec 13 17:07 file.txt

root@debian-box:/home/raphael# chown root file.txt
```
give ownership of `file.txt` to user `root`
```
root@debian-box:/home/raphael# ls -l file.txt
-rw-r--r-- 1 root raphael 0 Dec 13 17:07 file.txt
```

---

## Change File Ownership
```
root@debian-box:/home/raphael# ls -l file.txt
-rw-r--r-- 1 raphael raphael 0 Dec 13 17:07 file.txt

root@debian-box:/home/raphael# chown :operator file.txt
```
give ownership of `file.txt` to group `operator`
```
root@debian-box:/home/raphael# ls -l file.txt
-rw-r--r-- 1 raphael operator 0 Dec 13 17:07 file.txt
```

---

## Change File Ownership
```
root@debian-box:/home/raphael# ls -l file.txt
-rw-r--r-- 1 raphael raphael 0 Dec 13 17:07 file.txt

root@debian-box:/home/raphael# chown root:operator file.txt
```
give ownership of `file.txt` to user `root` and group `operator`
```
root@debian-box:/home/raphael# ls -l file.txt
-rw-r--r-- 1 root operator 0 Dec 13 17:07 file.txt
```

---

## Elevation
Global actions usually require `root` privileges to execute

Users can elevate a command using `sudo`

---

<!-- _class: invert -->

# Installing programs: **Package Managers**

---

## What are _Package Managers_?
- install and maintain _packages_

## What are _Packages_?
- archive that contains:
    - program
    - library
    - documentation
    - source code

---

## Features of _Package Managers_
- install, update and remove packages
- maintain dependencies + versions

**Best Practice**:
- install programs with your distros package manager, if possible

**Why?**
- easier maintenance

---

## Example: _Debian / Ubuntu_

```
raphael@debian-box:~$ sudo apt install tmux

[sudo] password for raphael:
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libevent-2.0-5 libutempter0
The following NEW packages will be installed:
  libevent-2.0-5 libutempter0 tmux
0 upgraded, 3 newly installed, 0 to remove and 37 not upgraded.
Need to get 425 kB of archives.
After this operation, 1,026 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://deb.debian.org/debian stretch/main amd64 libevent-2.0-5 amd64 2.0.21-stable-3 [152 kB]
Get:2 http://deb.debian.org/debian stretch/main amd64 libutempter0 amd64 1.1.6-3 [7,812 B]
Get:3 http://deb.debian.org/debian stretch/main amd64 tmux amd64 2.3-4 [265 kB]
Fetched 425 kB in 0s (599 kB/s)
Selecting previously unselected package libevent-2.0-5:amd64.
(Reading database ... 10148 files and directories currently installed.)
Preparing to unpack .../libevent-2.0-5_2.0.21-stable-3_amd64.deb ...
Unpacking libevent-2.0-5:amd64 (2.0.21-stable-3) ...
Selecting previously unselected package libutempter0:amd64.
Preparing to unpack .../libutempter0_1.1.6-3_amd64.deb ...
Unpacking libutempter0:amd64 (1.1.6-3) ...
Selecting previously unselected package tmux.
Preparing to unpack .../archives/tmux_2.3-4_amd64.deb ...
Unpacking tmux (2.3-4) ...
Setting up libutempter0:amd64 (1.1.6-3) ...
Processing triggers for libc-bin (2.24-11+deb9u3) ...
Processing triggers for man-db (2.7.6.1-2) ...
Setting up libevent-2.0-5:amd64 (2.0.21-stable-3) ...
Setting up tmux (2.3-4) ...
Processing triggers for libc-bin (2.24-11+deb9u3) ...
```

---

<!-- _class: invert -->

# _Short Break_

---

<!-- _class: invert -->

# Now let's do some wizardry

---

## Command Streams
`stdin` (`0`) Standard Input
This strean is used for inputing data from other programs

`stdout` (`1`) Standard Output
This strean contains output from programs

`stderr` (`2`) Standard Error
This strean is intended to be used for outputing errors of programs

---

## `cat`
Print a (text) file to `stdout`

```
raphael@debian-box:~$ cat /var/log/syslog
Dec 13 00:00:20 debian-box rsyslogd:  [origin software="rsyslogd" swVersion="8.1901.0" x-pid="64" x-info="https://www.rsyslog.com"] rsyslogd was HUPed
Dec 13 00:00:24 debian-box systemd[1]: man-db.service: Succeeded.
Dec 13 00:00:24 debian-box systemd[1]: Started Daily man-db regeneration.
Dec 13 00:08:37 debian-box systemd[1]: Starting Daily apt download activities...
Dec 13 00:08:42 debian-box systemd[1]: apt-daily.service: Succeeded.
Dec 13 00:08:42 debian-box systemd[1]: Started Daily apt download activities.
Dec 13 00:48:01 debian-box CRON[2736]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 01:48:02 debian-box CRON[2740]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 02:28:37 debian-box systemd[1]: Starting Cleanup of Temporary Directories...
Dec 13 02:28:38 debian-box systemd[1]: systemd-tmpfiles-clean.service: Succeeded.
Dec 13 02:28:38 debian-box systemd[1]: Started Cleanup of Temporary Directories.
Dec 13 02:48:01 debian-box CRON[2744]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 03:48:01 debian-box CRON[2748]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 04:48:01 debian-box CRON[2751]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 05:31:06 debian-box dhclient[86]: DHCPREQUEST for 192.168.3.27 on eth0 to 192.168.1.1 port 67
Dec 13 05:31:06 debian-box dhclient[86]: DHCPACK of 192.168.3.27 from 192.168.1.1
Dec 13 05:31:06 debian-box dhclient[86]: bound to 192.168.3.27 -- renewal in 33604 seconds.
[...]
```

---

## `head` / `tail`
Print a defined amount of a (text) file to `stdout` from start / end of file

_Example: Print first/last 3 lines of file `/var/log/syslog`_
```
raphael@debian-box:~$ head -n3 /var/log/syslog
Dec 13 00:00:20 debian-box rsyslogd:  [origin software="rsyslogd" swVersion="8.1901.0" x-pid="64" x-info="https://www.rsyslog.com"] rsyslogd was HUPed
Dec 13 00:00:24 debian-box systemd[1]: man-db.service: Succeeded.
Dec 13 00:00:24 debian-box systemd[1]: Started Daily man-db regeneration.

raphael@debian-box:~$ tail -n3 /var/log/syslog
Dec 13 18:29:29 debian-box systemd[1]: session-125213.scope: Succeeded.
Dec 13 18:29:35 debian-box systemd[1]: Started Session 125380 of user raphael.
Dec 13 18:48:01 debian-box CRON[3351]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
```

---

## `grep`
Print lines of a text file containing search text to `stdout`

_Example: Search `raphael` in `/var/log/syslog`_
```
raphael@debian-box:~$ grep raphael /var/log/syslog
Dec 13 16:12:50 debian-box systemd[1]: Started Session 125213 of user raphael.
Dec 13 18:29:35 debian-box systemd[1]: Started Session 125380 of user raphael.
```

---

## `|` (piping)
With `|` you can transfer output from one command to the input to another command.

_Example: Search for lines containing `CRON` in the output of the `tail` command_
```
raphael@debian-box:~$ tail /var/log/syslog | grep CRON
Dec 13 17:48:01 debian-box CRON[3083]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 18:48:01 debian-box CRON[3351]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
```

---

## `>`
With `>` you can store command output into a file. If the file exists, it will be replaced.

```
raphael@debian-box:~$ tail /var/log/syslog | grep CRON > output.txt
raphael@debian-box:~$ cat output.txt
Dec 13 17:48:01 debian-box CRON[3083]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 18:48:01 debian-box CRON[3351]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
```

## `>>`
`>>` is the same as `>` but it appends data to a file if it already exists.

```
raphael@debian-box:~$ tail /var/log/syslog | grep CRON >> output.txt
raphael@debian-box:~$ cat output.txt
Dec 13 17:48:01 debian-box CRON[3083]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 18:48:01 debian-box CRON[3351]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 17:48:01 debian-box CRON[3083]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Dec 13 18:48:01 debian-box CRON[3351]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
```

---

## `<`
With `<` you can pipe a file into `stdin` of a program.

```
raphael@debian-box:~$ tail -n1 < output.txt
Dec 13 18:48:01 debian-box CRON[3351]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
```

---

## Redirect `stdout` and `stderr`

_Example: Write `stderr` into `stdout`_
`$ command 2>&1`

_Example: Write `stderr` of `command` into `error.log`, `stdout` to `output.log`_
`$ command 2> error.log 1> output.log`

_Example: Get rid of `stderr` output_
`$ command 2> /dev/null`

