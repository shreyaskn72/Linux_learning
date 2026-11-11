# Linux Learning Roadmap — From Beginner to Production Engineer

If you want to learn Linux properly, I recommend following a hands-on, lab-based roadmap rather than just memorizing commands.

Since Linux is widely used in DevOps, Docker, Kubernetes, cloud infrastructure, and backend systems, the goal should be to understand both Linux fundamentals and how Linux behaves in production environments.

This roadmap takes you from zero Linux knowledge to an advanced level.

## Complete Table of Contents

Beginner → Intermediate → Advanced

Recommended sequence: 16 modules + hands-on projects

## Phase 1 — Linux Fundamentals

[The Linux command line for beginners - Ubuntu Desktop documentation](https://images.openai.com/static-rsc-4/TWruKNqYQumj6tFWCGD_KsTWJyjrJlhNxj9ugRN_86MnKsTNHpwk_DmgDaFGfFdUMB09yzXjoM7V1EqXSg-gi7b0a0e20tcA7JJMf1HDDOnd_EGOZ2LpaQT2sZOi4_d-5_-9TjPD4fbow_PAhALicZpBjXxKB3Epdblzp6q1G0U?purpose=inline)

[Understanding the Linux Boot Process | jpcairesf/Blog](https://images.openai.com/static-rsc-4/2giEJS2B8Eb2wAv9MZCFtNZf3AaYjS3YrpT0Mn6w_G64fut_9qK9pBOa-Wyb9vKQJW8gmErlg3XZKy4wFxCXgzngsG728b-HgPIK3txhQwtW-ztbQBMQSPx_n9lsYFIh52Zrha0pEDnnpTYR6b3UPc9LgktbGr5kQaaKdpQDuSg?purpose=inline)

[Ubuntu - Command Line](https://images.openai.com/static-rsc-4/FcWXDAlUeV00Oj05IIUUcSYtvRL1Xa5QAnRlmHZUl7uQNKy93lXTO5kIoRlH1M6-cuzYu-YHo78LQQ9QptMffE-g7-51gerCRi2ocx7qFMou2SFX_Lxl4-hsw5NEf3GNiMROdNxD6gAkF97yVmIENGMnkl4Gk4joxl5mOFb215c?purpose=inline)

5

### Module 1: Introduction to Linux

- What is Linux?
- Linux kernel vs Operating System
- Linux distributions — Ubuntu, Debian, RHEL, Fedora, Alpine
- Linux vs Windows vs macOS
- Linux architecture: Kernel, Shell, User Space
- CLI vs GUI
- Installing Linux using VirtualBox, WSL2, or a VM
- SSH into a Linux machine

### Module 2: Linux Command Line Essentials

- Terminal and Shell
- Bash fundamentals
- `pwd`, `ls`, `cd`
- `mkdir`, `touch`, `cp`, `mv`, `rm`
- `cat`, `less`, `more`
- `echo`, `printf`
- `clear`, `history`, `man`
- Command options and arguments
- Absolute vs relative paths
- Tab completion

### Module 3: Linux Filesystem

- Linux filesystem hierarchy
- `/`, `/home`, `/etc`, `/var`, `/tmp`
- `/usr`, `/opt`, `/root`
- `/dev`, `/proc`, `/sys`
- Files vs directories
- File types
- Hard links vs symbolic links
- Inodes
- Mount points
- Disk partitions and filesystems

### Module 4: File Permissions and Ownership

- Linux users and groups
- `chmod`
- `chown`, `chgrp`
- Read, write, execute permissions
- Numeric permissions: 755, 644, 700
- `umask`
- SUID, SGID, Sticky Bit
- ACLs
- `sudo` and privilege escalation

## Phase 2 — Linux Administration

### Module 5: Users and Groups

- `/etc/passwd`
- `/etc/shadow`
- `/etc/group`
- `useradd`, `usermod`, `userdel`
- `groupadd`, `gpasswd`
- Password management
- User login and logout
- Root user vs normal user
- SSH key-based authentication

### Module 6: Package Management

- Package managers
- APT — Ubuntu/Debian
- DNF/YUM — RHEL-based systems
- RPM and DEB packages
- Installing and removing packages
- Repository configuration
- Package dependencies
- Version management
- Installing software from source

### Module 7: Process Management

- What is a Linux process?
- PID and PPID
- Process states
- `ps`, `top`, `htop`
- `pgrep`, `pkill`
- `kill`, `killall`
- Foreground vs background processes
- `jobs`, `bg`, `fg`
- `nohup`
- Signals: SIGTERM, SIGKILL, SIGHUP
- Process priorities: `nice`, `renice`

### Module 8: Services and Systemd

- What is a Linux service?
- systemd architecture
- `systemctl`
- Starting, stopping, enabling services
- Service unit files
- Creating a custom systemd service
- Service dependencies
- Boot process
- Runlevels vs systemd targets
- Troubleshooting failed services

### Module 9: Storage and Disk Management

- Disk, partition, filesystem
- `lsblk`, `blkid`, `fdisk`
- `df`, `du`
- `mount`, `umount`
- `/etc/fstab`
- LVM fundamentals
- Swap space
- Filesystem types: ext4, XFS
- Disk space troubleshooting

## Phase 3 — Linux Networking

[Deep Dive: The ip Command in Linux](https://images.openai.com/static-rsc-4/STXkYgHj4tynSxFc7YKl-w6i2KXZsuzFvYDYKe6A8X0sYgb4IiH5SOfXymWiPYzZ0ZkfA2-nShZrB-jFd2PBkxvy0Cubm59dOpI0jHJonv4SU-fAo4dxmgds1nXUllZ73FvRncUNYfb91DMMBeXgnn_N9bel46mEul20gkXveXM?purpose=inline)

[Linux Networking Stack tutorial: Receiving Data | Maxnilz🌴](https://images.openai.com/static-rsc-4/cKNoKjeRlRSQjEsQvd-l6ELluhAbma5VInEaFlh2UiVd6UjNGK2badyoqkJ-_NIhH2jYHLnWVQQgYQbhdOBo6gusKHPB2QM0tbRegYD50tMb0zAniaiS7eId2hWOByV0MnBYaZpZmBTsKcRheoOM_n-o7405hY-fxc6s3WG-2sw?purpose=inline)

[Les étapes pour la comminication socket (Les protocoles TCP et UDP) | Programmation système (Linux et C)](https://images.openai.com/static-rsc-4/NPXlZTbmGinOJAY8JfoM8unii2TcLyeKNoPOeuoOPbwnqosNyWxGLIkn15qpM6C822SHX-Mvz9MYcaYPZuv0E4KVQZ8YRlsl_Dc0pBLlO-jP3mJt2DseTsTR9szz84oSObr_xoC17lHtG5ohmKtteRgSn-hmeIQucFqdlQxI44U?purpose=inline)

7

### Module 10: Linux Networking Fundamentals

- Network interfaces
- IP address and subnet
- MAC address
- Default gateway
- Routing table
- DNS resolution
- TCP vs UDP
- Ports and sockets
- Network namespaces
- Linux network stack

### Module 11: Networking Commands

- `ip addr`
- `ip route`
- `ip link`
- `ping`
- `traceroute`
- `ss`
- `netstat`
- `curl`, `wget`
- `dig`, `nslookup`
- `tcpdump`
- `nc` (Netcat)
- `iptables`, `nftables`
- Troubleshooting connectivity

## Phase 4 — Shell Scripting and Automation

### Module 12: Bash Scripting

- Shell scripting fundamentals
- Variables and environment variables
- Input and output
- Command substitution
- Conditions: `if`, `else`
- Loops: `for`, `while`
- Functions
- Exit codes
- Positional arguments
- `set -euo pipefail`
- Arrays
- Error handling
- Script debugging
- ShellCheck
- Cron jobs and scheduled tasks

### Module 13: Text Processing and Log Analysis

- `grep`
- `find`
- `awk`
- `sed`
- `sort`, `uniq`
- `cut`, `tr`
- `xargs`
- `head`, `tail`
- `tee`
- Pipes and redirection
- Regular expressions
- Log rotation
- Analyzing application logs

## Phase 5 — Advanced Linux

### Module 14: Linux Performance and Troubleshooting

- CPU utilization
- Memory management
- Virtual memory
- Load average
- OOM Killer
- Disk I/O
- `free`, `vmstat`
- `iostat`, `iotop`
- `uptime`
- `sar`
- `dmesg`
- `journalctl`
- Linux performance bottlenecks
- Troubleshooting methodology

### Module 15: Linux Security

- Linux security model
- SSH hardening
- Firewall configuration
- SELinux fundamentals
- AppArmor
- Linux capabilities
- File integrity
- Audit logs
- Security updates
- Secrets and environment variables
- Principle of least privilege

### Module 16: Linux Internals

- Kernel architecture
- System calls
- Process creation: `fork`, `exec`
- Signals
- Threads
- Scheduling
- Memory allocation
- Virtual memory and paging
- `/proc` filesystem
- `/sys` filesystem
- `strace`
- `lsof`
- `ldd`
- Dynamic libraries
- Kernel modules
- cgroups
- namespaces

## Phase 6 — Practical Linux Labs

Theory alone is not enough. These labs will help you apply the modules.

[How To Learn Linux: A Comprehensive Guide for Beginners](https://images.openai.com/static-rsc-4/tabCgwxVpwzlEVuijuyP3nep0wDS3rd8UihfFcszWpFRu0kI_jTTUuVhUkNB_KwK4j4BGkG-HDvs4EzXqX7MLO_gOq1RtjcbzxSn5fcBgOHZHd6Du5ovNMt3PVOK_P2AYPnGjvY90phjYM1LFtdwxV-yrQym6x2jN9nf4q963GA?purpose=inline)

Lab 1 — Linux Command Line

Create files, directories, navigate the filesystem, and practice 30 essential commands.

[How To Change File or Directory Permissions via the Linux Terminal | Tom's Hardware](https://images.openai.com/static-rsc-4/oDy1_HJDIWWEJaHxVpo4I9MiUnxIY19wLxGw24pzn7-N6I5dh2qJnkQqotLT74J7hxWRRyfN-dQFSIX_p67GetQGKerHHU0Be5cg-Vx5No3H4Pixudp5DR4olwtKReLxpXpKWuWE9ipY2sPElTeTKKTDY9NXZYqcWEMlhUiyKwg?purpose=inline)

Lab 2 — User and Permission Management

Create users, groups, permissions, and secure directories.

[How to list and manage Services in Linux? - AccuWebHosting](https://images.openai.com/static-rsc-4/UXYHUZEouNG2hMJdQuWUZ7LJ71ZNbV9Sl3-Dk1M5kC_h6r_U8-gFN5cexeo8J9NJ8V9opamYkzfsm9JgBWdiO7rBRsExC4MuPtBvVqbj-HgamlWxpsMptON8W0vLsZ37lCxxp3tCu9yDi0GaVgevIxQF8si6WGDHvIbDmxRKsTk?purpose=inline)

Lab 3 — Deploy a Linux Service

Install NGINX and create a custom systemd service.

[What is Bash Shell?. Bash (Bourne-Again Shell) is a command… | by Emin Ugar | Orion Innovation techClub | Medium](https://images.openai.com/static-rsc-4/XQj3ayiRSx3fVJLjXQj3hFw7uSesBHi_j46q70FzF3qCv6zGUdF_WkdhznKxJjV8atW4JaSbJGnuj-sSUni2yGEopMqCfWvyq0HzWxzTXR_TnTaODsHv3SafQdjtKHxq7ZuRqeWxtlCPx_tLU_NYPNlZOM14m0fKHoAWqhpfWHM?purpose=inline)

Lab 4 — Bash Automation

Write scripts for backups, health checks, and log cleanup.

[5 Best Terminal-Based Linux Monitoring Tools](https://images.openai.com/static-rsc-4/zLa4E2Z62S7d9G1Zn_ThiIhN0wKtk4WeK0WwthRC4FRJF1uutcnxi7V0AmR9pSJNdo6450dciChEFIkeXHf59oqXj3qfjstORC1mlQEmuiFXcqTxJXiIoRsj1kMzfrE4w9Z8Nne7ALEq-sOLpyYCNg-sdtXsdzMO44sAomIC0t8?purpose=inline)

Lab 5 — Linux Server Troubleshooting

Diagnose high CPU, memory, disk, and network usage.

[Use Docker to Run Nginx Web Server in Ubuntu! — Part II: Running and Managing Nginx | by Jazz Lien | Medium](https://images.openai.com/static-rsc-4/Z_S_L6QWfCOpV8nkvw0JxcGsbsq-oRpFG5SlU2JTcvULC_IAvQPPTG-XQ1g7-r8uNhJJiZ_nJsanGZngAk9wcTDxsg26627e8BSRZ4SkW9b4frNbdkmarB7dFt9avWBnEV4lYATphpRpaOL9_DTZsJMj5pV8iaYUuwGstKMRh6A?purpose=inline)

Lab 6 — Linux for Docker and Kubernetes

Explore container namespaces, cgroups, networking, and container processes.

## Phase 7 — Linux for DevOps and Cloud-Native Engineering

This is the part I recommend prioritizing after the core fundamentals.

| Topic                     | What to learn                                    |
| ------------------------- | ------------------------------------------------ |
| Docker                    | Images, containers, volumes, networking          |
| Kubernetes                | Pods, nodes, container runtime, Linux namespaces |
| Container internals       | cgroups, capabilities, filesystem isolation      |
| Linux networking          | CNI, veth, bridges, routing, iptables/nftables   |
| Observability             | Logs, metrics, process monitoring                |
| Cloud VMs                 | SSH, systemd, disks, firewall, cloud-init        |
| Production operations     | Incident troubleshooting, service recovery       |
| Infrastructure automation | Bash, Ansible, Terraform Linux provisioning      |

## Recommended Learning Strategy

12-Week Learning Plan

Practical

Learning schedule

Suggested weeks per learning phase

01234BasicsAdminNetworkingScriptingAdvancedFinal lab

- Week 1–2: Linux fundamentals
- Week 3–5: Administration
- Week 6–7: Networking
- Week 8–9: Bash scripting
- Week 10–11: Advanced Linux
- Week 12: Production-style project

### Final Project: Production Linux Server

Build and operate a Linux server that hosts a Flask API.

- Install Ubuntu Server.
- Configure users and SSH.
- Install Python and dependencies.
- Deploy a Flask API using Gunicorn.
- Create a systemd service.
- Configure NGINX as a reverse proxy.
- Configure firewall rules.
- Write Bash health-check scripts.
- Configure log rotation.
- Monitor CPU, memory, disk, and networking.
- Simulate failures and troubleshoot them.

Outcome: You will understand Linux not just as a command-line tool, but as the operating system powering your Docker containers, Kubernetes nodes, and cloud infrastructure.

My recommendation: Start with Module 1 and Module 2 as a Linux Fundamentals Lab, then progress module by module with practical exercises.
