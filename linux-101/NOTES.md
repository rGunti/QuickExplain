# Linux Command Line Wizardry 101
by Raphael Guntersweiler <raphael@guntersweiler.net>

_For the sake of brevity this document will use the term "Linux" to address operating systems using the GNU/Linux kernel. Read more [here][1]._

## File System
Linux uses a file system with a common root at `/`. All files, folders and even _devices_ are available within this file system.

Linux doesn't know the concept of "drive letters" like Windows. External file systems are mapped as directories within `/`.

Example of a Linux file system:
```
/
├── bin
├── boot
├── dev
│   └── ...
├── etc
│   ├── alternatives
│   ├── apt
│   └── ...
├── home
│   └── raphael
├── lib
├── lib64
├── media
├── mnt
│   ├── c
│   └── r
├── opt
├── proc
├── root [error opening dir]
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
│   ├── bin
│   ├── games
│   ├── include
│   ├── lib
│   ├── local
│   ├── sbin
│   ├── share
│   └── src
└── var
    ├── backups
    ├── cache
    ├── lib
    ├── local
    ├── lock -> /run/lock
    ├── log
    ├── mail
    ├── opt
    ├── run -> /run
    ├── spool
    └── tmp
```

### File Tree Convention
| Path              | Description |
|:------------------|:------------|
|`/bin`             | Essential binaries
|`/dev`             | Device files
|`/etc`             | Config files
|`/home`            | Home directories for all users
|`/lib`             | Shared libraries
|`/media` or `/mnt` | Mountpoints for removable / temporary media / file systems<br>i.e. USB drives, SD cards, network shares, etc.
|`/root`            | Home directory
|`/sbin`            | Essential system binaries
|`/tmp`             | Temporary files
|`/usr`             | Secondary tree for files (usually also contains `bin`, `sbin`, etc.)
|`/var`             | Variable data (i.e. log files)

### `~` directory
`~` is a shorthand for the current users home directory. For most users, this is `/home/<username>` although this can usually also be reconfigured.

### Device Files
Pretty much every device will have a device file under `/dev` where it can be interacted with. Some examples:

**Hard disks**, **USB drives** or similar devices appear as `/dev/hdX`, `/dev/sdX`, etc. (where `X` is a lower-case letter from a to z)
Partitions of said hard disk appear with the name of their hard disk _plus_ a number, i.e. `/dev/sda3` is the third partition on the drive `/dev/sda`.

If the data **partition** on your hard disk (let's assume `/dev/sda2`) has a **volume label** (e.g. `MY_DATA`), you will also have a _symlinked_ device file with the volume label under `/dev/disk/by-label/MY_DATA`, which will point to the parition's device file (`/dev/sda2`).

You might also find the **USB ports** of your system under something like `/dev/bus/usb/001/002`.

Speaking of USB, your attached **input devices**, like mice, keyboards, trackpads, etc. also appear in this tree. Here is a sample from a Linux-running MacBook:

```
/dev
├── ...
├── input
│   ├── by-id
│   │   ├── usb-Apple_Inc._Apple_Internal_Keyboard___Trackpad-event-kbd -> ../event7
│   │   ├── usb-Apple_Inc._Apple_Internal_Keyboard___Trackpad-if02-event-mouse -> ../event5
│   │   └── usb-Apple_Inc._Apple_Internal_Keyboard___Trackpad-if02-mouse -> ../mouse1
│   ├── by-path
│   │   ├── pci-0000:00:14.0-usb-0:5:1.0-event-kbd -> ../event7
│   │   ├── pci-0000:00:14.0-usb-0:5:1.2-event-mouse -> ../event5
│   │   └── pci-0000:00:14.0-usb-0:5:1.2-mouse -> ../mouse1
│   ├── event0
│   ├── event1
│   ├── ...
│   ├── mice
│   └── mouse1
```

As you can see in this short sample, we can find the keyboard and trackpad here. The links under `by-id` show the device's descriptor and `by-path` shows us where it is attached to hardware-wise.

#### Why device files?
On Windows, you have to have to use special utilities to access certain devices. You cannot access a webcam the same way as a keyboard or a printer. Linux on the other hand maps out everything as a file and allows you to read from or write to it, just like a regular file on a local drive. This means you can use the same routines to read a text file, keyboard input or a CPU's temperature sensor. The only thing that changes is how the content has to be interpreted.

As an example, here are some virtual devices that most Linux distros provide:

#### `/dev/null` - The Black Hole
`/dev/null` is a device that contains nothing. Reading from it will yield no result but every user can write to it. Everything that is written to this device is being discarded instantly.

_As an example, say a program forces you to specify a file to write logs to but you don't want any logs to be created. In this case, you can just specify `/dev/null` as your log file and everything will be written into the void and never stored anywhere._

#### `/dev/zero` - A constant source of NULL bytes
`/dev/zero` is like `/dev/null` in that everything written to it will be discarded. However if you read from `/dev/zero`, you will receive a constant stream of NULL bytes (`0x0`).

_This can be useful for creating dummy files or overwritting hard disks._

#### `/dev/random` & `/dev/urandom` - Random Number Generator
~~`/dev/random`~~ and `/dev/urandom` are basically random number generators provided as device files. They can be read from in a similar way as `/dev/zero` but it will generate random bytes instead of NULL bytes. When writing to `/dev/urandom`, one can influence the output of it.

## Package Managers
Most Linux distributions come with a so called "[package manager][2]". A package is a type of archive that contains a program, library, source code or documentation. A package manager is a program that keeps track of installed packages, checks for updates and dependencies. _You might think of it as an "App Store" for your computer._

It is regarded best practice to install programs using the package manager as much as possible. This has the advanges of reducing duplicates in program files due to required libraries, makes updates easier to install and usually reduces maintenance cost.

However, if required, a user can also install programs either by downloading a package and installing it manually, installing the program itself manually (i.e. copying files to the right locations and doing the configuration) or build the program from source code. Most open source projects have instructions available on how to build and install the program, which most of the time boil down to running a simple build script followed by some kind of install script.

### Example: Debian, Ubuntu and derivates
Debian derivates use `apt` as their package manager, packages come in the form of `.deb` files.

In this example, the user **installed a program** called `tmux` on their system by running `apt install tmux`:

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

raphael@debian-box:~$
```

As one can see from the log above, `tmux` required 2 additional packages to be installed for it to work correctly: `libevent` and `libutempter0`. `apt` made sure that the system has access to these libraries before installing `tmux`.

To **update programs**, one has to update their local repository by running `apt update`. If there are updates available, `apt` will provide a message for that.

```
raphael@L000W1552:~$ sudo apt update
Ign:1 http://deb.debian.org/debian stretch InRelease
Get:2 http://security.debian.org/debian-security stretch/updates InRelease [53.0 kB]
Get:3 http://deb.debian.org/debian stretch-updates InRelease [93.6 kB]
Hit:4 http://deb.debian.org/debian stretch Release
Get:5 http://security.debian.org/debian-security stretch/updates/main amd64 Packages [635 kB]
Get:6 http://security.debian.org/debian-security stretch/updates/main Translation-en [287 kB]
Fetched 1,069 kB in 2s (372 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
37 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

If there are updates available, one can run `apt upgrade` to install all updates or `apt upgrade <package>` to just update a package.

```
raphael@debian-box:~$ sudo apt upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  apt apt-utils base-files debian-archive-keyring e2fslibs e2fsprogs gnupg gnupg-agent gpgv libapt-inst2.0 libapt-pkg5.0 libc-bin libc-l10n libc6
  libcomerr2 libdns-export162 libgnutls30 libidn11 libisc-export160 libseccomp2 libsqlite3-0 libss2 libssl1.0.2 libsystemd0 libudev1 locales
  multiarch-support perl-base sudo systemd systemd-sysv tzdata udev vim-common vim-tiny wget xxd
37 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 26.5 MB of archives.
After this operation, 93.2 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://security.debian.org/debian-security stretch/updates/main amd64 e2fslibs amd64 1.43.4-2+deb9u2 [208 kB]
Get:2 http://deb.debian.org/debian stretch/main amd64 base-files amd64 9.9+deb9u13 [67.6 kB]

[ ... ]
```

**Removing a program** is done by running `apt remove <package>` or `apt purge <package>`.

## Root, `sudo` and Permissions
In a bare-bones Linux-based OS, only the `root` user is allowed to modify the system. `root` (_UID 1_) is a default user provided by all Linux OS's with the permission to do anything on the system. The permissions of this user can never be restricted, it will always have full control over the system.

In order for users to do maintenance work, they would need to be provided with `root`'s password, but sharing passwords is a high security risk. 

---

[1]: https://en.wikipedia.org/wiki/Linux
[2]: https://en.wikipedia.org/wiki/Package_manager
