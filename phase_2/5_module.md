# **Phase 2 — Linux Administration**

## **Module 5: Users and Groups**



Learning material

Beginner → Intermediate



**Objective:** Learn how Linux manages user accounts, groups, passwords, login sessions, administrative privileges, and SSH authentication.

This module is especially important for server administration, Docker, Kubernetes, CI/CD pipelines, and production deployments.

By the end, you should be able to create Linux users, organize them into groups, manage account access, troubleshoot login issues, and configure SSH key-based authentication.

## **1. Understanding Linux User Management**

Linux is a multi-user operating system. Multiple users can access the same machine, but each user can have different permissions and privileges.

For example, imagine a Linux server running a Flask application:

| User         | Responsibility                   |
| ------------ | -------------------------------- |
| `root`       | System administration            |
| `devops`     | Server and deployment management |
| `developer`  | Application development          |
| `appuser`    | Runs the Flask application       |
| `monitoring` | Runs monitoring services         |

Why create separate users?

- Security: applications don't need full root access.
- Isolation: users have separate permissions.
- Accountability: actions can be associated with individual accounts.
- Access control: groups make shared access easier.

### **Important Linux user concepts**

| Term                 | Meaning                               |
| -------------------- | ------------------------------------- |
| Username             | Human-readable account name           |
| UID                  | Numeric User ID                       |
| GID                  | Numeric Group ID                      |
| Primary group        | Main group associated with a user     |
| Supplementary groups | Additional groups the user belongs to |
| Home directory       | User's personal working directory     |
| Login shell          | Shell started when the user logs in   |

Check your current identity:

```
whoami
id
id -u
id -g
groups
```

Example:

```
uid=1000(alex) gid=1000(alex) groups=1000(alex),27(sudo)
```

This indicates:

- Username: `alex`
- UID: `1000`
- Primary GID: `1000`
- Supplementary group: `sudo`

## **2. Linux Account Configuration Files**

Linux stores user and group account information in important configuration files.

## **Linux account database**

**/etc/passwd**

User account information

**/etc/shadow**

Password hashes and password aging information

**/etc/group**

Group information and membership

**/etc/gshadow**

Secure group administration information

### **2.1&#x20;**`/etc/passwd`

Contains user account information.

View:

```
cat /etc/passwd
```

Example:

```
alex:x:1000:1000:Alex User:/home/alex:/bin/bash
```

Each field is separated by a colon.

| Field | Value        | Meaning                                |
| ----- | ------------ | -------------------------------------- |
| 1     | `alex`       | Username                               |
| 2     | `x`          | Password information stored separately |
| 3     | `1000`       | UID                                    |
| 4     | `1000`       | Primary GID                            |
| 5     | `Alex User`  | User description / GECOS               |
| 6     | `/home/alex` | Home directory                         |
| 7     | `/bin/bash`  | Login shell                            |

Important:

- `root` normally has UID `0`.
- Regular users on many distributions start at UID `1000`.
- System account UID ranges depend on the distribution.
- The `x` indicates that password hashes are stored in `/etc/shadow`.

Search for a user:

```
grep '^alex:' /etc/passwd
```

### **2.2&#x20;**`/etc/shadow`

Stores password hashes and password aging information.

View (requires privileges):

```
sudo cat /etc/shadow
```

Example structure:

```
alex:$y$j9T$example_hash:20000:0:99999:7:::
```

Fields include:

| Field       | Meaning                                                    |
| ----------- | ---------------------------------------------------------- |
| Username    | Account name                                               |
| Password    | Password hash or account status marker                     |
| Last change | Date of last password change, in days since the Unix epoch |
| Minimum     | Minimum days before password change                        |
| Maximum     | Maximum password age                                       |
| Warning     | Days before expiry to warn                                 |
| Inactive    | Days after expiry before account is disabled               |
| Expire      | Account expiration date                                    |
| Reserved    | Reserved field                                             |

Important password markers:

- `!` or `*` can indicate that password-based login is locked or unavailable.
- A locked password does not necessarily disable every authentication method, such as SSH key authentication.
- Do not share or publish `/etc/shadow` contents.

### **2.3&#x20;**`/etc/group`

Stores group information.

```
cat /etc/group
```

Example:

```
developers:x:1002:alex,sam
```

Fields:

| Field        | Meaning                     |
| ------------ | --------------------------- |
| `developers` | Group name                  |
| `x`          | Group password placeholder  |
| `1002`       | GID                         |
| `alex,sam`   | Supplementary group members |

Search:

```
grep '^developers:' /etc/group
```

### **2.4&#x20;**`/etc/gshadow`

Contains protected group administration information, including group administrators and membership details.

```
sudo cat /etc/gshadow
```

It is normally managed through group administration commands rather than edited manually.

**Remember these four files:**

| File           | Purpose                        |
| -------------- | ------------------------------ |
| `/etc/passwd`  | User accounts                  |
| `/etc/shadow`  | Password hashes and aging      |
| `/etc/group`   | Group membership               |
| `/etc/gshadow` | Protected group administration |

## **3. Creating Users —&#x20;**`useradd`

`useradd` creates a new Linux user account.

Basic syntax:

```
sudo useradd username
```

For a regular human user, you generally want a home directory and a login shell.

```
sudo useradd -m -s /bin/bash developer
```

Explanation:

| Option | Purpose                      |
| ------ | ---------------------------- |
| `-m`   | Create home directory        |
| `-s`   | Set login shell              |
| `-d`   | Specify home directory       |
| `-u`   | Specify UID                  |
| `-g`   | Specify primary group        |
| `-G`   | Specify supplementary groups |
| `-c`   | Add user description         |

Example:

```
sudo useradd -m \
  -s /bin/bash \
  -c "Application Developer" \
  developer
```

Set a password:

```
sudo passwd developer
```

Verify:

```
id developer
getent passwd developer
ls -ld /home/developer
```

### **Create a service account**

Applications should usually run under a dedicated, unprivileged service account.

Example:

```
sudo useradd -r -s /usr/sbin/nologin appuser
```

- `-r`: Create a system account.
- `-s`: Specify the login shell.
- `nologin`: Prevent normal interactive login through that shell.

The location of `nologin` can vary by distribution. Check it with:

```
which nologin
```

A service account can still run an application when a service manager is configured to use it; interactive login is a separate concern.

## **4. Modifying Users —&#x20;**`usermod`

`usermod` modifies an existing user account.

### **4.1 Add user to a supplementary group**

```
sudo usermod -aG developers alex
```

- `-a`: Append.
- `-G`: Specify supplementary groups.

**Important:** Always remember `-aG` when adding supplementary groups. Using `-G` without `-a` replaces the existing supplementary group list.

Verify:

```
id alex
groups alex
```

### **4.2 Change the login shell**

```
sudo usermod -s /bin/bash alex
```

### **4.3 Change the home directory**

```
sudo usermod -d /home/newalex -m alex
```

- `-d`: New home directory.
- `-m`: Move existing home contents to the new location.

### **4.4 Lock and unlock an account password**

```
sudo usermod -L alex
```

Unlock:

```
sudo usermod -U alex
```

These commands lock or unlock password authentication. They do not necessarily disable SSH key authentication or all other login methods.

### **4.5 Change username**

```
sudo usermod -l newalex alex
```

This changes the login name. It does not automatically rename the home directory.

### **4.6 Change primary group**

```
sudo usermod -g developers alex
```

Verify:

```
id alex
```

## **5. Deleting Users —&#x20;**`userdel`

Delete a user account:

```
sudo userdel developer
```

Delete the account and its home directory:

```
sudo userdel -r developer
```

Difference:

| Command           | Result                                                                                |
| ----------------- | ------------------------------------------------------------------------------------- |
| `userdel user`    | Removes the account but leaves the home directory                                     |
| `userdel -r user` | Removes the account and its home directory and associated mail spool where applicable |

**Caution:** Before deleting a user, check whether the account owns application files, scheduled jobs, running processes, or service configurations.

Find files owned by a user:

```
sudo find / -user developer -print 2>/dev/null
```

Do not run account deletion commands against production users without an ownership and recovery plan.

## **6. Linux Group Management**

Groups are used to manage access for multiple users.

Imagine a development team:

```
developers
    |
    |-- alex
    |-- sam
    |-- john
```

Instead of granting permissions individually, you can grant access to the `developers` group.

### **6.1 Create a group —&#x20;**`groupadd`

```
sudo groupadd developers
```

Verify:

```
getent group developers
```

### **6.2 Delete a group —&#x20;**`groupdel`

```
sudo groupdel developers
```

A group cannot normally be deleted while it is the primary group of an existing user.

### **6.3 Add users to groups**

```
sudo usermod -aG developers alex
sudo usermod -aG developers sam
```

### **6.4&#x20;**`gpasswd`

`gpasswd` manages group membership and group administration.

Add a user:

```
sudo gpasswd -a alex developers
```

Remove a user:

```
sudo gpasswd -d alex developers
```

Set group administrators:

```
sudo gpasswd -A alex developers
```

Check membership:

```
getent group developers
```

### **6.5 Primary vs supplementary groups**

| Type                | Explanation                                                                            |
| ------------------- | -------------------------------------------------------------------------------------- |
| Primary group       | Main group associated with the user; used as the default group for newly created files |
| Supplementary group | Additional groups that provide access to shared resources                              |

Example:

```
id alex
```

Output:

```
uid=1000(alex) gid=1000(alex) groups=1000(alex),1002(developers),27(sudo)
```

Here, the primary group is the group with GID `1000`; `developers` and `sudo` are supplementary groups.

## **7. Password Management**

Linux provides the `passwd` command to manage user passwords.

### **Change your own password**

```
passwd
```

### **Change another user's password**

```
sudo passwd developer
```

### **Lock password authentication**

```
sudo passwd -l developer
```

### **Unlock password authentication**

```
sudo passwd -u developer
```

### **Check password/account status**

```
sudo passwd -S developer
```

### **Password expiry management —&#x20;**`chage`

`chage` manages password aging and account expiry policies.

Display current password aging details:

```
sudo chage -l developer
```

Set maximum password age to 90 days:

```
sudo chage -M 90 developer
```

Set minimum password age to 1 day:

```
sudo chage -m 1 developer
```

Warn the user 7 days before password expiry:

```
sudo chage -W 7 developer
```

Require a password change at next login:

```
sudo chage -d 0 developer
```

Set account expiration date:

```
sudo chage -E 2027-12-31 developer
```

Summary:

| Command      | Purpose                             |
| ------------ | ----------------------------------- |
| `passwd`     | Change password                     |
| `passwd -l`  | Lock password                       |
| `passwd -u`  | Unlock password                     |
| `passwd -S`  | Display password status             |
| `chage -l`   | Show password aging                 |
| `chage -M`   | Set maximum password age            |
| `chage -m`   | Set minimum password age            |
| `chage -W`   | Set warning period                  |
| `chage -d 0` | Force password change at next login |

**Security recommendation:** Use strong unique passwords, SSH keys for server administration where practical, and centrally managed identity policies where available. Password aging policies should follow your organization's security requirements rather than being applied blindly.

## **8. User Login and Logout**

Linux supports multiple ways to access a system.

### **8.1 Check current logged-in users**

```
who
```

Shows logged-in sessions.

```
w
```

Shows logged-in users and their current activity.

```
whoami
```

Shows the current effective username.

### **8.2 Switch to another user —&#x20;**`su`

```
su developer
```

This starts a shell as another user, normally asking for that user's password.

Switch to another user with a login environment:

```
su - developer
```

The `-` loads the target user's login environment, including their home directory and login shell configuration.

Return to the previous shell:

```
exit
```

### **8.3 Run one command as another user**

```
sudo -u appuser whoami
```

Useful for testing whether an application user can access files or execute a command.

### **8.4 Logout**

```
exit
```

Or use:

```
logout
```

`logout` is generally used for login shells. `exit` works for ending the current shell.

### **8.5 Check login history**

```
last
```

Shows historical login sessions using system login records.

```
lastlog
```

Shows the most recent login for users, where supported.

```
lastb
```

Shows recorded failed login attempts, where the system maintains those records and you have sufficient privileges.

## **9. Root User vs Normal User**

Linux has a special superuser called `root`.

The root account has UID `0` and has broad privileges over the operating system.

| Feature               | Root user            | Normal user                    |
| --------------------- | -------------------- | ------------------------------ |
| UID                   | 0                    | Usually non-zero               |
| System configuration  | Broad access         | Requires authorization         |
| Install packages      | Directly             | Usually requires sudo          |
| Modify system files   | Broad access         | Restricted                     |
| Application execution | Can run applications | Can run permitted applications |
| Security risk         | High if misused      | Lower by default               |

### **9.1 Check whether you are root**

```
whoami
id -u
```

If the output of `id -u` is `0`, your current effective UID is root.

### **9.2 Run a command with root privileges**

```
sudo apt update
```

### **9.3 Open a root login shell**

```
sudo -i
```

Exit:

```
exit
```

### **9.4 Root vs sudo**

- `root` is an account with UID 0.
- `sudo` is a mechanism that allows authorized users to execute commands with elevated privileges.
- `sudo` permissions are controlled by sudo policy, usually configured in `/etc/sudoers` and included files under `/etc/sudoers.d/`.

Check your allowed sudo operations:

```
sudo -l
```

**Production best practice:** Use a normal user for daily work. Use `sudo` only for the administrative commands that require it. Avoid enabling direct root SSH login unless there is a specific, justified operational need.

## **10. SSH Key-Based Authentication**

Important DevOps skill

SSH (Secure Shell) allows you to connect securely to a remote Linux server.

Example:

You have a Linux server hosted on Azure, AWS, or another cloud provider. You want to connect from your laptop without entering your account password every time.

SSH key-based authentication is a common solution.

### **10.1 How SSH authentication works**

SSH uses a key pair:

- **Private key:** Stays on your laptop. Never share it.
- **Public key:** Installed on the remote server.

## **SSH key authentication flow**

**Your laptop**

**Private key + SSH client**

Private key remains here

SSH connection and authentication challenge

**Remote Linux server**

**Public key in authorized_keys**

Verifies proof of private-key possession

**Authentication successful**

The private key proves that you possess the corresponding key. The server uses the public key to verify that proof. The private key is never sent to the server.

### **10.2 Check SSH installation**

On Ubuntu/Debian:

```
ssh -V
```

Check whether the SSH server is installed:

```
sudo systemctl status ssh
```

If it is not installed:

```
sudo apt update
sudo apt install openssh-server
```

Start and enable the service:

```
sudo systemctl enable --now ssh
```

On some distributions, the service name is `sshd` rather than `ssh`.

### **10.3 Generate an SSH key pair**

On your laptop:

```
ssh-keygen -t ed25519 -C "developer-laptop"
```

You will be prompted for a file location and optionally a passphrase.

Typical output files:

```
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

| File             | Purpose     |
| ---------------- | ----------- |
| `id_ed25519`     | Private key |
| `id_ed25519.pub` | Public key  |

You can use a passphrase to protect your private key if the key file is copied or stolen.

Check the generated keys:

```
ls -la ~/.ssh
```

### **10.4 Copy the public key to the server**

If your system supports `ssh-copy-id`:

```
ssh-copy-id developer@192.168.1.100
```

This installs your public key into the remote user's `~/.ssh/authorized_keys` file.

If `ssh-copy-id` is unavailable, you can install the public key manually.

First, display your public key on your laptop:

```
cat ~/.ssh/id_ed25519.pub
```

Copy the public key text.

Log in to the remote server using an existing authorized method, then execute:

```
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Add the copied public key to:

```
nano ~/.ssh/authorized_keys
```

Ensure the key is on one line, then:

```
chmod 600 ~/.ssh/authorized_keys
```

The `.ssh` directory and `authorized_keys` file should be owned by the target user.

Check:

```
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

### **10.5 Connect to the remote server**

From your laptop:

```
ssh developer@192.168.1.100
```

If using a non-default private key:

```
ssh -i ~/.ssh/my_server_key developer@192.168.1.100
```

For a non-default SSH port:

```
ssh -p 2222 developer@192.168.1.100
```

### **10.6 SSH troubleshooting**

Common problems:

| Error                         | Possible reason                                                   |
| ----------------------------- | ----------------------------------------------------------------- |
| Connection refused            | SSH service is not listening or a connection is actively rejected |
| Connection timed out          | Network route, firewall, or security rule blocks access           |
| Permission denied (publickey) | Wrong key, username, or public-key configuration                  |
| Bad permissions               | SSH private key or server-side key files have unsafe permissions  |
| Host key verification failed  | Server host key changed or known_hosts entry does not match       |

Useful debugging:

```
ssh -v developer@192.168.1.100
```

The `-v` option provides verbose connection and authentication information.

Check the server's SSH service:

```
sudo systemctl status ssh
```

Check listening ports:

```
sudo ss -tlnp | grep ssh
```

Check SSH logs on Ubuntu:

```
sudo journalctl -u ssh
```

### **10.7 SSH server security basics**

Once key authentication has been tested successfully, administrators may choose to disable password authentication and direct root login.

SSH server configuration is usually located at:

```
/etc/ssh/sshd_config
```

Common settings to review:

```
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

These are policy examples, not commands. Do not disable password authentication until you have confirmed that key-based access works and you have a recovery method.

After changing SSH server configuration:

```
sudo sshd -t
```

Validate the configuration first.

Then reload the service:

```
sudo systemctl reload ssh
```

Keep your existing SSH session open while testing a second connection. This helps avoid accidentally locking yourself out.

## **11. Practical Lab — Create and Manage Linux Users**

Let's simulate a small development team on your Linux machine.

Hands-on Lab 1

### **Lab objective**

Create:

- `developer1`
- `developer2`
- `appuser`
- `developers` group

Give both developers membership in the developers group.

### **Step 1: Create the group**

```
sudo groupadd developers
```

Verify:

```
getent group developers
```

### **Step 2: Create two developer users**

```
sudo useradd -m -s /bin/bash developer1
sudo useradd -m -s /bin/bash developer2
```

Set passwords:

```
sudo passwd developer1
sudo passwd developer2
```

### **Step 3: Add users to the developers group**

```
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
```

Verify:

```
id developer1
id developer2
```

### **Step 4: Create an application user**

```
sudo useradd -r -s /usr/sbin/nologin appuser
```

Check:

```
getent passwd appuser
```

### **Step 5: Create a shared project directory**

```
sudo mkdir -p /opt/company-project
sudo chown root:developers /opt/company-project
sudo chmod 2770 /opt/company-project
```

Why `2770`?

- `2`: SGID, so new entries inherit the directory's group.
- `7`: Owner has full permissions.
- `7`: Group has full permissions.
- `0`: Others have no permissions.

### **Step 6: Test developer access**

If your environment supports switching users:

```
sudo -iu developer1
```

Create a test file:

```
cd /opt/company-project
touch developer1.txt
ls -l
```

Exit:

```
exit
```

Now test as developer2:

```
sudo -iu developer2
cd /opt/company-project
touch developer2.txt
ls -l
```

Because the directory has SGID, newly created files should inherit the `developers` group. Note that their group write permission depends on the user's umask and any ACL configuration.

### **Step 7: Inspect all accounts**

```
id developer1
id developer2
id appuser

getent group developers
ls -ld /opt/company-project
```

## **12. Practical Lab — SSH Key Authentication**

Hands-on Lab 2

You need two Linux environments or a Linux laptop and a remote Linux server.

For example:

- Machine A: Your laptop
- Machine B: Ubuntu server

### **Step 1: Check SSH client**

On Machine A:

```
ssh -V
```

### **Step 2: Generate keys**

```
ssh-keygen -t ed25519 -C "linux-lab"
```

### **Step 3: Copy the public key**

```
ssh-copy-id developer1@SERVER_IP
```

If unavailable, follow the manual installation procedure in Section 10.

### **Step 4: Connect**

```
ssh developer1@SERVER_IP
```

### **Step 5: Verify your identity**

On the remote server:

```
whoami
id
hostname
```

Expected username:

```
developer1
```

### **Step 6: Disconnect**

```
exit
```

If you have only one Linux machine, you can practise SSH locally using localhost, provided the SSH server is installed and running.

## **13. Real-World DevOps Scenario**

Imagine you are deploying a Flask API on an Ubuntu server.

A poor design is to run everything as root:

```
root
  |
  |-- Flask API
  |-- Deployment scripts
  |-- Application logs
  |-- Configuration files
```

A better design separates responsibilities:

```
Linux Server
    |
    |-- root
    |     System administration
    |
    |-- devops
    |     Deployment and server maintenance
    |
    |-- developers
    |     Shared development access
    |
    |-- appuser
          Runs Flask application
```

Example service configuration concept:

```
[Service]
User=appuser
Group=appuser
WorkingDirectory=/opt/myapp
ExecStart=/opt/myapp/.venv/bin/gunicorn app:app
Restart=always
```

This is a `systemd` service configuration example. The application runs as `appuser`, rather than root.

**Architectural lesson:** A Linux user account is not just a login identity. It is also an important security boundary for running applications and services.

## **14. Common Commands Cheat Sheet**

| Command                  | Purpose                             |
| ------------------------ | ----------------------------------- |
| `whoami`                 | Current username                    |
| `id`                     | UID, GID and groups                 |
| `who`                    | Logged-in users                     |
| `w`                      | Logged-in users and activity        |
| `getent passwd`          | Query user accounts                 |
| `getent group`           | Query groups                        |
| `useradd -m user`        | Create user with home directory     |
| `usermod -aG group user` | Add user to supplementary group     |
| `userdel -r user`        | Delete user and home directory      |
| `groupadd group`         | Create group                        |
| `groupdel group`         | Delete group                        |
| `gpasswd -a user group`  | Add user to group                   |
| `gpasswd -d user group`  | Remove user from group              |
| `passwd user`            | Change user's password              |
| `passwd -l user`         | Lock password                       |
| `passwd -u user`         | Unlock password                     |
| `chage -l user`          | View password aging                 |
| `su - user`              | Switch to login shell of user       |
| `sudo -u user command`   | Run command as another user         |
| `sudo -i`                | Open root login shell               |
| `last`                   | Show login history                  |
| `ssh-keygen`             | Generate SSH key pair               |
| `ssh-copy-id`            | Install public key on remote server |
| `ssh user@host`          | Connect to remote server            |

## **15. Module 5 Completion Checklist**

**Learning progress**

# **0/13**

I understand UID, GID, primary groups and supplementary groups.

I understand /etc/passwd and its fields.

I understand /etc/shadow and password hashes.

I understand /etc/group and /etc/gshadow.

I can create users using useradd.

I can modify users using usermod.

I can safely delete users using userdel.

I can create and manage groups.

I can manage passwords using passwd and chage.

I understand login, logout, su and sudo.

I understand root versus normal users.

I can generate SSH keys and connect to a remote Linux server.

I understand SSH key permissions and basic troubleshooting.

## **16. Knowledge Check — Quick Revision**

Try these questions before moving to the next module.

1. What is the difference between `/etc/passwd` and `/etc/shadow`?
2. What does UID `0` represent?
3. Why should you use `usermod -aG` instead of `usermod -G` when adding a supplementary group?
4. What is the difference between a primary group and a supplementary group?
5. What is the purpose of `useradd -m`?
6. How can you force a user to change their password at the next login?
7. What is the difference between `su - user` and `sudo -u user command`?
8. Why should applications run under dedicated service accounts?
9. What is the difference between an SSH private key and public key?
10. Why should you never share your private SSH key?

## **What you should remember from Module 5**

The five major concepts are:

1. **User database:** `/etc/passwd`, `/etc/shadow`.
2. **Group management:** `/etc/group`, `groupadd`, `gpasswd`.
3. **Account administration:** `useradd`, `usermod`, `userdel`, `passwd`, `chage`.
4. **Privilege management:** Root, normal users, `sudo`.
5. **Remote access:** SSH public/private key authentication.

**Next: Module 6 — Linux Process Management**

- Processes and process IDs (PID)
- `ps`, `top`, `htop`
- `kill`, `pkill`, `killall`
- Linux signals
- Foreground and background processes
- Jobs and `nohup`
- Process priority: `nice`, `renice`
- `systemd` services
- Service troubleshooting and logs

This module will connect Linux administration with the way real applications, Flask APIs, background workers, and services run on Linux servers.
