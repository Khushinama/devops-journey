# Day 04 · Linux Fundamentals: Basics and the Filesystem

📅 **Date:** 05 Oct 2026 · ⏱️ **Time:** ~3 hrs (video + practice) · 🎥 **Source:** CodeWithHarry, Linux tutorial (YouTube)

## 📌 At a glance

| | |
|---|---|
| **Topics** | What Linux is, terminal and shell, filesystem structure, paths, basic commands |
| **Key idea** | Linux servers are managed through the command line, and everything is organised in one tree starting at `/` |
| **Outcome** | Can navigate the filesystem, create, copy, move and delete files, and read `ls -l` output |

---

## 1. What is Linux?

**Linux** is a free, open-source operating system. It is built around the **Linux kernel**, created by Linus Torvalds in 1991.

- **Kernel:** the core that connects hardware (CPU, RAM, disk) with software.
- **Distribution (distro):** the kernel packaged with tools and software.

| Distro | Typical use |
|--------|-------------|
| **Ubuntu** | Beginners and servers (used in this journey, via WSL) |
| **Debian** | Stable servers |
| **CentOS / RHEL** | Enterprise servers |
| **Amazon Linux** | AWS servers |

## 2. Why Linux matters for DevOps

- Most servers, cloud instances and containers run on Linux
- Free, stable, secure and lightweight (no graphical interface needed)
- Almost everything can be done with commands, which makes automation easy

## 3. Terminal, shell and bash

| Term | Meaning |
|------|---------|
| **Terminal** | The window where commands are typed |
| **Shell** | The program that reads and runs the commands |
| **Bash** | The most common shell, the default in Ubuntu |

Reading the prompt:

```text
khushi@DESKTOP-8HM5O20:~$
```

| Part | Meaning |
|------|---------|
| `khushi` | Username |
| `DESKTOP-8HM5O20` | Machine name |
| `~` | Current location (home directory) |
| `$` | Normal user (`#` means root user) |

## 4. Command structure

```text
command   -option   argument
ls        -l        /home
```

- **Command:** what to do
- **Option:** how to do it
- **Argument:** what to do it on

## 5. Filesystem hierarchy

Linux has a single tree that starts at the root directory `/`.

```text
/
├── home    user directories (/home/khushi)
├── etc     configuration files
├── var     logs and changing data (/var/log)
├── bin     essential commands
├── usr     installed programs
├── tmp     temporary files
├── root    home directory of the root user
└── mnt     mounted drives (in WSL, Windows C: drive is /mnt/c)
```

> **Everything is a file** in Linux: documents, settings and devices.

> ⚠️ In WSL, work only inside `/home/<username>`. `/mnt/c` is the Windows system.

## 6. Paths

| Type | Example | Meaning |
|------|---------|---------|
| **Absolute path** | `/home/khushi/notes.txt` | Starts from `/`, the full address |
| **Relative path** | `../notes.txt` | Starts from the current directory |

| Symbol | Meaning |
|--------|---------|
| `.` | Current directory |
| `..` | Parent directory (one level up) |
| `~` | Home directory |
| `/` | Root directory |

## 7. Reading `ls -l`

```text
-rw-r--r-- 1 khushi khushi   15 Oct 5 10:30 notes.txt
drwxr-xr-x 2 khushi khushi 4096 Oct 5 10:25 linux-practice
```

| Field | Meaning |
|-------|---------|
| First character | `-` = file, `d` = directory |
| `rw-r--r--` | Permissions (covered in the next lesson) |
| `khushi khushi` | Owner and group |
| `15` | Size in bytes |
| `Oct 5 10:30` | Last modified |
| `notes.txt` | Name |

**Hidden files** start with a dot (for example `.bashrc`). Show them with `ls -a`.

## 8. Basic commands cheat sheet

### Navigation

| Command | Purpose |
|---------|---------|
| `pwd` | Show the current directory |
| `ls` | List files |
| `ls -l` | Long listing (size, date, permissions) |
| `ls -a` | Include hidden files |
| `ls -lh` | Human-readable sizes |
| `cd <folder>` | Enter a folder |
| `cd ..` | Go up one level |
| `cd ~` | Go to the home directory |
| `cd /` | Go to the root directory |
| `cd -` | Go back to the previous directory |

### Files and folders

| Command | Purpose |
|---------|---------|
| `mkdir <name>` | Create a folder |
| `mkdir -p a/b/c` | Create nested folders |
| `touch <file>` | Create an empty file |
| `cp <src> <dest>` | Copy a file |
| `cp -r <dir> <dest>` | Copy a folder |
| `mv <src> <dest>` | Move or rename |
| `rm <file>` | Delete a file |
| `rm -r <dir>` | Delete a folder and its contents |
| `rmdir <dir>` | Delete an empty folder |

> ⚠️ `rm` has no undo. Check `pwd` and `ls` before deleting.

### Viewing and writing content

| Command | Purpose |
|---------|---------|
| `cat <file>` | Print a file |
| `less <file>` | Read a long file page by page (`q` to quit) |
| `head <file>` / `tail <file>` | First / last 10 lines |
| `wc -l <file>` | Count lines |
| `echo "text"` | Print text |
| `echo "text" > file` | Write to a file (overwrites) |
| `echo "text" >> file` | Append to a file |

### Info and help

| Command | Purpose |
|---------|---------|
| `whoami` | Current user |
| `hostname` | Machine name |
| `date` | Current date and time |
| `uname -a` | System information |
| `man <command>` | Manual page |
| `<command> --help` | Quick help |
| `history` | Previous commands |
| `clear` | Clear the screen |

### Shortcuts

| Shortcut | Purpose |
|----------|---------|
| **Tab** | Auto-complete names |
| **Up arrow** | Previous command |
| **Ctrl + C** | Stop a running command |
| **Ctrl + L** | Clear the screen |
| **Ctrl + A / Ctrl + E** | Start / end of the line |

## 9. Common mistakes

1. Linux is **case-sensitive**: `Notes.txt` and `notes.txt` are different files.
2. `rm` is permanent.
3. Working inside `/mnt/c` can affect Windows.
4. "No such file" errors usually mean the wrong folder or a typo. Check with `pwd` and `ls`.

## 10. Interview answers

**Q: What is Linux?**
> "Linux is a free, open-source operating system built around the Linux kernel. It is stable, secure and lightweight, so most servers and cloud systems run on it."

**Q: Why is Linux important for DevOps?**
> "Most servers, cloud instances and containers run on Linux, and almost everything on it can be done from the command line, which makes automation easy."

**Q: Absolute vs relative path?**
> "An absolute path starts from the root directory, like /home/khushi/notes.txt. A relative path starts from the current directory, like ../notes.txt."

---

## ✏️ Vim basics

Vim is a terminal text editor with two main modes.

| Mode | Purpose | How to enter |
|------|---------|--------------|
| **Normal** | Run commands, no typing. Vim opens here | Press `Esc` |
| **Insert** | Type text | Press `i` |

| Command (in Normal mode) | Purpose |
|--------------------------|---------|
| `i` | Start typing (Insert mode) |
| `:w` | Save |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `dd` | Delete a line |
| `u` | Undo |
| `/word` | Search for a word |
| `:set number` | Show line numbers |

> If stuck inside Vim: press `Esc` twice, then type `:q!` and press Enter. `nano` is a simpler alternative (Ctrl + O to save, Ctrl + X to exit).

---

## 🛠️ Practicals (completed in Ubuntu on WSL)

### Practical 1 · Build a project folder structure

```bash
mkdir -p devops-lab/app devops-lab/logs devops-lab/notes
cd devops-lab/app
touch server.js README.md
echo "console.log('Hello DevOps');" > server.js
echo "# My App" > README.md
echo "Runs on port 5000" >> README.md
cd ../logs
touch app.log
echo "App started" >> app.log
echo "Request received" >> app.log
wc -l app.log
cd ~/devops-lab && ls -R
```

**Learned:** `mkdir -p` creates nested folders. `>` overwrites a file and `>>` appends to it. `ls -R` lists folders recursively.

### Practical 2 · Copy, move, rename and delete

```bash
cp app/server.js app/server-backup.js      # copy a file
cp -r app app-copy                          # copy a folder
mv app-copy app-v2                          # rename
mv app/server-backup.js notes/              # move
rm notes/server-backup.js                   # delete a file
rm -r app-v2                                # delete a folder
rm -i temp.txt                              # delete with confirmation
```

**Learned:** `mv` both moves and renames. Folders need `-r` to copy or delete. `rm` has no undo, so `rm -i` and checking `pwd` first are good habits.

### Practical 3 · Reading files

```bash
seq 1 20 > numbers.txt
head -n 3 numbers.txt
tail -n 3 numbers.txt
less numbers.txt            # press q to quit
wc -l numbers.txt
cat /etc/os-release
tail -f logs/app.log        # live log view, Ctrl + C to stop
```

**Learned:** `head` and `tail` show the start and end of a file, `less` pages through long files, `wc -l` counts lines, and `tail -f` follows a log live, which is how real server issues are watched.

### Practical 4 · Mini challenge

Built a `challenge` project with `src`, `config`, `logs` and `backup`, then:

- Wrote `main.js` in Vim
- Created `settings.txt` with `echo`
- Generated a 12-line `server.log` with `seq` and two `ERROR` lines
- Used `head`, `tail` and `wc -l` on the log
- Backed up files with `cp` and `cp -r`, renamed with `mv`, and deleted with `rm`
- Verified the final structure with `ls -R`

```text
challenge/
├── backup/   settings.bak  src-copy/helpers.js
├── config/   settings.txt
├── logs/     server.log
└── src/      helpers.js  main.js
```

### Checklist

- [x] Practical 1: folder structure
- [x] Practical 2: copy, move, rename, delete
- [x] Practical 3: reading files and live logs
- [x] Practical 4: mini challenge
- [x] Wrote and saved a file in Vim

## ✅ Key takeaways

1. Linux is an open-source OS, and Ubuntu is a distribution of it.
2. Servers run Linux, so it is essential for DevOps.
3. The terminal runs bash, and commands follow `command -option argument`.
4. The filesystem is one tree starting at `/`, and home is `/home/<user>`.
5. Absolute paths start at `/`; relative paths start at the current folder.
6. Navigation, file and content commands are the daily toolkit.

⬅️ [Day 03](../../01-foundations/day-03/README.md) · 🏠 [Main README](../../README.md)
