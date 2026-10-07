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

## 🛠️ Practicals (completed in Ubuntu on WSL)

### Practical 1 · Processes

```bash
ps && ps aux | head -n 5
ps aux | wc -l
sleep 300 &                       # run in the background, prints the PID
jobs
pgrep sleep
kill <PID>                        # SIGTERM: asks the process to stop cleanly
sleep 400 & sleep 500 & sleep 600 &
pkill sleep                       # stop several by name
sleep 200                         # Ctrl + Z pauses it, then:
bg                                # resume in the background
fg                                # bring it back, Ctrl + C to stop
sleep 700 &
kill -9 $(pgrep sleep)            # SIGKILL: immediate, no cleanup
ps aux --sort=-%mem | head -n 5   # top memory users
htop                              # q to quit
```

**Learned:** every running program is a process with a PID. `kill` allows a clean shutdown (`Terminated`), while `kill -9` does not (`Killed`) and is a last resort. `jobs`, `bg`, `fg`, `Ctrl + Z` and `Ctrl + C` manage foreground and background work.

### Practical 2 · Services and logs

```bash
ps -p 1 -o comm=                          # systemd confirms services work
systemctl list-units --type=service --state=running
systemctl status cron
systemctl is-active cron && systemctl is-enabled cron
sudo apt install nginx
curl -I localhost                         # HTTP/1.1 200 OK
sudo systemctl stop nginx                 # curl now gives "Connection refused"
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx              # still active, only boot start changes
sudo systemctl enable nginx
sudo journalctl -u nginx -n 20 --no-pager
sudo journalctl -u nginx -f               # live logs (Ctrl + C to stop)
sudo tail -n 3 /var/log/nginx/access.log
sudo nginx -t                             # validate config before reload
systemctl status fake-service             # "could not be found"
sudo systemctl stop nginx
```

**Learned:** `start` runs a service now, `enable` runs it at boot, and the two are independent. `reload` re-reads configuration without dropping connections, so it is safer than `restart` on a live site. Logs are available through `journalctl` and in `/var/log`.

### Practical 3 · Environment variables, PATH and aliases

```bash
cp ~/.bashrc ~/.bashrc.bak                   # backup before editing
MYNAME=Khushi
bash -c 'echo "child sees: $MYNAME"'         # empty: shell variable only
export MYNAME
bash -c 'echo "child sees: $MYNAME"'         # Khushi: environment variable
echo $HOME $USER $SHELL $PWD
env | head -n 10

# script that reads variables, like process.env in Node.js
echo '#!/bin/bash' > show-port.sh
echo 'echo "App would run on port ${PORT:-3000} in ${APP_ENV:-development} mode"' >> show-port.sh
chmod +x show-port.sh
./show-port.sh                               # default 3000
PORT=5000 ./show-port.sh                     # variable for one command only
export PORT=8080 && ./show-port.sh
unset PORT

echo $PATH | tr ':' '\n'
which ls
mkdir ~/bin && cp show-port.sh ~/bin/showport && chmod +x ~/bin/showport
export PATH=$PATH:$HOME/bin
which showport
```

Added to `~/.bashrc` to make them permanent:

```bash
export PATH=$PATH:$HOME/bin
export APP_ENV=development
alias ll='ls -la'
alias projects='cd ~/env-lab'
```

```bash
source ~/.bashrc    # apply without reopening the terminal
```

After closing and reopening the terminal, `APP_ENV`, `ll` and `showport` still worked, while the plain `export MYNAME` was gone.

**Learned:** only `export`ed variables reach child programs. Terminal-only exports vanish when the terminal closes, while `.bashrc` settings persist. Always write `PATH=$PATH:...`, because overwriting PATH breaks every command.

### Practical 4 · Archives and backups

```bash
mkdir -p archive-lab/project/{src,config,logs}
seq 1 50000 > archive-lab/project/logs/app.log
du -sh project
tar -cvf project.tar project               # bundle only
tar -tf project.tar                        # list without extracting
tar -czvf project.tar.gz project           # bundle + compress (much smaller)
ls -lh project.tar project.tar.gz
mkdir restore && tar -xzvf project.tar.gz -C restore
diff -r project restore/project            # no output means identical
tar -xzvf project.tar.gz -C restore2 project/config/settings.txt   # one file only
gzip big.log && gunzip big.log.gz
sudo apt install zip unzip
zip -r project.zip project && unzip project.zip -d zip-restore
tar -czvf backup-$(date +%F).tar.gz project   # dated backup
```

**Learned:** `tar` bundles and `gzip` compresses. In `-czvf name`, `f` must come last. `-C` extracts into a chosen directory, and `$(date +%F)` gives each backup a unique date-stamped name.

### Mini challenge · A fake server from start to finish

Script `app/server.sh` that appends one log line every 2 seconds:

```bash
#!/bin/bash
while true; do
  echo "$(date +%T) Server running on port ${PORT:-3000} in ${APP_ENV:-development} mode" >> ~/day6-challenge/logs/server.log
  sleep 2
done
```

Steps performed:

```bash
mkdir -p ~/day6-challenge/{app,logs,backups}
chmod +x ~/day6-challenge/app/server.sh

export PORT=5000 APP_ENV=staging
~/day6-challenge/app/server.sh &              # run in the background
jobs
pgrep -f day6-challenge/app/server.sh         # find the PID
ps aux | grep server.sh
tail -n 5 ~/day6-challenge/logs/server.log    # log shows port 5000, staging
tail -f ~/day6-challenge/logs/server.log

tar -czvf backups/logs-$(date +%F).tar.gz logs   # backup while it runs
tar -tzf backups/logs-$(date +%F).tar.gz

cp app/server.sh ~/bin/myserver && chmod +x ~/bin/myserver
which myserver
alias applog='tail -n 5 ~/day6-challenge/logs/server.log'

kill $(pgrep -f day6-challenge/app/server.sh) # stop it
wc -l logs/server.log; sleep 3; wc -l logs/server.log   # same number: it stopped

export PORT=6000
./app/server.sh & ./app/server.sh &
kill %1                                       # Terminated
kill -9 %2                                    # Killed

sudo systemctl start nginx && curl -I localhost
sudo journalctl -u nginx -n 5 --no-pager
sudo systemctl stop nginx && systemctl is-active nginx   # inactive

rm -r logs                                    # simulate data loss
tar -xzvf backups/logs-$(date +%F).tar.gz     # restore from the backup
tail -n 3 logs/server.log
```

**Result:** the environment values appeared in the log lines, the backup restored the deleted logs, the line count stopped growing after `kill`, and `jobs` showed `Terminated` versus `Killed`.

**Learned:** this combined environment variables, background processes, PATH, aliases, archives and services in one realistic workflow, from start-up to monitoring, backup, shutdown and recovery.

### Cleanup

```bash
pkill -f day6-challenge/app/server.sh
sudo systemctl stop nginx
rm ~/bin/myserver
unalias applog
unset PORT
rm -r ~/day6-challenge
```

### Checklist

- [✔️] Practical 1: processes
- [✔️] Practical 2: services and logs
- [✔️] Practical 3: environment variables, PATH and aliases
- [✔️] Practical 4: archives and backups
- [✔️] Mini challenge: fake server from start to finish
- [✔️] Cleaned up test files and stopped background scripts

## ✅ Key takeaways

1. A process is a running program with a PID. `ps aux` and `top` show them.
2. Use `kill` first, and `kill -9` only as a last resort.
3. Services run in the background and are managed with `systemctl`. `journalctl` shows their logs.
4. `start` runs a service now, `enable` runs it at boot.
5. Environment variables carry settings, and PATH tells the shell where commands live.
6. `.bashrc` makes aliases and variables permanent, and `source` applies changes immediately.
7. `tar -czvf` creates a compressed archive and `tar -xzvf` extracts it.

⬅️ [Day 05](../day-05/README.md) · 🏠 [Main README](../../README.md)
