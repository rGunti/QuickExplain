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

^^^^^^^ ^^^^^^^^^^ ^
    |        |     |
    |        |     +-- Your current location (i.e. "~")
    |        +-------- Your machines name
    +----------------- Your username
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

raphael@debian-box:~$ ls -l
total 5
-rw-r--r-- 1 raphael raphael  0 Dec  7 02:20 another-textfile.txt
-rw-r--r-- 1 raphael raphael 20 Dec  7 02:19 file.txt

^^^^^^^^^^   ^^^^^^^ ^^^^^^^ ^^ ^^^^^^^^^^^^ ^
    |           |       |     |       |      |
    |           |       |     |       |      +- File name
    |           |       |     |       +-------- Last modified
    |           |       |     +---------------- File size (bytes)
    |           |       +---------------------- Owner group
    |           +------------------------------ Owner
    +------------------------------------------ File permissions
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
^^  ^^ ^
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
