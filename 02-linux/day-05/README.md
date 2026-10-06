# Day 05 · Linux: Users, Package Management and Permissions

📅 **Date:** 06 Oct 2026 · ⏱️ **Time:** ~2 hrs · 🎥 **Source:** CodeWithHarry, Linux tutorial (YouTube)

## 📌 At a glance

| | |
|---|---|
| **Topics** | Users and sudo, package management with apt, file permissions, groups |
| **Key idea** | Linux controls who can do what through users, groups and permissions, and software is managed from the command line |
| **Outcome** | Can create users, install software, read and change permissions, and manage groups |

---

# Part A · Users

## 1. Why users exist

Linux is a **multi-user** system. Several people (or programs) can share one machine, each with their own account, files and level of access.

| Type | Description | Example |
|------|-------------|---------|
| **Root** | Superuser with unrestricted access | `root` |
| **Normal user** | A person's account | `khushi` |
| **System user** | Used by programs and services, no human login | `www-data`, `nobody` |

## 2. User commands

| Command | Purpose |
|---------|---------|
| `whoami` | Show the current user |
| `id` | Show user ID, group ID and groups |
| `sudo adduser <name>` | Create a user (interactive, creates the home folder) |
| `sudo passwd <name>` | Change a user's password |
| `su - <name>` | Switch to another user |
| `sudo deluser <name>` | Delete a user |
| `sudo -i` | Open a root shell (use sparingly) |
| `exit` | Leave the current shell or return to the previous user |

> On Ubuntu use `adduser`. The lower-level `useradd` does not create the home folder or set a password by default.

## 3. What is sudo?

`sudo` ("superuser do") runs **one command with administrator privileges**, after asking for the user's own password. Only members of the `sudo` group can use it.

- Prefer `sudo <command>` over staying logged in as root
- Root can delete or change anything, including system files

> ⚠️ In WSL, root can also change Windows files under `/mnt/c`. Avoid working there.

## 4. The `/etc/passwd` file

Each line describes one user:

```text
khushi:x:1000:1000::/home/khushi:/bin/bash
```

| Field | Meaning |
|-------|---------|
| `khushi` | Username |
| `x` | Password placeholder (the hash is stored in `/etc/shadow`) |
| `1000` | User ID (UID) |
| `1000` | Primary group ID (GID) |
| `/home/khushi` | Home directory |
| `/bin/bash` | Login shell |

Passwords are stored encrypted in `/etc/shadow`, readable only by root.

---

# Part B · Package management (apt)

## 5. What is a package manager?

A package manager installs, updates and removes software from online **repositories**. It works like an app store for the terminal. Ubuntu and Debian use **apt**. Red Hat based systems use **yum** or **dnf**.

## 6. apt commands

| Command | Purpose |
|---------|---------|
| `sudo apt update` | Refresh the list of available packages (installs nothing) |
| `sudo apt upgrade` | Upgrade installed packages to newer versions |
| `sudo apt install <pkg>` | Install a package |
| `sudo apt remove <pkg>` | Remove a package (keeps config files) |
| `sudo apt purge <pkg>` | Remove a package and its config files |
| `sudo apt autoremove` | Remove unused dependencies |
| `apt search <word>` | Search for packages |
| `apt show <pkg>` | Show package details |

> Always run `sudo apt update` before installing. **update** refreshes the list, **upgrade** changes the software.

---

# Part C · Permissions and groups

## 7. Reading permissions

```text
-rwxr-xr--  1  khushi  devteam  120  Oct 6  script.sh
```

| Part | Meaning |
|------|---------|
| `-` | File (`d` means directory) |
| `rwx` | **Owner (u):** read, write, execute |
| `r-x` | **Group (g):** read, execute |
| `r--` | **Others (o):** read only |
| `khushi` / `devteam` | Owner and group |

| Permission | On a file | On a directory |
|------------|-----------|----------------|
| **r** (read) | View contents | List contents |
| **w** (write) | Modify | Create or delete files inside |
| **x** (execute) | Run as a program | Enter with `cd` |

## 8. Numeric permissions

**r = 4, w = 2, x = 1.** Add them for each of owner, group and others.

| Letters | Calculation | Number |
|---------|-------------|--------|
| `rwx` | 4 + 2 + 1 | **7** |
| `rw-` | 4 + 2 | **6** |
| `r-x` | 4 + 1 | **5** |
| `r--` | 4 | **4** |
| `---` | 0 | **0** |

| Mode | Meaning | Typical use |
|------|---------|-------------|
| **755** | Owner full, others read and execute | Scripts, directories |
| **644** | Owner read and write, others read | Normal files |
| **600** | Owner read and write only | Private keys, secrets |
| **777** | Everyone full access | ⚠️ Unsafe, avoid |

## 9. chmod, chown, chgrp

```bash
chmod 755 script.sh          # numeric mode
chmod +x script.sh           # add execute permission
chmod u+w file.txt           # give owner write
chmod go-r file.txt          # remove read from group and others
chmod -R 755 folder          # apply recursively

sudo chown khushi file.txt           # change owner
sudo chown khushi:devteam file.txt   # change owner and group
sudo chgrp devteam file.txt          # change group only
```

> A script needs the **execute** permission to run with `./script.sh`. Without `chmod +x` you get "Permission denied".

## 10. Groups

A group is a set of users that share access to files.

| Command | Purpose |
|---------|---------|
| `groups` | Show my groups |
| `sudo groupadd <group>` | Create a group |
| `sudo usermod -aG <group> <user>` | Add a user to a group |
| `sudo groupdel <group>` | Delete a group |

> The `-a` in `usermod -aG` appends the group. Without it, the user is removed from existing groups. The change takes effect at the next login.

---

## 💬 Interview answers

**Q: What is sudo?**
> "sudo lets an authorised user run a single command with administrator privileges, using their own password. It is safer than logging in as root."

**Q: What is the difference between apt update and apt upgrade?**
> "apt update refreshes the list of available packages. apt upgrade installs newer versions of the packages that are already installed."

**Q: What does chmod 755 mean?**
> "It gives the owner read, write and execute permissions, and gives the group and others read and execute permissions."

**Q: How do you add a user to a group?**
> "With `sudo usermod -aG groupname username`. The `-a` flag appends the group without removing the user from existing ones."

---

## 🛠️ Practicals (completed in Ubuntu on WSL)

### Practical 1 · Users

```bash
whoami && id
sudo adduser testuser
id testuser
tail -n 2 /etc/passwd
su - testuser              # switch user, then 'exit' once to return
sudo passwd devuser        # change a password
sudo deluser --remove-home devuser
```

**Learned:** a new user has no admin power, so `sudo` fails for them ("not in the sudoers file"). `adduser` creates the home folder, and `--remove-home` removes it on deletion.

### Practical 2 · Package management with apt

```bash
sudo apt update
apt show tree
sudo apt install tree
sudo apt install htop
sudo apt remove tree
sudo apt autoremove
apt list --installed | wc -l      # about 554 packages installed
```

**Learned:** `update` refreshes the package list, `install` and `remove` change software, and `|` (pipe) passes one command's output to another.

### Practical 3 · Permissions

```bash
chmod 600 notes.txt        # -rw-------
chmod 755 notes.txt        # -rwxr-xr-x
chmod 000 notes.txt        # Permission denied, even for the owner
chmod +x hello.sh          # allow ./hello.sh to run
chmod g+w notes.txt        # symbolic form
chmod 000 secret-dir       # cannot cd into it without x
sudo chown testuser file   # change owner
```

**Learned:** a script needs the execute bit to run, a directory needs `x` to be entered, and `600` hides a file from other users while `644` lets them read it. Tested with `testuser` in `/tmp`.

### Practical 4 · Groups and mini challenge

```bash
sudo groupadd devteam
sudo usermod -aG devteam testuser
sudo chgrp devteam /tmp/team-share
sudo chmod 770 /tmp/team-share
sudo gpasswd -d testuser devteam     # remove from group, access is lost
```

**Mini challenge:** created a `deployer` user and a `webteam` group, then a `/tmp/website` folder (mode 750) with `index.html` (640) and an executable `deploy.sh` (750).

**Lesson learned:** `deploy.sh` failed with *Permission denied* for `deployer`. Cause: `chgrp -R` only changes files that already exist, and a new file takes the **creator's group**, not the folder's. Fixed with `sudo chgrp webteam /tmp/website/deploy.sh`. Using `chmod g+s` on a folder (setgid) makes new files inherit the folder's group.

### Checklist

- [✔️] Practical 1: users and sudo
- [✔️] Practical 2: apt install, remove, autoremove
- [✔️] Practical 3: chmod and chown
- [✔️] Practical 4: groups, shared folder, mini challenge
- [✔️] Cleaned up test users, groups and `/tmp` folders
## ✅ Key takeaways

1. Linux is multi-user: root, normal and system users.
2. `sudo` runs one command with admin power and is safer than using root.
3. `apt update` refreshes the package list, then `apt install` installs software.
4. Permissions apply to owner, group and others as read, write and execute.
5. r = 4, w = 2, x = 1, so 755 and 644 are the most common modes.
6. `chmod` changes permissions, `chown` changes ownership, and `usermod -aG` adds users to groups.

⬅️ [Day 04](../day-04/README.md) · 🏠 [Main README](../../README.md)
