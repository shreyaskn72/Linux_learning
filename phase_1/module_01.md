# **Module 1 — Introduction to Linux**

Beginner-friendly learning material + practical labs

This module is designed to help you understand Linux from the ground up. By the end, you should be able to explain what Linux is, understand its architecture, choose a Linux distribution, install Ubuntu, and connect to a Linux server using SSH.

**Estimated time:** 4–6 hours including labs.

## **Learning objectives**

After completing this module, you should be able to:

- Explain Linux in simple terms.
- Understand the difference between the Linux kernel and the operating system.
- Choose an appropriate Linux distribution.
- Understand Linux architecture.
- Explain CLI vs GUI.
- Install Ubuntu in a virtual environment.
- Connect to a remote Linux machine using SSH.

# **1. What is Linux?**

Imagine you have a computer with a CPU, RAM, hard disk, keyboard, network card, and monitor.

You need software to manage all these hardware resources and allow applications to run.

That software is the Operating System (OS).

Linux is an open-source operating system family built around the Linux kernel.

Examples of operating systems based on Linux:

- Ubuntu
- Debian
- Red Hat Enterprise Linux (RHEL)
- Fedora
- Alpine Linux

### **Simple real-world analogy**

Think of a restaurant.

| Restaurant                | Computer             |
| ------------------------- | -------------------- |
| Kitchen equipment         | Hardware             |
| Kitchen manager           | Linux Kernel         |
| Restaurant operations     | Operating System     |
| Waiter taking orders      | Shell                |
| Customer                  | User                 |
| Food ordering application | Application software |

The kernel manages the hardware. Applications use the kernel to perform their work.

### **Why is Linux important?**

Linux powers many systems:

- Cloud servers
- Web servers
- Docker containers
- Kubernetes worker nodes
- Android devices (kernel)
- Networking equipment
- Embedded systems
- Supercomputers

For a DevOps engineer, Linux is a fundamental skill because many production workloads run on Linux.

# **2. Linux Kernel vs Operating System**

This is one of the most important concepts.

## **What is the Linux Kernel?**

The kernel is the core component of the operating system.

It manages:

- CPU scheduling
- Memory management
- Hardware devices
- Processes
- Filesystems
- Networking
- System calls

Applications do not directly control hardware. They request services from the kernel.

[Linux Made Simple: The Ultimate Beginner’s Guide to Essential Usage | by Sami ST | Mar, 2026 | Medium](https://images.openai.com/static-rsc-4/4HKwozSLoAJhJtWk_v3Y7JBoKejgCGR7gEl7QCr5CFog5RRqUHb6p6A3hGq3XpMP1_mJoNPMpnQU7n-GKvjFnUSNTXQ9aJiaiMt4Ej84izpqC4-YchvXDS2-GtGxI-bLE5d0aObq153tTqARBXDVgDhGVWq9b6El8vBlOSYHFjE?purpose=inline)

[Understanding the Linux Kernel: A Beginner’s Guide | by Anup Keshwani | Medium](https://images.openai.com/static-rsc-4/Ji71AB9LDbpQBI9hwgtMIrOdP7jJ-KkOZJsEb7WaVaJrSBP4R7-gkIonORPt9MfKgYFHEc2faGVio7YiOAX8iixziYtC8NVv7DNW3YKabzB9jf-wQXok3pLHEHE7lDBugDNMHEW5rcLiAhDeDkGKtq5FXrUnBUcuB22YIsXTLuY?purpose=inline)

[PPT - Linux System Call PowerPoint Presentation, free download - ID:6419038](https://images.openai.com/static-rsc-4/Jzzew9ONUjDptmNiJkBJTmUUpSVvzh3vc6-2cbrDWdxiVFjxu0bi26oBqsg5KQkHy6kaBFE7R-ITCnliJuLrD7_bdb9TrAygsUC4fynJMvVNe_G1xyQNqJzkDX8ecWH9Y3DHIOsml2O0E8UbpNGCgaUanp0TqGcAtgwcmmx687Q?purpose=inline)

6

### **Example: Reading a file**

Suppose you execute:

```
cat hello.txt
```

What happens?

1. The `cat` program requests the file contents.
2. It makes a system call.
3. The Linux kernel accesses the filesystem.
4. The kernel retrieves the file data.
5. The data is returned to `cat`.
6. `cat` prints the contents in the terminal.

### **Linux Kernel vs Linux Distribution**

| Linux Kernel                                  | Linux Distribution                                 |
| --------------------------------------------- | -------------------------------------------------- |
| Core of the OS                                | Complete packaged operating system                 |
| Manages hardware and resources                | Includes kernel, tools, libraries, package manager |
| Example: Linux kernel 6.x                     | Example: Ubuntu 24.04                              |
| Developed primarily by Linux kernel community | Maintained by distribution communities or vendors  |

**Important:** Ubuntu is not the Linux kernel. Ubuntu is a Linux distribution that uses the Linux kernel.

# **3. Linux Distributions**

A Linux distribution (distro) is a complete operating system built using the Linux kernel and a collection of software.

Different distributions are designed for different needs.

[대학생이 리눅스 우분투를 2년간 써보면서 (Ubuntu 22.04) — 쓰기 전에 생각하자](https://images.openai.com/static-rsc-4/bF59MORMZ4qphvtglvWGYgBa5ni09ANAlMmB55QUd-o9yFozSeAyRZhIul-Yhtc7SobhfLUU-4MCQriFsymzoz_2dAFJUCIsw9UYvm3azBeLxrfJ1rODldsoPSSI2YcYF7uQIgpvcbzob4CNfV8QIOZBpxwtKnhhXzgklJ4o9pM?purpose=inline)

[Debial 13 "Trixie" Lands with RISC-V Support — But Prepares to Drop a Few Other Architectures - Hackster.io](https://images.openai.com/static-rsc-4/1ABelhyoIAJHRYzSNbMCvZv_P1h-MurDfT9JErYMgR9W5rhd6MQCVxp9dr1dLlbEvc7tIK8Bl1RJyHgm2DovTaXxFjsv_uodahuh-AZco0gfI5uaAGDEtpqWU7RrsFHKy3KP3_rf_Z11M5yeR3meqqLTJYjz33B45GFxBn7f5Do?purpose=inline)

[What Makes GNOME So Appealing? - The New Stack](https://images.openai.com/static-rsc-4/POuluGLeG-7MVdvSbQN5TE0XrHRmRd7CBUVMewWr89Vh7TENExIa4nDEL4-Q8HPuRU3Ho_RJ97STE_vhQmqxKoN3oa5_6tv7UrZxrWmbO-2OQ-8nDhvC_Gk8BGc6Cr-cIfqG8q9gOcATC61rZxr7b3O17XXySoCoe7FODdFv3u8?purpose=inline)

5

## **Major Linux distributions**

[ClinicMaster INTERNATIONAL](https://images.openai.com/static-rsc-4/LL_uhk0iJjO3vJzlkX9BOPATizCsH_I7ipcK4Q9hlUASNvX1cbd48I__CpMr81WyGky80KcgQbY3q4zGdmg2LOqoUJLHl-5C9wLXOdnFXqMyjcljjnaOwOrqTzj69IqCFuoXYrpzCMUobZxr-uhLozDhk8Hnd9tT5c1AUBQ8rVE?purpose=inline)

**Ubuntu**

Recommended for beginners

- Beginner-friendly
- Large community
- Common in cloud environments
- Package manager: `apt`

[Software We Use At NixNexus - NixNexus - Your Guide to everything FOSS](https://images.openai.com/static-rsc-4/oQLMZJ5NWzk43EGLhYMslNEVN8F1l_EH4xxV0UNmIrjEA4CfVsV41sJNau6RtUOuvUQIgSmpqY0AuYTVVJt3szz23XQz8rmBqFka5DdSUv8b7LFpDD1S9-4ABn8nvDJONySZWntm5ATShZ9HUpT2VG3GAD71f_XdussdqaEPSGY?purpose=inline)

**Debian**

- Stability-focused
- Community-maintained
- Foundation for Ubuntu
- Package manager: `apt`

[RHEL, Red Hat Enterprise Linux | Piros](https://images.openai.com/static-rsc-4/EjVvGt3mJ4qjZ_lrzyGgIBMGV35FRC6XLF9_SmD_JC8iKHnIw1VoTOsgi5UzQiLbC6Y2ydH1wd0kGHyxS8COY1Oy5mDpGPgsibK-ZLBOOEW1XD9QYT1sq1DV67rsF6x8egnIwIEpvOrYMi2xabKB6F53vex_wsjYUNROPguFcm0?purpose=inline)

**Red Hat Enterprise Linux (RHEL)**

- Enterprise Linux
- Commercial support
- Common in enterprise environments
- Package manager: `dnf`

[Fedora Logo PNG Transparent & SVG Vector - Freebie Supply](https://images.openai.com/static-rsc-4/ewWtKFnExEFk9bDNlTzj0pBTmAoe13ptlLDOrjAt_H3VEvuW6U4wte_rWCB6EASjY-v80a5JO5XK7iTSoJ6Eb7widRK34MGN3OpkRzLUY7yvVVqiTvm5frHcFPrHw_Sd9SESkI5pvBO3TImr-_y2NuutwSTRvEsnxYz7D6g1UIg?purpose=inline)

**Fedora**

- Modern Linux technologies
- Community-driven
- Upstream ecosystem for RHEL
- Package manager: `dnf`

[Alpine Linux: Great Even Outside of Containers | by Umair | Medium](https://images.openai.com/static-rsc-4/RSgCyEqrmTO4d5_RnxfZLFCRax4_U3CSRLryjEY3Yg55oX0LzEqlbY-YetVCPm9uTB17Lv-FclQpJyu-mVpcYtGAtuQUhfkChgmSrBooIVXyjCqhVUIM4KqbOpR29cCz1l8ilKjnND18PBOw8aRWLhH8XFktuMgs5MoXhvGNLk8?purpose=inline)

**Alpine Linux**

- Lightweight
- Popular for minimal containers
- Package manager: `apk`

### **Which distribution should you learn?**

Start with Ubuntu.

Once you understand Ubuntu, learning RHEL-based distributions becomes easier because most Linux fundamentals remain the same.

For now, focus on:

- Ubuntu commands
- Filesystem
- Permissions
- Processes
- Networking
- Bash scripting

Learn `dnf`, SELinux, and RHEL-specific administration later.

# **4. Linux vs Windows vs macOS**

All three are operating systems, but their architecture and typical use cases differ.

| Feature            | Linux              | Windows                | macOS                  |
| ------------------ | ------------------ | ---------------------- | ---------------------- |
| Kernel             | Linux              | Windows NT             | XNU                    |
| Source model       | Mostly open source | Mostly proprietary     | Mostly proprietary     |
| CLI                | Bash, Zsh, etc.    | PowerShell, CMD        | Zsh, etc.              |
| Package management | APT, DNF, APK      | Winget, etc.           | Homebrew, etc.         |
| Server usage       | Very common        | Common                 | Less common            |
| Cloud-native usage | Very common        | Common for development | Common for development |
| Customization      | Very high          | Moderate               | More limited           |

### **Important clarification**

macOS is Unix-based, but it is not Linux.

Both Linux and macOS provide Unix-like command-line environments, so many commands are similar:

```
ls
cd
pwd
cat
grep
```

However, some commands differ in options and behavior. For example, Linux commonly uses GNU tools, while macOS ships with BSD variants.

# **5. Linux Architecture — Kernel, Shell, User Space**

Let's understand how the components communicate.

[Linux Made Simple: The Ultimate Beginner’s Guide to Essential Usage | by Sami ST | Mar, 2026 | Medium](https://images.openai.com/static-rsc-4/4HKwozSLoAJhJtWk_v3Y7JBoKejgCGR7gEl7QCr5CFog5RRqUHb6p6A3hGq3XpMP1_mJoNPMpnQU7n-GKvjFnUSNTXQ9aJiaiMt4Ej84izpqC4-YchvXDS2-GtGxI-bLE5d0aObq153tTqARBXDVgDhGVWq9b6El8vBlOSYHFjE?purpose=inline)

[samist.medium.com](https://samist.medium.com/linux-made-simple-the-ultimate-beginners-guide-to-essential-usage-bc21cdd6cbad?utm_source=chatgpt.com)

## **Architecture layers**


    flowchart TB
    U["Users"]
    A["Applications: Flask, NGINX, Docker"]
    S["Shell: Bash, Zsh"]
    L["System Libraries"]
    K["Linux Kernel"]
    H["Hardware: CPU, RAM, Disk, Network"]

    U --> A
    U --> S
    A --> L
    S --> L
    L -->|"System Calls"| K
    K --> H

### **1. User Space**

This is where regular applications execute.

Examples:

- Python
- Flask
- NGINX
- Bash
- Docker CLI
- Text editors

Applications generally cannot directly access hardware. They interact with the kernel through system calls and supporting libraries.

### **2. Shell**

A shell is a command interpreter.

You type:

```
ls -l
```

The shell interprets the command and launches the `ls` program.

Popular shells:

| Shell | Description                     |
| ----- | ------------------------------- |
| Bash  | Common Linux shell              |
| Zsh   | Popular interactive shell       |
| Sh    | Traditional shell interface     |
| Fish  | User-friendly interactive shell |

### **3. Kernel Space**

The kernel operates with privileged access to hardware and system resources.

It handles:

- Process scheduling
- Memory allocation
- Network packet processing
- Filesystem operations
- Device drivers

### **4. Hardware**

The actual computer resources:

- CPU
- RAM
- Disk
- Network interface
- USB devices

### **Example: Flask API request**

Imagine you deploy a Flask API on Linux.

```
Client
  |
  v
NGINX
  |
  v
Gunicorn
  |
  v
Flask Application
  |
  v
Linux Kernel
  |
  v
CPU / RAM / Network
```

Flask does not manage the CPU or network hardware directly. Linux handles those responsibilities.

# **6. CLI vs GUI**

[What Is Ubuntu?](https://images.openai.com/static-rsc-4/CVNhAzxyLXkQp7kEd2V091JS60bdVxc0tCnP-fbeDWu3wCOA4S1_zONIsu6MVjTgPERdKdu48NwoKw5dDv27GJxw2YMY-7Y05d8PV8P3OrkhRwvufre_CNCFZkGho9D3HHMWsFirOhVbHbUCa4KYp3O0pQDBFoT18ozaIPZ5So4?purpose=inline)

[The Linux command line for beginners - Ubuntu Desktop documentation](https://images.openai.com/static-rsc-4/TWruKNqYQumj6tFWCGD_KsTWJyjrJlhNxj9ugRN_86MnKsTNHpwk_DmgDaFGfFdUMB09yzXjoM7V1EqXSg-gi7b0a0e20tcA7JJMf1HDDOnd_EGOZ2LpaQT2sZOi4_d-5_-9TjPD4fbow_PAhALicZpBjXxKB3Epdblzp6q1G0U?purpose=inline)

[Ubuntu - Command Line](https://images.openai.com/static-rsc-4/FcWXDAlUeV00Oj05IIUUcSYtvRL1Xa5QAnRlmHZUl7uQNKy93lXTO5kIoRlH1M6-cuzYu-YHo78LQQ9QptMffE-g7-51gerCRi2ocx7qFMou2SFX_Lxl4-hsw5NEf3GNiMROdNxD6gAkF97yVmIENGMnkl4Gk4joxl5mOFb215c?purpose=inline)

## **GUI — Graphical User Interface**

GUI allows you to interact with a system using visual elements.

Examples:

- Desktop
- Windows
- Icons
- File manager
- Mouse

Ubuntu Desktop is a GUI-based Linux environment.

## **CLI — Command Line Interface**

CLI allows you to interact with a system through text commands.

Example:

```
pwd
ls
mkdir linux-lab
cd linux-lab
touch hello.txt
```

### **When should you use CLI?**

| Task                    | GUI       | CLI       |
| ----------------------- | --------- | --------- |
| Browse files            | Easy      | Easy      |
| Create files            | Easy      | Easy      |
| Manage 100 servers      | Difficult | Efficient |
| Automate deployments    | Limited   | Excellent |
| Troubleshoot production | Useful    | Essential |
| Kubernetes operations   | Limited   | Essential |

**DevOps principle:** GUI is useful for exploration, but CLI is essential for automation and production operations.

# **7. Installing Linux — Your First Hands-on Setup**

There are multiple ways to install Linux. For learning, you do not need to replace your current operating system.

**Option A — WSL2 (Windows)**

Recommended for Windows

Run Ubuntu directly within Windows. Great for command-line learning, Python, Docker, and development.

**Option B — VirtualBox VM**

Recommended for full OS learning

Run a complete Ubuntu Desktop or Server machine inside your computer. Useful for learning booting, disks, networking, and services.

**Option C — Cloud VM**\
Create an Ubuntu VM on Azure, AWS, or another cloud provider. Useful for practicing SSH and real server administration.

## **7.1 Install Ubuntu using WSL2**

If you are using Windows 10 or Windows 11, this is a simple way to get started.

### **Step 1 — Open PowerShell**

Open PowerShell as Administrator.

### **Step 2 — Install WSL**

```
wsl --install
```

This command installs the required WSL components and typically installs Ubuntu. Restart your machine if prompted.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



### **Step 3 — Open Ubuntu**

Open Ubuntu from the Start menu.

The first time you launch it, create a Linux username and password.

Example:

```
Enter new UNIX username: linuxuser
New password:
Retype new password:
```

Note: While typing a Linux password, nothing appears on the screen. This is normal.

### **Step 4 — Verify the installation**

Inside the Ubuntu terminal:

```
whoami
pwd
uname -a
cat /etc/os-release
```

Expected output examples:

```
linuxuser
/home/linuxuser
Linux ...
```

### **Step 5 — Update Ubuntu**

```
sudo apt update
sudo apt upgrade -y
```

### **Step 6 — Check your WSL installation**

Back in PowerShell:

```
wsl --list --verbose
```

You should see Ubuntu and its WSL version.

Official installation guides:

- [Microsoft — Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install?utm_source=chatgpt.com)
- [Ubuntu — Install Ubuntu on WSL2](https://ubuntu.com/wsl/docs/latest/howto/install-ubuntu-wsl2/?utm_source=chatgpt.com)

## **7.2 Install Ubuntu using VirtualBox**

If your goal is to learn Linux system administration, a full virtual machine is a great option.

[Install Ubuntu on VirtualBox and Configure it Properly | WxGuy](https://images.openai.com/static-rsc-4/x7euDWIeZAatXgUCiWxZ8C8FTAVRUf27g2kUABv1r9MH0YczSlB7Akytc030ys7_L9mz5LVEX9iUsHPUfdxj--XZkfqomC1nskaiSpLHM_79QrEu5uq4UIAq8xbV0UyUXeYa_nIC47iPMmCrIiA6p6lanJLAEIt-rHZN1wu3u-g?purpose=inline)

[How to install Ubuntu on VirtualBox? - GeeksforGeeks](https://images.openai.com/static-rsc-4/f4FmnOulSfyLOHvhY9THeCPyCw_7jvN5_Kd69mXYPntHxvF1LtuFsgpz1FqJv1ygW4ieHQghXqJwRuKdlOhZDFqlVNNOpSYnAMD9glzzOFunlcp3FbjcHXGhwnQEzPsHygF1g9x4Ont7hO_qmidKA966foSf8Yx7s76JQj2A_Og?purpose=inline)

[Installing Linux Using a Virtual Machine | Codecademy](https://images.openai.com/static-rsc-4/qAARqj7kodGSxUtc1bq6SdLWMatFTaUZc1l-3NJiYazjR76NPITekiphbP31hKlVE-7YXpNYIFUaPzXHuFpVu0jsG4U5spNwHrqyDw9ajp9uAU8yStUbmaPZOfwmQsHOM5QEYawNMKX8etYQY-QMS0GCNrEBugdVGbXTEJ0ErLI?purpose=inline)

5

### **Step 1 — Download the required software**

- [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads?utm_source=chatgpt.com)
- [Download Ubuntu Desktop ISO](https://ubuntu.com/download/desktop?utm_source=chatgpt.com)

### **Step 2 — Create a virtual machine**

In VirtualBox:

1. Click **New**.
2. Name the VM `Ubuntu-Linux-Lab`.
3. Select the downloaded Ubuntu ISO.
4. Choose Linux / Ubuntu 64-bit if prompted.
5. Allocate RAM — 4 GB is a reasonable starting point if your host has enough memory.
6. Allocate 2 CPUs.
7. Create a virtual disk — around 25–40 GB for a basic lab.

These are suggested learning settings, not minimum requirements.

### **Step 3 — Install Ubuntu**

Start the VM and follow the Ubuntu installer.

Choose:

- Normal installation
- Create a user account
- Install to the VM's virtual disk

**Important:** Only select the virtual machine's disk. Do not modify your host computer's disk.

### **Step 4 — Verify the installation**

Open the Ubuntu terminal:

```
uname -r
lsb_release -a
whoami
```

### **WSL2 vs VirtualBox**

| Feature                      | WSL2                            | VirtualBox |
| ---------------------------- | ------------------------------- | ---------- |
| Installation                 | Simple                          | More steps |
| Full Linux OS experience     | Limited compared with a full VM | Yes        |
| Linux CLI learning           | Excellent                       | Excellent  |
| Boot process learning        | Limited                         | Excellent  |
| Disk/partition labs          | Limited                         | Better     |
| Production-style server labs | Good                            | Excellent  |

For this module, either installation is enough. You can begin with WSL2 and later use a VM for advanced Linux labs.

# **8. SSH — Connecting to a Linux Machine**

## **What is SSH?**

SSH stands for Secure Shell.

It is a network protocol that allows you to securely connect to a remote machine and execute commands.

Imagine:

- Your laptop is in Bengaluru.
- A Linux server is running in Azure.
- You want to administer the server remotely.

You can connect using SSH instead of physically accessing the server.

[Hostinger Tutorials](https://images.openai.com/static-rsc-4/sAYhBWR0G3LSmyK296fq_dFntUM0dZTO5GqpdMGroIS5-j-QMk-i0yGK5eTnnRzEp0Q08NAz2-b_WEG8p8XLiz3-mmTn8EcEqRzAevQXzr_3tcn3lJ8BWHNAbOE0VXGJcmBCPC48FhHqBjroAYfEoPGSD5SrU8BV97mtnLX6Oi0?purpose=inline)

[hostinger.in](https://www.hostinger.in/tutorials/ssh-tutorial-how-does-ssh-work?utm_source=chatgpt.com)

### **SSH architecture**


    flowchart LR
    A["Your Laptop<br/>SSH Client"]
    B["Network<br/>Encrypted SSH"]
    C["Linux Server<br/>SSH Server"]
    D["Linux Shell"]

    A --> B --> C --> D

SSH uses TCP port 22 by default.

The two main components are:

| Component           | Purpose                    |
| ------------------- | -------------------------- |
| SSH Client          | Initiates the connection   |
| SSH Server (`sshd`) | Accepts remote connections |

## **8.1 SSH command**

The basic SSH syntax is:

```
ssh username@server-ip
```

Example:

```
ssh linuxuser@192.168.1.100
```

Here:

- `linuxuser` = account on the remote server
- `192.168.1.100` = server IP address
- `ssh` = command to establish the connection

You may be prompted to confirm the server's host key and enter your password.

Only accept a host key after verifying it when connecting to a server you administer.

## **8.2 Lab — Configure SSH on Ubuntu**

This lab works on a full Ubuntu VM or a Linux machine you control.

### **Step 1 — Install the SSH server**

On the Linux server:

```
sudo apt update
sudo apt install openssh-server -y
```

### **Step 2 — Start the SSH service**

```
sudo systemctl enable --now ssh
```

Check its status:

```
sudo systemctl status ssh
```

Expected:

```
Active: active (running)
```

### **Step 3 — Find the server IP address**

```
ip addr
```

Look for an IP address associated with your network interface.

For example:

```
192.168.1.100
```

Do not use `127.0.0.1` for a remote connection. That is the loopback address.

### **Step 4 — Connect from another machine**

On your client machine:

```
ssh linuxuser@192.168.1.100
```

Replace the username and IP address with your own.

If the server is on a different network, its routing and firewall must also allow SSH access.

### **Step 5 — Disconnect**

```
exit
```

Official reference: [Ubuntu SSH Guide](https://help.ubuntu.com/community/SSH?utm_source=chatgpt.com).&#x20;

[image](https://www.google.com/s2/favicons?domain=https://help.ubuntu.com\&sz=32)

Community Help Wiki



# **9. Module 1 — Hands-on Practice**

Complete these exercises before moving to Module 2.

**Module 1 Checklist**

0/9 completed

Explain the difference between Linux kernel and Linux distribution.

Name 5 Linux distributions and their package managers.

Draw Linux architecture: User → Application → Shell/Libraries → Kernel → Hardware.

Explain the difference between CLI and GUI.

Install Ubuntu using WSL2 or VirtualBox.

Run uname -a and cat /etc/os-release.

Explain what happens when you execute cat hello.txt.

Explain what SSH is and why it is useful.

Connect to a Linux machine using SSH, if you have access to one.

## **10. Knowledge Check**

Try answering these questions without looking at the material.

**1. Which component manages CPU scheduling and memory?**

Shell

Kernel

Terminal

Application

**2. Ubuntu is:**

A Linux kernel

A Linux distribution

A shell

A package

**3. Which command displays the Linux kernel version?**

whoami

pwd

uname -r

ls

**4. Which protocol is commonly used to connect securely to a remote Linux server?**

FTP

SSH

HTTP

DNS

**5. Which command installs packages on Ubuntu?**

dnf

apk

apt

rpm

Check Answers

## **Official Learning References**

These are useful resources to keep bookmarked.

- [Linux Kernel Official Website](https://www.kernel.org/?utm_source=chatgpt.com)
- [Ubuntu Tutorials](https://ubuntu.com/tutorials?utm_source=chatgpt.com)
- [Ubuntu Server Documentation](https://ubuntu.com/server/docs?utm_source=chatgpt.com)
- [VirtualBox User Manual](https://www.virtualbox.org/manual/?utm_source=chatgpt.com)
- [Microsoft WSL Documentation](https://learn.microsoft.com/en-us/windows/wsl/?utm_source=chatgpt.com)

**Next module:** Linux Command Line Essentials — we'll learn commands with real terminal examples, exercises, and mini challenges.
