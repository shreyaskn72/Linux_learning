# **Module 3 — Linux Filesystem**

Beginner-friendly learning material • Architecture • Commands • Hands-on labs

In Module 2, you learned how to navigate Linux using commands such as `pwd`, `ls`, `cd`, `mkdir`, and `cp`.

Now, let's understand **how Linux organizes files, directories, storage devices, and system information**.

This is an important foundation for:

- Linux system administration
- Docker volumes and container filesystems
- Kubernetes persistent volumes
- Cloud VM disk management
- Production troubleshooting

**Learning objectives**

- Understand the Linux filesystem hierarchy.
- Identify important directories and their purpose.
- Understand files, directories, and file types.
- Learn inodes and hard links vs symbolic links.
- Understand mount points, partitions, and filesystems.
- Practice filesystem inspection commands.

**Estimated duration:** 4–5 hours including labs.

# **1. Linux Filesystem Hierarchy**

## **1.1 What is a filesystem?**

A filesystem is the method an operating system uses to organize, store, and retrieve data.

Imagine your computer as a large office building.

- Building = Storage device
- Floors = Directories
- Rooms = Subdirectories
- Documents = Files
- Building entrance = Root directory `/`

Linux organizes its filesystem as a hierarchical tree.

Unlike Windows, which commonly uses drive letters such as `C:\` and `D:\`, Linux presents files and directories under a single root directory.

[Understanding the .ssh Directory - Topic](https://images.openai.com/static-rsc-4/JdRBvTzDZxwaNeHaQYxSZCggY_tsVgS_Y2rpYfAMNUIWxw2nC5SSF6UvwaJym4j3kU3KWvAOGvpJQrWB8j5VwuFRka7S8YMA3toJlU_REk3PAHOPRdpj6-ZQbVuOIwNTEk1cdS4l4As8dYEKC5JHuXKUZESNaGPyH3a_dYdRv-0?purpose=inline)

[d-libro.com](https://d-libro.com/topic/locate-ssh-directory/?utm_source=chatgpt.com)

## **1.2 Linux filesystem tree**

```
/
├── bin
├── boot
├── dev
├── etc
├── home
│   ├── linuxuser
│   └── anotheruser
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── sys
├── tmp
├── usr
└── var
```

The important thing to understand:

**Everything starts from&#x20;**`/`**, the root directory.**

A file such as:

```
/home/linuxuser/projects/app.py
```

is located inside:

```
/
└── home
    └── linuxuser
        └── projects
            └── app.py
```

## **1.3 Explore the filesystem**

Open your Linux terminal and execute:

```
cd /
pwd
ls
ls -lah
```

You are now looking at the root directory.

To explore individual directories:

```
ls /home
ls /etc
ls /var
ls /usr
```

Try:

```
cd /home
pwd
```

Output:

```
/home
```

Then:

```
cd /
cd tmp
pwd
```

Output:

```
/tmp
```

This demonstrates how Linux paths work from the root.

# **2. Important Linux Directories**

Let's understand the directories you will frequently use in real Linux environments.

## **2.1&#x20;**`/`**&#x20;— Root Directory**

The root directory is the starting point of the entire Linux filesystem.

```
cd /
ls
```

Every absolute path starts from `/`.

Example:

```
/etc/ssh/sshd_config
```

The first `/` means root.

### **Important clarification**

`/` and `/root` are completely different.

- `/` = Root of the filesystem
- `/root` = Home directory of the root user

## **2.2&#x20;**`/home`**&#x20;— User Home Directories**

Linux typically stores regular users' personal files under `/home`.

Example:

```
/home/
├── shreyas/
├── devuser/
└── testuser/
```

Each user has their own home directory.

Check your current home:

```
echo "$HOME"
```

Example output:

```
/home/linuxuser
```

Navigate to your home directory:

```
cd ~
```

or:

```
cd "$HOME"
```

Create a file:

```
touch ~/myfile.txt
```

This creates the file inside your home directory.

## **2.3&#x20;**`/etc`**&#x20;— System Configuration**

`/etc` stores system-wide configuration files.

Examples:

| File                   | Purpose                        |
| ---------------------- | ------------------------------ |
| `/etc/passwd`          | User account information       |
| `/etc/group`           | Group information              |
| `/etc/hosts`           | Local hostname-to-IP mappings  |
| `/etc/hostname`        | System hostname                |
| `/etc/fstab`           | Filesystem mount configuration |
| `/etc/ssh/sshd_config` | SSH server configuration       |
| `/etc/os-release`      | OS identification              |

Explore:

```
ls /etc
```

Read OS information:

```
cat /etc/os-release
```

Read hostname:

```
cat /etc/hostname
```

### **Real-world example**

When you configure NGINX, you may edit:

```
/etc/nginx/nginx.conf
```

When you configure SSH, you may edit:

```
/etc/ssh/sshd_config
```

**Think of&#x20;**`/etc`**&#x20;as the system's configuration center.**

## **2.4&#x20;**`/var`**&#x20;— Variable Data**

`/var` stores data that changes while the system operates.

Examples:

| Directory    | Purpose                                                |
| ------------ | ------------------------------------------------------ |
| `/var/log`   | System and application logs                            |
| `/var/cache` | Cached data                                            |
| `/var/lib`   | Persistent application state                           |
| `/var/tmp`   | Temporary files intended to persist longer than `/tmp` |
| `/var/spool` | Queued data such as jobs                               |

Explore:

```
ls /var
ls /var/log
```

Check log files:

```
ls -lh /var/log
```

### **Real-world example**

Suppose your Flask API is running on Linux.

Application logs may be stored under:

```
/var/log/myapp/
```

Database state or package-managed application data may be under:

```
/var/lib/
```

This is why `/var` is important for production troubleshooting.

## **2.5&#x20;**`/tmp`**&#x20;— Temporary Files**

`/tmp` is used for temporary files.

Examples:

- Temporary application files
- Intermediate processing files
- Temporary test data
- Short-lived files

Create a temporary file:

```
touch /tmp/linux-demo.txt
```

Check it:

```
ls -l /tmp/linux-demo.txt
```

Delete it:

```
rm /tmp/linux-demo.txt
```

### **Important**

Files in `/tmp` may be removed automatically during reboot or by system cleanup policies.

Do not use `/tmp` to store important application data.

## **2.6&#x20;**`/usr`**&#x20;— User Applications and System Software**

`/usr` contains much of the system's installed software, libraries, and shared resources.

Common subdirectories:

| Directory    | Purpose                              |
| ------------ | ------------------------------------ |
| `/usr/bin`   | User commands and applications       |
| `/usr/sbin`  | System administration commands       |
| `/usr/lib`   | Libraries                            |
| `/usr/share` | Architecture-independent shared data |
| `/usr/local` | Locally installed software           |

Example:

```
which ls
```

On many Ubuntu systems, the output is:

```
/usr/bin/ls
```

Check:

```
ls /usr/bin
```

You will find many Linux commands and applications.

### `/bin`**&#x20;and&#x20;**`/sbin`

On modern Ubuntu systems, `/bin` and `/sbin` are commonly symbolic links to `/usr/bin` and `/usr/sbin`, respectively, as part of the merged-`/usr` layout.

So you may see:

```
/bin -> /usr/bin
/sbin -> /usr/sbin
```

## **2.7&#x20;**`/opt`**&#x20;— Optional Software**

`/opt` is commonly used for additional or third-party software.

Example:

```
/opt/
├── mycompany/
├── custom-app/
└── vendor-software/
```

You might install custom software under:

```
/opt/myapp
```

This is useful when software is not managed through the normal distribution package layout.

## **2.8&#x20;**`/root`**&#x20;— Root User's Home Directory**

The root user is the administrator account.

Its home directory is:

```
/root
```

Compare:

```
/home/linuxuser
/root
```

The root user is different from the root filesystem.

Try:

```
whoami
echo "$HOME"
```

If you are a regular user, you might see:

```
linuxuser
/home/linuxuser
```

If you are logged in as root:

```
root
/root
```

## **2.9&#x20;**`/dev`**&#x20;— Device Files**

Linux represents many hardware devices and virtual devices through special files under `/dev`.

Examples:

| Device         | Purpose                  |
| -------------- | ------------------------ |
| `/dev/sda`     | Common disk device name  |
| `/dev/sda1`    | A partition on that disk |
| `/dev/nvme0n1` | NVMe disk                |
| `/dev/null`    | Discards written data    |
| `/dev/zero`    | Provides zero bytes      |
| `/dev/tty`     | Terminal device          |
| `/dev/random`  | Randomness interface     |

Explore:

```
ls /dev
```

### **Example:&#x20;**`/dev/null`

```
echo "Hello Linux" > /dev/null
```

The output is discarded.

This is useful when you want to suppress command output.

For example:

```
some-command > /dev/null
```

`/dev/null` is often called the "black hole" of Linux.

## **2.10&#x20;**`/proc`**&#x20;— Process and Kernel Information**

`/proc` is a virtual filesystem.

It does not behave like a normal directory on a disk. It exposes information about running processes and the kernel.

Examples:

| Path            | Information                 |
| --------------- | --------------------------- |
| `/proc/cpuinfo` | CPU information             |
| `/proc/meminfo` | Memory information          |
| `/proc/uptime`  | System uptime               |
| `/proc/version` | Kernel version information  |
| `/proc/<PID>`   | Information about a process |

Explore:

```
cat /proc/cpuinfo
```

Check memory:

```
cat /proc/meminfo
```

Check uptime:

```
cat /proc/uptime
```

You can also use:

```
free -h
uptime
```

These commands provide more user-friendly views.

### **Why is&#x20;**`/proc`**&#x20;useful?**

Suppose you want to investigate memory usage on a Linux server.

You can inspect:

```
cat /proc/meminfo
```

This provides kernel-reported memory information.

## **2.11&#x20;**`/sys`**&#x20;— Kernel and Device Information**

`/sys` is another virtual filesystem.

It exposes information about devices, drivers, and kernel subsystems.

Explore:

```
ls /sys
```

Example:

```
ls /sys/class/net
```

This shows network interfaces known to the kernel.

You might see:

```
eth0
lo
```

Here:

- `eth0` = A network interface
- `lo` = Loopback interface

### `/proc`**&#x20;vs&#x20;**`/sys`

| `/proc`                        | `/sys`                                       |
| ------------------------------ | -------------------------------------------- |
| Process and kernel information | Device and kernel subsystem information      |
| CPU/memory/process details     | Network devices, drivers, device attributes  |
| Mostly informational           | Information and some configurable attributes |

**Remember:** Both are virtual filesystems. They expose kernel information rather than ordinary files stored on disk.

# **3. Files vs Directories**

In Linux, a directory is a special type of file that organizes names and references to filesystem objects.

A regular file stores data.

A directory stores entries that associate filenames with filesystem objects.

For example:

```
projects/
├── app.py
├── config.txt
└── README.md
```

Here:

- `projects` = Directory
- `app.py` = Regular file
- `config.txt` = Regular file
- `README.md` = Regular file

## **Create and inspect**

```
mkdir demo
touch demo/file.txt
ls -l demo
```

Example:

```
-rw-r--r-- 1 linuxuser linuxuser 0 Nov 9 18:00 file.txt
```

The first character tells you the file type.

# **4. Linux File Types**

Linux supports multiple file types.

Use:

```
ls -l
```

The first character in the output indicates the type.

| Symbol | File type         | Example         |
| ------ | ----------------- | --------------- |
| `-`    | Regular file      | `notes.txt`     |
| `d`    | Directory         | `projects/`     |
| `l`    | Symbolic link     | `app-link`      |
| `c`    | Character device  | Terminal device |
| `b`    | Block device      | Disk device     |
| `p`    | Named pipe (FIFO) | IPC pipe        |
| `s`    | Socket            | Unix socket     |

## **4.1 Regular File**

Stores data such as:

- Text
- Python code
- Images
- Videos
- Binary programs

Example:

```
touch file.txt
```

## **4.2 Directory**

Stores filesystem entries.

Example:

```
mkdir projects
```

## **4.3 Symbolic Link**

A symbolic link points to another pathname.

Example:

```
ln -s file.txt file-link
```

## **4.4 Character Device**

Transfers data as a stream of characters or bytes.

Example:

```
/dev/tty
```

## **4.5 Block Device**

Represents devices that support block-based data access.

Example:

```
/dev/sda
```

## **4.6 Named Pipe**

A FIFO allows processes to communicate through a filesystem entry.

Example:

```
mkfifo mypipe
```

## **4.7 Socket**

A socket can be used for communication between processes.

Example:

```
/run/docker.sock
```

Check the file type using:

```
file filename
```

For example:

```
file /etc/passwd
file /dev/null
file /proc/cpuinfo
```

# **5. Inodes — How Linux Identifies Files**

This is a very important Linux filesystem concept.

## **5.1 What is an inode?**

An inode (index node) is a filesystem data structure that stores metadata about a filesystem object.

An inode typically contains:

- File type
- Permissions
- Owner and group
- File size
- Timestamps
- Link count
- Pointers or references to file data blocks

**An inode does not normally store the filename itself.**

The directory stores the filename and associates it with an inode number.

### **Simple analogy**

Imagine a library.

| Library                    | Linux filesystem |
| -------------------------- | ---------------- |
| Book title                 | Filename         |
| Book identification number | Inode number     |
| Book content               | File data        |
| Library catalog            | Directory        |

A book can have more than one title/reference but still be the same physical book.

Similarly, multiple filenames can refer to the same inode.

## **5.2 Check an inode**

Create a file:

```
touch demo.txt
```

Check its inode:

```
ls -i demo.txt
```

Example:

```
123456 demo.txt
```

The number `123456` is the inode number in this example.

You can also use:

```
stat demo.txt
```

Example output:

```
File: demo.txt
Size: 0
Inode: 123456
Links: 1
```

The actual inode number will differ on your system.

## **5.3 Why are inodes important?**

Suppose you create:

```
touch app.py
```

Linux stores the file's metadata and data.

The directory entry associates:

```
app.py → inode 123456
```

If you create another name that points to the same inode, both names refer to the same underlying file.

This is how hard links work.

# **6. Hard Links vs Symbolic Links**

Linux provides two types of links.

- Hard link
- Symbolic link (soft link)

[Permissions, Metadata, and Edge Cases](https://images.openai.com/static-rsc-4/SgbNJyk0t5CaPk0TXt6AvtHniJpMOi5Xq-xveFFInNP_6FYBiu2h4Ac1niU5cZUPmUquv_WuxPmpicjDw42uNVTmK5ja7gxdGE6I8C87-PMqa5JE9pv-5hUS9_Gk-G8WOb0AdfqMcNIK_GHGaEAyDN1R_NZYu6Q-7SKxG_0OVQA?purpose=inline)

[thenodebook.com](https://www.thenodebook.com/file-system/permissions-metadata-edge-cases?utm_source=chatgpt.com)

## **6.1 Hard Link**

A hard link is another directory entry that refers to the same inode.

Create a file:

```
echo "Linux Filesystem" > original.txt
```

Create a hard link:

```
ln original.txt hardlink.txt
```

Check:

```
ls -li original.txt hardlink.txt
```

Example:

```
123456 -rw-r--r-- 2 linuxuser linuxuser original.txt
123456 -rw-r--r-- 2 linuxuser linuxuser hardlink.txt
```

Notice:

- Both files have the same inode number.
- Link count is 2.

Read either file:

```
cat original.txt
cat hardlink.txt
```

Both display the same content.

Now modify the original:

```
echo "Updated Content" >> original.txt
```

Read the hard link:

```
cat hardlink.txt
```

You will see the updated content.

Why?

Because both names point to the same inode and underlying data.

### **Delete the original**

```
rm original.txt
```

Now:

```
cat hardlink.txt
```

The data still exists.

The inode remains accessible through the hard link.

### **Hard link limitations**

- Usually cannot be created across different filesystems.
- Cannot normally be created for directories.
- Both names point to the same inode.

## **6.2 Symbolic Link (Soft Link)**

A symbolic link is a separate filesystem object that stores a reference to another pathname.

Create a file:

```
echo "Linux Filesystem" > original.txt
```

Create a symbolic link:

```
ln -s original.txt softlink.txt
```

Check:

```
ls -li original.txt softlink.txt
```

Example:

```
123456 -rw-r--r-- original.txt
123789 lrwxrwxrwx softlink.txt -> original.txt
```

Notice:

- Different inode numbers.
- `softlink.txt` points to `original.txt`.

Read:

```
cat softlink.txt
```

Output:

```
Linux Filesystem
```

### **Delete the original**

```
rm original.txt
```

Now:

```
cat softlink.txt
```

You will get an error because the target no longer exists.

This is called a dangling symbolic link.

## **6.3 Comparison**

| Feature                | Hard Link                                | Symbolic Link           |
| ---------------------- | ---------------------------------------- | ----------------------- |
| Inode                  | Same as target                           | Different               |
| Points to              | Inode                                    | Pathname                |
| Cross-filesystem       | No                                       | Yes                     |
| Directory linking      | Generally prohibited                     | Allowed                 |
| Target deleted         | Data remains if another hard link exists | Link becomes dangling   |
| Can point to directory | Generally no                             | Yes                     |
| Common usage           | Multiple names for same file             | Shortcuts, shared paths |

### **Practical use cases**

Symbolic link:

```
ln -s /opt/myapp/current /var/www/myapp
```

This can be useful for application deployment layouts.

Hard link:

```
ln data.txt data-backup.txt
```

This creates another name for the same file data.

# **7. Mount Points**

## **7.1 What is a mount point?**

A mount point is a directory where a filesystem is attached to the Linux directory tree.

Imagine you have:

- Disk 1: Operating system
- Disk 2: Application data

Linux can attach Disk 2's filesystem to:

```
/data
```

After mounting, you can access its contents through `/data`.

\#chatgpt-mermaid-\_r_giu\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_giu\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_giu\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_giu\_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_giu\_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_giu\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_giu\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_giu\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_giu\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_giu\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_giu\_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_giu\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_giu\_ p{margin:0;}#chatgpt-mermaid-\_r_giu\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_giu\_ .label text,#chatgpt-mermaid-\_r_giu\_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ .node rect,#chatgpt-mermaid-\_r_giu\_ .node circle,#chatgpt-mermaid-\_r_giu\_ .node ellipse,#chatgpt-mermaid-\_r_giu\_ .node polygon,#chatgpt-mermaid-\_r_giu\_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ .rough-node .label text,#chatgpt-mermaid-\_r_giu\_ .node .label text,#chatgpt-mermaid-\_r_giu\_ .image-shape .label,#chatgpt-mermaid-\_r_giu\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_giu\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ .rough-node .label,#chatgpt-mermaid-\_r_giu\_ .node .label,#chatgpt-mermaid-\_r_giu\_ .image-shape .label,#chatgpt-mermaid-\_r_giu\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_giu\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_giu\_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_giu\_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_giu\_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-\_r_giu\_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_giu\_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_giu\_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_giu\_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-\_r_giu\_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_giu\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_giu\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_giu\_ .icon-shape,#chatgpt-mermaid-\_r_giu\_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_giu\_ .icon-shape p,#chatgpt-mermaid-\_r_giu\_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-\_r_giu\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_giu\_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_giu\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_giu\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_giu\_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_giu\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_giu\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_giu\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_giu\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_giu\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_giu\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_giu\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_giu\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_giu\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_giu\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_giu\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ .node rect,#chatgpt-mermaid-\_r_giu\_ .node circle,#chatgpt-mermaid-\_r_giu\_ .node ellipse,#chatgpt-mermaid-\_r_giu\_ .node polygon,#chatgpt-mermaid-\_r_giu\_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_giu\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_giu\_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_giu\_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_giu\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_giu\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Linux Root Filesystem //home/etc/data — Mount PointFilesystem on Disk 2

## **7.2 Example**

Suppose a disk partition is:

```
/dev/sdb1
```

You want to mount it at:

```
/data
```

The general commands are:

```
sudo mkdir -p /data
sudo mount /dev/sdb1 /data
```

After mounting:

```
ls /data
```

You can access the files from that filesystem.

**Warning:** Do not run these commands on a real disk unless you know what it contains. Mounting an existing filesystem can expose or modify its contents. Formatting a disk can destroy data.

For now, let's inspect existing mounts without changing anything.

## **7.3 Explore mount points**

Run:

```
findmnt
```

This displays mounted filesystems.

To see the root filesystem:

```
findmnt /
```

To see disk usage:

```
df -h
```

To see mounted filesystems with filesystem types:

```
df -Th
```

### **What is&#x20;**`/etc/fstab`**?**

`/etc/fstab` defines filesystems that Linux should mount during boot or through system management.

Explore:

```
cat /etc/fstab
```

Example:

```
UUID=xxxx-xxxx  /data  ext4  defaults  0  2
```

This is only an illustrative example. Do not copy it into your actual `/etc/fstab` without a real filesystem UUID and a valid configuration.

# **8. Disk Partitions and Filesystems**

These terms are related, but they mean different things.

## **8.1 Disk**

A disk is a storage device.

Examples:

- HDD
- SSD
- NVMe SSD
- Virtual disk

Linux may identify disks as:

```
/dev/sda
/dev/sdb
/dev/nvme0n1
```

The names depend on hardware and configuration.

## **8.2 Partition**

A partition is a logical division of a disk.

Example:

```
/dev/sda
├── /dev/sda1
├── /dev/sda2
└── /dev/sda3
```

Here, `/dev/sda1`, `/dev/sda2`, and `/dev/sda3` are partitions.

A partition can hold a filesystem, swap space, or other data structures.

## **8.3 Filesystem**

A filesystem defines how data is organized on a storage volume.

Common Linux filesystems:

| Filesystem | Description                                  |
| ---------- | -------------------------------------------- |
| ext4       | Common general-purpose Linux filesystem      |
| XFS        | Common in enterprise/server environments     |
| Btrfs      | Supports advanced features such as snapshots |
| tmpfs      | Memory-backed temporary filesystem           |
| FAT32      | Common in removable media                    |
| NTFS       | Common Windows filesystem                    |

## **8.4 Disk → Partition → Filesystem → Mount Point**

\#chatgpt-mermaid-\_r_gks\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_gks\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_gks\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_gks\_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_gks\_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_gks\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_gks\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_gks\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_gks\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_gks\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_gks\_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_gks\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_gks\_ p{margin:0;}#chatgpt-mermaid-\_r_gks\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_gks\_ .label text,#chatgpt-mermaid-\_r_gks\_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ .node rect,#chatgpt-mermaid-\_r_gks\_ .node circle,#chatgpt-mermaid-\_r_gks\_ .node ellipse,#chatgpt-mermaid-\_r_gks\_ .node polygon,#chatgpt-mermaid-\_r_gks\_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ .rough-node .label text,#chatgpt-mermaid-\_r_gks\_ .node .label text,#chatgpt-mermaid-\_r_gks\_ .image-shape .label,#chatgpt-mermaid-\_r_gks\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_gks\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ .rough-node .label,#chatgpt-mermaid-\_r_gks\_ .node .label,#chatgpt-mermaid-\_r_gks\_ .image-shape .label,#chatgpt-mermaid-\_r_gks\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_gks\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_gks\_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_gks\_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_gks\_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-\_r_gks\_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_gks\_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_gks\_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_gks\_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-\_r_gks\_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_gks\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_gks\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_gks\_ .icon-shape,#chatgpt-mermaid-\_r_gks\_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_gks\_ .icon-shape p,#chatgpt-mermaid-\_r_gks\_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-\_r_gks\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_gks\_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_gks\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_gks\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_gks\_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_gks\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_gks\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_gks\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_gks\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_gks\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_gks\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_gks\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_gks\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_gks\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_gks\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_gks\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ .node rect,#chatgpt-mermaid-\_r_gks\_ .node circle,#chatgpt-mermaid-\_r_gks\_ .node ellipse,#chatgpt-mermaid-\_r_gks\_ .node polygon,#chatgpt-mermaid-\_r_gks\_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_gks\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_gks\_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_gks\_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_gks\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_gks\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Physical or Virtual DiskPartitionFilesystem — ext4 / XFSMount Point — /dataApplication Reads/WritesFiles

### **Important clarification**

A disk does not always need a partition table or partitions. Linux can also use filesystems directly on a disk or use logical volumes.

For this module, remember the common pattern:

```
Disk
  ↓
Partition
  ↓
Filesystem
  ↓
Mount Point
  ↓
Files and Directories
```

# **9. Linux Filesystem Commands — Cheat Sheet**

Keep this reference for daily Linux administration.

| Command   | Purpose                   | Example                   |
| --------- | ------------------------- | ------------------------- |
| `ls`      | List files                | `ls -lah /etc`            |
| `tree`    | Display directory tree    | `tree ~/linux-lab`        |
| `find`    | Search filesystem         | `find /tmp -name "*.txt"` |
| `file`    | Identify file type        | `file /etc/passwd`        |
| `stat`    | Display file metadata     | `stat app.py`             |
| `ls -i`   | Display inode             | `ls -i app.py`            |
| `ln`      | Create links              | `ln -s a.txt b.txt`       |
| `df`      | Filesystem disk usage     | `df -h`                   |
| `du`      | Directory/file size       | `du -sh ~/linux-lab`      |
| `lsblk`   | List block devices        | `lsblk`                   |
| `blkid`   | Show filesystem UUID/type | `sudo blkid`              |
| `findmnt` | Show mount points         | `findmnt`                 |
| `mount`   | Attach filesystem         | `mount`                   |
| `umount`  | Detach filesystem         | `umount /data`            |
| `cat`     | Read file                 | `cat /etc/os-release`     |

Note: `tree` may not be installed. On Ubuntu:

```
sudo apt install tree -y
```

# **10. Hands-on Lab — Explore the Linux Filesystem**

Lab 1 — Safe Exploration

## **Step 1 — Explore important directories**

```
cd /
ls
```

Then inspect:

```
ls /home
ls /etc
ls /var
ls /usr
ls /tmp
ls /dev
ls /proc
ls /sys
```

## **Step 2 — Create a filesystem lab**

```
mkdir -p ~/linux-lab/module3
cd ~/linux-lab/module3
```

Create files:

```
mkdir documents backup
touch documents/file1.txt
touch documents/file2.txt
```

Add content:

```
echo "Linux filesystem learning" > documents/file1.txt
```

## **Step 3 — Inspect file metadata**

```
ls -li documents
stat documents/file1.txt
file documents/file1.txt
```

Observe:

- Inode number
- File size
- Permissions
- Link count

## **Step 4 — Create a hard link**

```
ln documents/file1.txt hardlink.txt
```

Check:

```
ls -li documents/file1.txt hardlink.txt
```

The inode numbers should match.

## **Step 5 — Create a symbolic link**

```
ln -s documents/file1.txt softlink.txt
```

Check:

```
ls -li documents/file1.txt softlink.txt
```

The inode numbers should be different.

## **Step 6 — Test hard link behavior**

```
echo "New content" >> documents/file1.txt
cat hardlink.txt
```

You should see the new content.

## **Step 7 — Test symbolic link behavior**

```
cat softlink.txt
```

It should display the file content.

## **Step 8 — Inspect storage**

```
df -h
df -Th
du -sh ~/linux-lab
lsblk
findmnt
```

These commands are especially useful when troubleshooting Linux servers.

# **11. Hands-on Lab — Filesystem Investigation**

This lab helps you think like a Linux administrator.

Imagine your Linux server has the following issues.

| Scenario                              | Command to investigate |
| ------------------------------------- | ---------------------- |
| Disk is almost full                   | `df -h`                |
| Application directory is too large    | `du -sh /path`         |
| Need to identify a file type          | `file filename`        |
| Need to inspect file metadata         | `stat filename`        |
| Need to understand a mounted disk     | `findmnt`              |
| Need to identify disks and partitions | `lsblk`                |
| Need to inspect CPU information       | `cat /proc/cpuinfo`    |
| Need to inspect memory information    | `cat /proc/meminfo`    |
| Need to inspect network interfaces    | `ls /sys/class/net`    |

Try each command and explain what the output means.

# **12. Knowledge Check**

**1. What is the root directory of Linux?**

/root

/home

/

/etc

Correct answer: /

**2. Where are system-wide configuration files generally stored?**

/tmp

/etc

/dev

/home

Correct answer: /etc

**3. Which directory contains virtual process and kernel information?**

/proc

/opt

/root

/usr

Correct answer: /proc

**4. What is an inode?**

A filename

A filesystem metadata structure

A disk partition

A mount point

Correct answer: A filesystem metadata structure

**5. A hard link and its original file generally have:**

Different inode numbers

The same inode number

Different filesystems

Different content

Correct answer: The same inode number

**6. A symbolic link refers to:**

A pathname

A CPU

A disk partition only

A process ID

Correct answer: A pathname

**7. What is a mount point?**

A filename

A directory where a filesystem is attached

A kernel module

A disk format

Correct answer: A directory where a filesystem is attached

**8. Which command displays mounted filesystems?**

findmnt

touch

echo

history

Correct answer: findmnt

Check Answers

## **Score: 8/8**

Try Again

## **Module 3 Completion Checklist**

**Completion Tracker**

0/8

I understand the Linux filesystem tree and root directory.

I can explain the purpose of /home, /etc, /var, /tmp, /usr, /opt, and /root.

I understand /dev, /proc, and /sys.

I can identify Linux file types.

I understand inodes.

I can create and explain hard links and symbolic links.

I understand disk, partition, filesystem, and mount point.

I can use lsblk, df, du, stat, and findmnt.

## **Next Module**

**Module 4 — File Permissions and Ownership**

We'll learn:

- Linux users and groups
- `chmod`, `chown`, `chgrp`
- Read, write, execute permissions
- Numeric permissions: 755, 644, 700
- `umask`
- SUID, SGID, Sticky Bit
- ACLs
- `sudo` and privilege escalation

This next module will be especially useful when deploying Flask APIs, Docker containers, and Kubernetes workloads on Linux.
