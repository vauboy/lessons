# ACCESSING LINUX FILE SYSTEMS

## Partitions and FS

One of the goals of having different partitions is to achieve higher data security in case of disaster. By dividing the hard disk in partitions, data can be grouped and separated. Every partition has its own file system.

What is FS:
- Data storage
- Namespace
- Security model
- API
- Implementation


In a file system, a file is represented Development Toolsby an inode, a kind of serial number containing information about the actual data that makes up the file: to whom this file belongs, and where is it located on the hard disk. Every FS has its own set of inodes; throughout a system with multiple partiotions, files with the same inode number can exist.

All partitions are attached to the system via a mount point. The mount point defines the place of a particular data set in the file system. Usually, all partitions are connected through the root partition. On this partition, which is indicated with the slash (/), directories are created.

On a running system, information about the partitions and their mount points can be displayed using the df command (which stands for disk full or disk free)

## Inodes

The inode is a data structure in a Unix-style file system that describes a file-system object such as a file or a directory. Each inode stores the attributes and disk block location(s) of the object's data. File-system object attributes may include metadata (times of last change, access, modification), as well as owner and permission data. Directories are lists of names assigned to inodes. A directory contains an entry for itself, its parent, and each of its children.

Each inode describes a data structure on the hard disk, storing the properties of a file, including the physical location of the file data. When a hard disk is initialized to accept data storage, usually during the initial system installation process or when adding extra disks to an existing system, a fixed number of inodes per partition is created. This number will be the maximum amount of files, of all types (including directories, special files, links etc.) that can exist at the same time on the partition.

At the time a new file is created, it gets a free inode. In that inode is the following information:

- Owner and group owner of the file.
- File type.
- Permissions on the file.
- Date and time of creation, last read and change.
- Date and time this information has been changed in the inode.
- Number of links to this file.
- File size.
- An address defining the actual location of the file data.

The only information not included in an inode, is the file name and directory. These are stored in the special directory files. By comparing file names and inode numbers, the system can make up a tree-structure that the user understands.

## VFS

The Virtual File System (also known as the Virtual Filesystem Switch) is the software layer in the kernel that provides the filesystem interface to userspace programs. It also provides an abstraction within the kernel which allows different filesystem implementations to coexist

![Kernel scheme](https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/Simplified_Structure_of_the_Linux_Kernel.svg/1280px-Simplified_Structure_of_the_Linux_Kernel.svg.png)

## Everything is a file

On a UNIX system, everything is a file; if something is not a file, it is a process.
Think of them in an ordered tree-like structure on the hard disk.

Implements caches (directory, inode, file, etc.)

## Sorts of files

- Regular files
- Directories - files that are lists of other files.
- Special files
  - Block device files
  - Character device files
  - Links - a system to make a file or directory visible in multiple parts of the system's file tree
  - Sockets - a special file type, similar to TCP/IP sockets, providing inter-process networking protected by the file system's access control.
  - Named pipes - act more or less like sockets and form a way for processes to communicate with each other, without using network socket semantics.

```bash
[vault@centos ~]$ ls -ali
total 132
  485273 drwx------. 6 vault vault   233 Dec 14 13:45 .
 8503582 drwxr-xr-x. 3 root  root     19 Nov 30 16:19 ..
  564463 -rw-------. 1 vault vault 93128 Dec 14 13:22 .bash_history
  485274 -rw-r--r--. 1 vault vault    18 Mar 31  2020 .bash_logout
  485275 -rw-r--r--. 1 vault vault   193 Mar 31  2020 .bash_profile
  485276 -rw-r--r--. 1 vault vault   231 Mar 31  2020 .bashrc
10798012 drwxrwxr-x. 2 vault vault    21 Dec 14 05:54 .beeline
 8409177 drwx------. 2 vault vault    29 Nov 30 16:26 .ssh
 5459324 drwxr-xr-x. 2 vault vault    24 Dec 14 13:45 .vim
  594022 -rw-------. 1 vault vault  9269 Dec 14 13:45 .viminfo
  594018 -rw-rw-r--. 1 vault vault   212 Dec 14 13:44 .vimrc
  564511 -rwxrwxr-x. 2 vault vault   176 Dec 10 11:28 hard_link_test.sh
 5135610 drwxrwxr-x. 2 vault vault  4096 Dec 13 17:08 lesson3
  594019 lrwxrwxrwx. 1 vault vault     7 Dec 14 13:43 soft_link_test.sh -> test.sh
  564511 -rwxrwxr-x. 2 vault vault   176 Dec 10 11:28 test.sh
```

## Absolute and relative paths

The path starts with a slash and is called an absolute path.
Paths that don't start with a slash are always relative.

In relative paths the . and .. indicate the current and the parent directory. They are used in combination with their inode number to determine the directory's position in the file system's tree structure

## Information about files

- ls

  List directory contents.

  The -l option to ls displays the file type, using the first character of each input line.
  The file type is one of the following characters:

  - '-' regular file
  - 'b' block special file
  - 'c' character special file
  - 'd' directory
  - 'l' symbolic link
  - 'p' FIFO (named pipe)
  - 's' socket

  Get directory tree structure:

  - ls -R /

- stat

  Gives detailed and verbose statistics on a given file (even a directory or device file) or set of files.
  Output may be formmatted with -c parameter. Format:

  - %a access rights in octal
  - %A access rights in human readable form
  - %F file type
  - %i inode number
  - %m mount point

- file

  Determine file type

  ```bash
  [vault@centos ~]$ file ~
  /home/vault: directory
  [vault@centos ~]$ file /dev/tty0
  /dev/tty0: character special
  [vault@centos ~]$ file access.log
  access.log: ASCII text, with very long lines
  ```

## FS examing tools

- du
  Show (disk) file usage, recursively

- ncdu

- df

Reports on used disk space on the partition containing file

On a running system, information about the partitions and their mount points can be displayed using the df command (which stands for disk full or disk free). In Linux, df is the GNU version, and supports the -h or human readable option which greatly improves readability.

The df command only displays information about active non-swap partitions. These can include partitions from other networked systems

The df command with a dot (.) as an option shows the partition the current directory belongs to:

```bash
[vault@centos ~]$ df -h .
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  6.2G  1.7G  4.6G  28% /
```

## Hard and soft links

A link is nothing more than a way of matching two or more file names to the same set of file data.
There are two ways to achieve this:

- hard link
- soft link

Hard link: Associate two or more file names with the same inode.
Hard links share the same data blocks on the hard disk, while they continue to behave as independent files.

Soft link or symbolic link (or for short: symlink): a small file that is a pointer to another file.
A symbolic link contains the path to the target file instead of a physical location on the hard disk.

ln <target> <link>

Make links between files. Create hard links by default.

```bash
[vault@centos ~]$ ln test.sh hard_link_test.sh
[vault@centos ~]$ ln -s test.sh soft_link_test.sh
[vault@centos ~]$ ls -li
total 12
 564511 -rwxrwxr-x. 2 vault vault  176 Dec 10 11:28 hard_link_test.sh
5135610 drwxrwxr-x. 2 vault vault 4096 Dec 13 17:08 lesson3
 594019 lrwxrwxrwx. 1 vault vault    7 Dec 14 13:51 soft_link_test.sh -> test.sh
 564511 -rwxrwxr-x. 2 vault vault  176 Dec 10 11:28 test.sh
```

unlink - call the unlink function to remove the specified file

## Files searching

- find

This command not only allows you to search file names, it can also accept filels size, date of last change and other file properties as criteria for a search.
Possible options:
- inum N      Search for files with inode number ‘N’.
- name demo   Search for files that are specified by ‘demo’.
- newer file  Search for files that were modified/created after ‘file’.
- perm octal  Search for the file if permission is ‘octal’.
- print       Display the path name of the files found by using the rest of the criteria.
- empty       Search for empty files and directories.
- size +N/-N  Search for files of ‘N’ blocks;
- user name   Search for files owned by user name or ID ‘name’.

File types using -type option:
- b      block (buffered) special
- c      character (unbuffered) special
- d      directory
- p      named pipe (FIFO)
- f      regular file
- l      symbolic  link; this is never true if the -L option or the -follow option is in effect, unless the symbolic link is broken.  If you want to search for symbolic links when -L is in effect, use -xtype.
- s      socket

```bash
[vault@centos ~]$ find / -name ls 2> /dev/null
/usr/bin/ls

[vault@centos ~]$ find /usr/bin/ -size +1000k
/usr/bin/dwp
/usr/bin/ld.gold
/usr/bin/systemd-analyze
/usr/bin/grub2-mkrescue
/usr/bin/vim
/usr/bin/grub2-fstest

```

- locate
  This program is easier to use, but more restricted than find, since its output is based on a file index database that is updated only once every day. On the other hand, a search in the locate database uses less resources than find and therefore shows the results nearly instantly.

  yum install mlocate

- which

Shows the full path of (shell) commands. It does this by searching for an executable or script in the directories listed in the environment variable PATH using the same algorithm as bash.

Options:
--all, -a Print all matching executables in PATH, not just the first

```bash
[vault@centos ~]$ which -a ls
alias ls='ls --color=auto'
	/usr/bin/ls
```

- whereis
  Locate the binary, source, and manual page files for a command
  - -b Search only for binaries.
  - -m Search only for manuals.
  - -s Search only for sources.

## Device files (block and character special files)

Devices, generally every peripheral attachment of a PC that is not the CPU itself, is presented to the system as an entry in the /dev directory.

|Name       |Device |
|-----------|-------|
|/dev/sr*   |“ROM” driver (data-oriented optical disc drives; scd is just a secondary alias) |
|/dev/nvme* |NVME devices |
|/dev/pty*  |Pseudo terminals |
|/dev/sd*   |SCSI disks with their partitions |
|/dev/dm*   |Virtual block devices |
|/dev/rtc*  |Real-time clock |
|/dev/tty*  |Virtual consoles simulating vt100 terminals. |
|/dev/usb*  |USB card and scanner |

```bash
[vault@centos lesson3]$ ls -l /dev
...
crw-------. 1 root root    252,   0 Dec  8 12:03 rtc0
brw-rw----. 1 root disk      8,   0 Dec  8 12:03 sda
brw-rw----. 1 root disk      8,   1 Dec  8 12:03 sda1
brw-rw----. 1 root disk      8,   2 Dec  8 12:03 sda2
crw-rw----. 1 root cdrom    21,   0 Dec  8 12:03 sr0
...
```

## FIFO

FIFOs (also known as named pipes) provide a unidirectional interprocess communication channel. It is accessed as part of the filesystem.
It can be opened by multiple processes for reading or writing. When processes are exchanging data via the FIFO, the kernel passes all data internally without writing it to the filesystem.

mkfifo - make FIFOs (named pipes)

```bash
[vault@centos ~]$ mkfifo testpipe
[vault@centos ~]$ ll -i testpipe
594021 prw-rw-r--. 1 vault vault 0 Dec 14 15:02 testpipe
```

## Sockets
Sockets are a way to enable inter-process communication between programs running on a server, or between programs running on separate servers

Types of sockets:
- Stream sockets, which use TCP as their underlying transport protocol.
    Stream sockets are connection oriented, which means that packets sent to and received from a network socket are delivered by the host operating system in order for processing by an application. Network based stream sockets typically use the Transmission Control Protocol (TCP) to encapsulate and transmit data over a network interface.

    ```bash
    socat TCP4-LISTEN:9090,fork -
    nc 127.0.0.1 9090
    ```

- Datagram sockets, which use UDP as their underlying transport protocol
    Datagram sockets are connectionless, which means that packets sent and received from a socket are processed individually by applications. Network-based datagram sockets typically use the User Datagram Protocol (UDP) to encapsulate and transmit data.

    ```bash
    socat UDP4-LISTEN:9090,fork -
    nc -u 127.0.0.1 9090
    ```

- Unix Domain Sockets, which use local files to send and receive data instead of network interfaces and IP packets.
    Programs that run on the same server can also communicate with each other using Unix Domain Sockets (UDS). Unix Domain Sockets can be stream-based, or datagram-based. When using domain sockets, data is exchanged between programs directly in the operating system’s kernel via files on the host filesystem. To send or receive data using domain sockets, programs read and write to their shared socket file, bypassing network based sockets and protocols entirely.

    ```bash
    socat unix-listen:/tmp/stream.sock,fork -
    nc -U /tmp/stream.sock
    ```

Additional

slabtop - display kernel slab cache information in real time
meminfo
iostat -x -m 1

Clear Cache:
- PageCache only.
```bash
# sync; echo 1 > /proc/sys/vm/drop_caches
```
- Dentries and inodes.
```bash
# sync; echo 2 > /proc/sys/vm/drop_caches
```
- Pagecache, dentries, and inodes.
```bash
# sync; echo 3 > /proc/sys/vm/drop_caches
```

---

Links

- [About files and the file system](https://tldp.org/LDP/intro-linux/html/sect_03_01.html)