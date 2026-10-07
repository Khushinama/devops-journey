# Day 06 · Linux: Processes, Services, Environment and Archives

📅 **Date:** 07 Oct 2026 · ⏱️ **Time:** ~2 hrs · 🎥 **Source:** CodeWithHarry, Linux tutorial (YouTube)

## 📌 At a glance

| | |
|---|---|
| **Topics** | Processes, services with systemd, environment variables, PATH, `.bashrc`, archives |
| **Key idea** | Everything running on Linux is a process, long-running ones are services, and behaviour is configured through environment variables |
| **Outcome** | Can inspect and stop processes, control services and read their logs, manage environment settings, and create archives |

---

# Part A · Processes

## 1. What is a process?

A **process** is a running program. Every process gets a unique number called the **PID** (Process ID).

## 2. Viewing processes

| Command | Purpose |
|---------|---------|
| `ps` | Processes in the current terminal |
| `ps aux` | All processes on the system |
| `top` | Live process view (`q` to quit) |
| `htop` | Friendlier live view (`q` to quit) |
| `pgrep <name>` | Find the PID by name |

Key columns in `ps aux`:

| Column | Meaning |
|--------|---------|
| `USER` | Owner of the process |
| `PID` | Process ID |
| `%CPU` / `%MEM` | CPU and memory use |
| `STAT` | State: `R` running, `S` sleeping, `Z` zombie |
| `COMMAND` | The program |

## 3. Foreground and background

| Task | How |
|------|-----|
| Run in the background | Add `&`: `sleep 300 &` |
| List background jobs | `jobs` |
| Bring a job to the foreground | `fg` |
| Pause the running command | `Ctrl + Z` |
| Stop the running command | `Ctrl + C` |

## 4. Stopping processes

| Command | Effect |
|---------|--------|
| `kill <PID>` | Sends **SIGTERM**: asks the process to shut down cleanly |
| `kill -9 <PID>` | Sends **SIGKILL**: stops it immediately, no cleanup |
| `pkill <name>` | Stop by name |

> Try `kill` first. Use `kill -9` only as a last resort, and only on your own processes.

---

# Part B · Services (systemd)

## 5. What is a service?

A **service** (daemon) is a program that runs continuously in the background, such as a web server, a database or SSH. **systemd** starts services at boot and keeps them running. It is controlled with `systemctl`.

## 6. systemctl commands

| Command | Purpose |
|---------|---------|
| `systemctl status <name>` | Show state |
| `sudo systemctl start <name>` | Start now |
| `sudo systemctl stop <name>` | Stop now |
| `sudo systemctl restart <name>` | Stop and start |
| `sudo systemctl reload <name>` | Re-read config without stopping |
| `sudo systemctl enable <name>` | Start automatically at boot |
| `sudo systemctl disable <name>` | Do not start at boot |
| `systemctl is-active <name>` | Print `active` or `inactive` |

> **start** runs the service now. **enable** makes it start at boot. They are independent.

## 7. Reading logs with journalctl

| Command | Purpose |
|---------|---------|
| `journalctl -u <name>` | Logs for one service |
| `journalctl -u <name> -n 50` | Last 50 lines |
| `journalctl -u <name> -f` | Follow live (`Ctrl + C` to stop) |
| `journalctl --since "1 hour ago"` | Recent logs |

First steps when something breaks on a server: `systemctl status`, then `journalctl`.

---

# Part C · Environment variables, PATH and bashrc

## 8. Variables

```bash
NAME=Khushi          # no spaces around =
echo $NAME           # read with $
```

| Type | Scope |
|------|-------|
| **Shell variable** | Only the current shell |
| **Environment variable** | Created with `export`, also visible to programs started from the shell |

```bash
export PORT=5000
echo $PORT
env                  # list environment variables
```

Common variables: `HOME`, `USER`, `PATH`, `SHELL`, `PWD`.

> In Node.js, `process.env.PORT` reads an environment variable. A `.env` file is a convenient way to define them per environment.

## 9. PATH

`PATH` is a colon-separated list of directories where the shell looks for commands.

```bash
echo $PATH
which ls                              # where a command lives
export PATH=$PATH:/home/khushi/bin    # add a directory
```

The current directory is not in `PATH`, which is why scripts need `./script.sh`.

## 10. The `.bashrc` file

`~/.bashrc` runs every time a new interactive terminal starts. Variables set with `export` in a terminal disappear when it closes, so put permanent settings here.

```bash
nano ~/.bashrc           # edit
alias ll='ls -la'        # shortcut
export PORT=5000         # persistent variable
source ~/.bashrc         # apply changes now
```

> Make a backup before editing: `cp ~/.bashrc ~/.bashrc.bak`

---

# Part D · Archives

## 11. tar, gzip and zip

| Tool | Role |
|------|------|
| **tar** | Bundles many files into one archive (no compression) |
| **gzip** | Compresses a file |
| **.tar.gz** | Bundle plus compression |

| Command | Purpose |
|---------|---------|
| `tar -cvf a.tar folder` | Create an archive |
| `tar -xvf a.tar` | Extract |
| `tar -czvf a.tar.gz folder` | Create and compress |
| `tar -xzvf a.tar.gz` | Extract a compressed archive |
| `tar -tf a.tar.gz` | List contents without extracting |
| `tar -xzvf a.tar.gz -C /tmp` | Extract into another directory |
| `zip -r a.zip folder` / `unzip a.zip` | Zip format (`sudo apt install zip unzip`) |

**Flags:** `c` create, `x` extract, `z` gzip, `v` verbose, `f` file name (always last).

---

## 💬 Interview answers

**Q: What is the difference between a process and a service?**
> "A process is any running program with a PID. A service is a long-running background process managed by systemd, which can start at boot and be controlled with systemctl."

**Q: kill vs kill -9?**
> "kill sends SIGTERM, which asks a process to shut down cleanly. kill -9 sends SIGKILL, which stops it immediately without cleanup, so it is a last resort."

**Q: systemctl start vs enable?**
> "start runs the service now. enable makes it start automatically at boot. They are independent."

**Q: What is PATH?**
> "PATH is an environment variable holding a list of directories where the shell looks for executable commands."

**Q: What is .bashrc?**
> "It is a script that runs whenever a new interactive bash shell starts, so it is used for aliases and persistent environment variables."

**Q: tar vs gzip?**
> "tar bundles many files into one archive without compressing. gzip compresses a file. Together they produce a .tar.gz."

---

## 🛠️ Practicals

- [ ] Practical 1: inspect and stop processes
- [ ] Practical 2: services and logs
- [ ] Practical 3: environment variables, PATH and alias
- [ ] Practical 4: archives and a mini challenge

## ✅ Key takeaways

1. A process is a running program with a PID. `ps aux` and `top` show them.
2. Use `kill` first, and `kill -9` only as a last resort.
3. Services run in the background and are managed with `systemctl`. `journalctl` shows their logs.
4. `start` runs a service now, `enable` runs it at boot.
5. Environment variables carry settings, and PATH tells the shell where commands live.
6. `.bashrc` makes aliases and variables permanent, and `source` applies changes immediately.
7. `tar -czvf` creates a compressed archive and `tar -xzvf` extracts it.

⬅️ [Day 05](../day-05/README.md) · 🏠 [Main README](../../README.md)
