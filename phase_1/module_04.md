# **Module 4: Linux File Permissions and Ownership**

Linux permissions control **who can read, modify, execute, or access a file or directory**.

This is one of the most important Linux topics for developers and DevOps engineers. Incorrect permissions can cause application failures, expose secrets, or prevent Docker containers and services from starting.

## **1. Linux Users and Groups**

Linux is a multi-user operating system. Every process runs with a user identity, and file access is checked against that identity.

### **Users**

Common user types:

| User type    | Description                                     |
| ------------ | ----------------------------------------------- |
| Root         | Superuser with broad system privileges; UID `0` |
| Regular user | Normal account used for daily work              |
| System user  | Account used by services and applications       |

Check your current user:

```
whoami
id
```

Example:

```
uid=1000(alex) gid=1000(alex) groups=1000(alex),27(sudo)
```

This means:

- `uid=1000`: User ID
- `gid=1000`: Primary group ID
- `groups`: All groups the user belongs to

View users and groups:

```
cat /etc/passwd
cat /etc/group
```

> `/etc/passwd` contains account information, not normally the actual password hashes. Password hashes are stored in `/etc/shadow`, which is restricted.

### **Groups**

Groups make it easier to share access between users.

For example, a team working on an application might share a group called `developers`.

```
groups
```

Create a group, if you have administrator privileges:

```
sudo groupadd developers
```

Add a user to a supplementary group:

```
sudo usermod -aG developers alex
```

The `-aG` combination means append the user to the specified supplementary group. Without `-a`, `-G` can replace the user's supplementary group list.

Check:

```
id alex
```

A new group membership may require logging out and back in, or starting a new session.

## **2. Understanding Read, Write, and Execute**

Linux permissions are divided into three categories:

- **User (**`u`**)** — file owner
- **Group (**`g`**)** — users belonging to the file's group
- **Other (**`o`**)** — everyone else

Each category can have:

| Permission | Symbol | Numeric value | Meaning for a file               |
| ---------- | ------ | ------------- | -------------------------------- |
| Read       | `r`    | 4             | View file contents               |
| Write      | `w`    | 2             | Modify file contents             |
| Execute    | `x`    | 1             | Run the file as a program/script |

### **Example permission display**

```
ls -l
```

Output:

```
-rwxr-xr--
```

Breakdown:

`- rwx r-x r--`

**Owner**

## **rwx**

Read, write, execute

**Group**

## **r-x**

Read, execute

**Other**

## **r--**

Read only

The first character `-` means this is a regular file. A `d` would indicate a directory.

### **Permissions on directories**

Directory permissions behave differently from file permissions:

| Permission | Meaning on a directory                                     |
| ---------- | ---------------------------------------------------------- |
| `r`        | List directory entries/names                               |
| `w`        | Create, delete, or rename entries, subject to other checks |
| `x`        | Traverse the directory; access entries by name             |

A user generally needs `x` on a directory to access files inside it. Having `r` without `x` lets them see names, but not necessarily access those files.

**Important:** Deleting a file is mainly controlled by permissions on its parent directory, not by the file's own write permission.

## **3.&#x20;**`chmod`**&#x20;— Change File Permissions**

`chmod` changes the permission bits of a file or directory.

### **A. Numeric permissions**

Each permission is represented by a number:

- Read = `4`
- Write = `2`
- Execute = `1`

Add them together for each category.

| Permission | Calculation | Numeric value |
| ---------- | ----------- | ------------- |
| `rwx`      | 4 + 2 + 1   | 7             |
| `rw-`      | 4 + 2       | 6             |
| `r-x`      | 4 + 1       | 5             |
| `r--`      | 4           | 4             |
| `-wx`      | 2 + 1       | 3             |
| `--x`      | 1           | 1             |

Three digits represent **owner, group, other**.

## **Example:&#x20;**`chmod 754 app.sh`

# **7**

**Owner**

rwx

# **5**

**Group**

r-x

# **4**

**Other**

r--

Common permission patterns:

| Permission | Meaning                                  | Common use                      |
| ---------- | ---------------------------------------- | ------------------------------- |
| `755`      | Owner full; group/other read and execute | Directories, executable scripts |
| `644`      | Owner read/write; others read            | Ordinary files, configuration   |
| `700`      | Owner only                               | Private directories/scripts     |
| `600`      | Owner read/write only                    | Private files, keys             |
| `750`      | Owner full; group read/execute           | Team-owned applications         |
| `770`      | Owner and group full access              | Shared team directories         |

Examples:

```
chmod 755 app.sh
chmod 644 config.txt
chmod 700 private_directory
chmod 600 secret.txt
```

Verify:

```
ls -l
```

### **B. Symbolic permissions**

You can also specify permissions using letters.

```
chmod u+x app.sh
```

Add execute permission for the owner.

```
chmod g-w file.txt
```

Remove write permission from the group.

```
chmod o-r file.txt
```

Remove read permission from others.

```
chmod a+r file.txt
```

Add read permission for all categories.

Symbols:

| Symbol | Meaning           |
| ------ | ----------------- |
| `u`    | User/owner        |
| `g`    | Group             |
| `o`    | Other             |
| `a`    | All               |
| `+`    | Add permission    |
| `-`    | Remove permission |
| `=`    | Set exactly       |

Examples:

```
chmod u=rw,g=r,o= file.txt
chmod a-x script.sh
chmod u+rwx,g+rx,o-rwx app.sh
```

### **C. Recursive permissions**

```
chmod -R 755 my_directory
```

This applies permissions recursively to all contents.

**Caution:** Avoid blindly applying one permission mode recursively. Files usually need different permissions from directories. For example, applying `755` to everything can make private files readable by others or mark files executable unnecessarily.

A common pattern:

```
find my_directory -type d -exec chmod 755 {} \;
find my_directory -type f -exec chmod 644 {} \;
```

Use this only when those permissions are appropriate for the contents.

## **4.&#x20;**`chown`**&#x20;and&#x20;**`chgrp`**&#x20;— Change Ownership**

Permissions are evaluated using the file's owner and group.

Check ownership:

```
ls -l
```

Example:

```
-rw-r--r-- 1 alex developers 1200 app.py
```

Here:

- Owner: `alex`
- Group: `developers`

### `chown`**&#x20;— Change owner**

```
sudo chown alex app.py
```

Change both owner and group:

```
sudo chown alex:developers app.py
```

Recursive ownership change:

```
sudo chown -R alex:developers project/
```

### `chgrp`**&#x20;— Change group**

```
sudo chgrp developers app.py
```

Recursive:

```
sudo chgrp -R developers project/
```

### **Practical example**

Suppose the application directory belongs to root, but the application should be managed by user `appuser`.

```
sudo chown -R appuser:appuser /opt/myapp
```

Check the result:

```
ls -ld /opt/myapp
ls -l /opt/myapp
```

**Security note:** Do not change ownership of system directories such as `/etc`, `/usr`, or `/var` without understanding the consequences.

## **5.&#x20;**`umask`**&#x20;— Default Permissions for New Files**

`umask` controls which permissions are removed from the default permissions of newly created files and directories.

Typical base permissions:

- Files: `666` — read/write for everyone, no execute
- Directories: `777` — read/write/execute for everyone

The mask removes selected permission bits.

### **Example:&#x20;**`umask 022`

```
umask
```

Typical output:

```
0022
```

With `umask 022`:

| Item      | Base | Mask | Result |
| --------- | ---- | ---- | ------ |
| File      | 666  | 022  | 644    |
| Directory | 777  | 022  | 755    |

This means:

- New files: owner can read/write; group and others can read.
- New directories: owner has full access; group and others have read/traverse access.

### **Try it**

```
mkdir umask_lab
cd umask_lab

umask 022
touch file_022
mkdir dir_022

umask 077
touch file_077
mkdir dir_077

ls -l
```

Expected permissions (subject to filesystem and application behavior):

```
-rw-r--r-- file_022
drwxr-xr-x dir_022
-rw------- file_077
drwx------ dir_077
```

Restore your shell's typical mask if needed:

```
umask 022
```

> The effective permissions can also be influenced by default ACLs. The mask is not simply ordinary decimal subtraction; permission bits are filtered out.

## **6. SUID, SGID, and Sticky Bit**

These are special permission bits that modify the normal permission behavior.

### **A. SUID — Set User ID**

On an executable file, SUID can make the program run with the effective user ID of the file owner.

Numeric prefix: `4`

```
chmod 4755 program
```

Symbolic:

```
chmod u+s program
```

A permission listing may show:

```
-rwsr-xr-x
```

If the owner's execute bit is not set, the `s` may appear as uppercase `S`.

SUID is commonly used by carefully designed system programs. It can be dangerous if applied to untrusted executables.

### **B. SGID — Set Group ID**

Numeric prefix: `2`

On an executable, SGID can make the process use the file's group as its effective group.

On a directory, SGID causes newly created files and directories to inherit the directory's group.

```
chmod 2775 shared_project
```

Symbolic:

```
chmod g+s shared_project
```

Example:

```
sudo chgrp developers shared_project
sudo chmod 2775 shared_project
```

Files created inside that directory generally inherit the `developers` group.

### **C. Sticky Bit**

Numeric prefix: `1`

The sticky bit on a directory restricts deleting or renaming entries to the file owner, directory owner, or a privileged user.

```
chmod 1777 shared_temp
```

Symbolic:

```
chmod +t shared_temp
```

A typical system temporary directory:

```
ls -ld /tmp
```

Often displays:

```
drwxrwxrwt
```

The final `t` indicates the sticky bit.

### **Special permission summary**

| Special bit | Numeric prefix | Main purpose                                                            |
| ----------- | -------------- | ----------------------------------------------------------------------- |
| SUID        | `4`            | Executable uses file owner's effective UID                              |
| SGID        | `2`            | Executable uses file group's effective GID; directory group inheritance |
| Sticky      | `1`            | Restricts deleting/renaming other users' entries in a shared directory  |

The special bit is added before the ordinary three permission digits:

```
chmod 4755 file
chmod 2775 directory
chmod 1777 directory
```

## **7. ACLs — Access Control Lists**

Traditional Linux permissions support three classes:

- Owner
- Group
- Other

But what if you need to give one specific user access without changing the file's group or making it accessible to everyone?

That is where ACLs help.

**ACL = Access Control List**

ACLs provide more detailed permissions for individual users and groups.

### **Example**

Suppose:

- File owner is `alex`
- Group is `developers`
- You want user `sam` to read and write the file

Using traditional permissions alone, you might need to change the group or grant access to others. ACLs can assign access directly to `sam`.

### **Check ACL**

```
getfacl report.txt
```

Example output:

```
# file: report.txt
# owner: alex
# group: developers
user::rw-
group::r--
other::---
```

### **Grant permissions to a user**

```
setfacl -m u:sam:rw report.txt
```

Grant read-only access:

```
setfacl -m u:sam:r report.txt
```

Grant permissions to a group:

```
setfacl -m g:qa:r report.txt
```

Remove an ACL entry:

```
setfacl -x u:sam report.txt
```

Remove all extended ACL entries:

```
setfacl -b report.txt
```

### **ACL mask**

ACLs include a **mask** that limits the effective permissions of named users, named groups, and the owning group entry.

For example:

```
setfacl -m m:r report.txt
```

This sets the ACL mask to read-only. A named user ACL may show `rw-` but have effective permissions limited to `r--`.

Check the effective permissions:

```
getfacl report.txt
```

Look for lines like:

```
user:sam:rw-                 #effective:r--
```

### **Default ACL on a directory**

A default ACL can be inherited by newly created items inside a directory.

```
setfacl -m d:u:sam:rwX shared_project
```

`X` means execute/search permission is added only when appropriate, such as for directories or files that already have an execute bit.

**Note:** `getfacl` and `setfacl` may need to be installed. ACL support also depends on the filesystem and mount configuration.

## **8.&#x20;**`sudo`**&#x20;and Privilege Escalation**

Linux restricts certain operations to privileged users. `sudo` allows an authorized user to run a command with elevated privileges.

### **Run one command as root**

```
sudo apt update
```

### **Check your sudo permissions**

```
sudo -l
```

### **Open a root shell**

```
sudo -i
```

Exit the root shell:

```
exit
```

Prefer `sudo command` over staying in a root shell for routine work. This reduces the chance of accidentally changing system files.

### **Why not use&#x20;**`sudo`**&#x20;for everything?**

Avoid commands like:

```
sudo chmod -R 777 /
```

This is dangerous and can break the system or expose sensitive files.

Use the least privilege necessary:

- Run normal application commands as a regular user.
- Use `sudo` only for administrative tasks.
- Avoid giving applications unnecessary root access.
- Never make private keys or secrets readable by all users.

### **Safely editing sudo configuration**

Do not edit `/etc/sudoers` directly with a normal text editor.

Use:

```
sudo visudo
```

`visudo` validates the configuration before saving, helping prevent syntax errors that could lock you out of sudo.

## **9. Hands-on Lab: Linux Permissions**

This lab uses a directory in your home folder. You do not need root privileges for most exercises.

### **Lab 1 — Create files**

```
mkdir -p ~/linux-permissions-lab
cd ~/linux-permissions-lab

touch app.py
touch secret.txt
mkdir shared
```

Inspect:

```
ls -l
```

### **Lab 2 — Change permissions**

```
chmod 644 app.py
chmod 600 secret.txt
chmod 755 shared
```

Verify:

```
ls -l
```

Think about:

- Who can read `app.py`?
- Who can modify `secret.txt`?
- Can other users traverse `shared`?

### **Lab 3 — Test execute permission**

Create a simple script:

```
cat > hello.sh <<'EOF'
#!/bin/bash
echo "Hello Linux"
EOF
```

Try running it:

```
./hello.sh
```

If execution is denied:

```
chmod u+x hello.sh
./hello.sh
```

The script now has execute permission for its owner.

### **Lab 4 — Practice symbolic permissions**

```
chmod g-r app.py
chmod o-r app.py
chmod u+x hello.sh
```

Inspect the result:

```
ls -l
```

### **Lab 5 — Test&#x20;**`umask`

```
umask 077
touch private_file
mkdir private_dir

ls -l
```

Compare with:

```
umask 022
touch public_file
mkdir public_dir

ls -l
```

### **Lab 6 — Explore ACLs**

If ACL commands are installed:

```
getfacl app.py
```

If you have a second test user available:

```
setfacl -m u:anotheruser:r app.py
getfacl app.py
```

Replace `anotheruser` with a valid local account. Do not use real sensitive files for this experiment.

### **Lab 7 — Ownership exploration**

```
ls -ln
```

This displays numeric UID and GID values.

Try:

```
id
```

If you have a test file and admin access, you can experiment with:

```
sudo chown root:root testfile
ls -l testfile
```

Use only a lab file. Then restore ownership if needed.

## **10. Common Permission Problems in Real Projects**

| Symptom                                  | Possible cause                                               | Useful check                  |
| ---------------------------------------- | ------------------------------------------------------------ | ----------------------------- |
| `Permission denied` while running script | Missing execute permission or directory traversal permission | `ls -l`, `namei -l path`      |
| Application cannot read config           | Wrong owner or read permission                               | `ls -l config`                |
| Application cannot write logs            | Log directory ownership or write permission                  | `ls -ld /var/log/myapp`       |
| SSH rejects private key                  | Key permissions too open                                     | `chmod 600 ~/.ssh/id_ed25519` |
| Container cannot access mounted files    | Host/container UID/GID mismatch or mount permissions         | `id`, `ls -ln`                |
| Team members cannot edit shared files    | Incorrect group ownership or directory permissions           | `ls -ld`, `id`                |

### **A practical debugging sequence**

When you see `Permission denied`, do not immediately use `chmod 777`.

```
whoami
id
ls -l target_file
ls -ld parent_directory
namei -l /path/to/target_file
```

Then determine:

1. Which user is running the process?
2. Who owns the file?
3. Which group owns it?
4. Which permission is missing?
5. Does the process have traversal permission on every parent directory?
6. Is an ACL or filesystem mount affecting access?

## **11. Quick Command Cheat Sheet**

| Command                     | Purpose                              |
| --------------------------- | ------------------------------------ |
| `whoami`                    | Show current username                |
| `id`                        | Show UID, GID, and groups            |
| `groups`                    | Show group membership                |
| `ls -l`                     | Show file permissions and ownership  |
| `chmod 755 file`            | Set numeric permissions              |
| `chmod u+x file`            | Add execute permission to owner      |
| `chown user file`           | Change file owner                    |
| `chown user:group file`     | Change owner and group               |
| `chgrp group file`          | Change group                         |
| `umask`                     | Display permission mask              |
| `getfacl file`              | View ACL                             |
| `setfacl -m u:user:rw file` | Add user ACL                         |
| `sudo -l`                   | Show sudo permissions                |
| `sudo command`              | Run command with elevated privileges |
| `visudo`                    | Safely edit sudo configuration       |

## **12. Knowledge Check**

**1. What does \`chmod 640 file.txt\` mean?**

Owner rw-, group r--, others ---

Owner rwx, group r--, others ---

Owner r--, group rw-, others ---

**2. Which permission is required to traverse a directory?**

Read

Write

Execute

**3. What does \`chmod 600 secret.txt\` provide?**

Everyone can read it

Owner can read/write; group and others have no access

Owner can execute it

**4. What is the usual effect of \`umask 077\` on new files?**

Usually owner-only read/write

Everyone can read/write

Owner and group can read

**5. What does SGID on a shared directory commonly do?**

Makes every file executable

Makes new items inherit the directory's group

Prevents file deletion

**6. What is the purpose of the sticky bit on \`/tmp\`?**

Only root can read files

Users cannot delete or rename other users' entries in the shared directory

All files become executable

**7. Which command grants user \`sam\` read access through an ACL?**

\`setfacl -m u:sam:r file\`

\`chmod sam+r file\`

\`chgrp sam file\`

**8. Why should you avoid \`chmod -R 777\`?**

It removes all permissions

It can expose files and make them writable by everyone

It changes the file owner

Check answers

## **13. Module 4 Completion Checklist**

I can explain Linux users, groups, UID, and GID.

I understand file and directory rwx permissions.

I can calculate and use 755, 644, 700, and 600.

I can use chmod in numeric and symbolic modes.

I can change file ownership using chown and chgrp.

I understand how umask affects new files and directories.

I understand SUID, SGID, and the sticky bit.

I can use basic ACL commands.

I understand safe sudo usage and least privilege.

I can troubleshoot Permission denied errors.

0 of 10 completed

### **Key takeaway**

Linux permissions are not just about `chmod`. Access depends on **the process identity, file owner/group, permission bits, directory traversal, ACLs, and sometimes special bits or filesystem configuration**.

Next module: **Module 5 — Linux Process Management**: `ps`, `top`, `htop`, `kill`, signals, jobs, `systemd`, services, and process troubleshooting.
