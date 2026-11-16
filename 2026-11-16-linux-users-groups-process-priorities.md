# Linux CLI: User/Group IDs, Process Priorities & GUI Monitoring 

**Date:** 2026-11-16

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #SysAdmin #Processes #Permissions #Performance #GUI

Today I learned how Linux tracks who is running a process and how it determines which processes get first dibs on the CPU. I explored the critical distinction between Real and Effective IDs, how to tune CPU scheduling using `nice` and `renice`, and how to manage it all visually using the GNOME System Monitor.

## User and Group IDs (UID & GID)
Multiple people can use a Linux system at the same time, and each can run multiple processes. The operating system uses internal ID numbers to track ownership and access rights.

*   **Real User ID (RUID):** Identifies the actual, logged-in user who started the process.
*   **Effective User ID (EUID):** Determines the actual access rights and permissions the process has while it is running. (This may or may not match the RUID).

> 💡 **Example: The `passwd` Command**
> When an ordinary (unprivileged) user runs the `passwd` command to change their password, the process still uses their **RUID** to record who ran it. However, it runs with an **EUID** of `root` (0). This temporary privilege escalation is required so the command can write to the protected `/etc/shadow` file where passwords are stored. 
> 
> *Takeaway: The RUID identifies who you are, while the EUID determines what the process is allowed to do.*

**Group IDs:**
Users can also be organized into groups. Each group has a Real Group ID (RGID), and the group's access rights are determined by the Effective Group ID (EGID). A user can belong to one or more groups.

### Checking Your IDs (`id`)
Most of the time, we don't worry about the real-versus-effective distinction and just talk about the User ID (UID) and Group ID (GID). You can see your own IDs and group memberships with the `id` command:

```bash
$ id 
uid=1000(student) gid=1000(student) groups=1000(student),27(sudo),4(adm)
```

## Process Priorities and "Niceness"
At any moment, many processes may be ready to run, but each CPU core (or hardware thread) can only execute one task at a time. Linux lets you adjust a process's scheduling priority to give critical tasks preferential access to CPU time.

Standard process priority is set using a value called niceness:

* Niceness ranges from **−20 (highest priority)** to **+19 (lowest priority)**.
* A process with a high nice value (e.g., +15) is "nice"—it steps aside and gladly yields CPU cycles to other programs.
* A process with a negative nice value (e.g., -10) is "selfish"—it demands higher access to CPU time.

> **📏 The Rule of Thumb:**
> Lower numerical niceness = Higher CPU priority. (The "nicer" the process, the lower its priority. This convention dates all the way back to the earliest days of UNIX).

### Starting a Process with `nice`
By default, new processes start with a niceness of `0`. You can launch a process with a specific priority using the `nice` command.

**Permission Note:** Regular users can only increase the niceness of their own processes (making them lower priority). Giving a process a negative niceness value (making it selfish) requires administrator (`sudo`) privileges.

```Bash
# Launching a heavy background backup as a "nice" process (low priority)
$ nice -n 15 tar -zcvf backup.tar.gz /home/student/

# Launching a critical task with high CPU priority (requires sudo)
$ sudo nice -n -5 ./important-job
```

### Modifying Running Processes (`renice`)
While `nice` is used when starting a command, `renice` is used to change the priority of a program that is already running.

Just like `nice`, regular users can only use `renice` to lower the priority of their own processes. You need `root` privileges to make a running process higher priority.

**`renice` Syntax and Examples:**

```Bash
# General Syntax
$ renice <new_nice_value> -p <PID>
```

#### Example A: Lowering the priority of a running script
If you notice a background script (`PID 4820`) is slowing down your web browsing, you can make it "nicer" to free up CPU time:

```Bash
$ renice +10 -p 4820
4820 (process ID) old priority 0, new priority 10
```

#### Example B: Boosting the priority of a critical application
If a database server (`PID 1105`) is struggling under load, a SysAdmin can force it to the front of the CPU line:

```Bash
$ sudo renice -10 -p 1105
1105 (process ID) old priority 0, new priority -10
```

#### Example C: Changing priority for an entire user
You can alter the priority of every process owned by a specific user using the `-u` flag. This is useful if a specific user is hogging system resources:

```Bash
$ sudo renice +5 -u student
```

## Real-Time Priorities
For highly time-sensitive background tasks, such as live audio processing or high-speed data capture, Linux supports real-time priorities. Real-time tasks take absolute precedence over all standard processes, regardless of their niceness values.

*Note: In standard Linux distributions, real-time priorities are "soft" real-time. This is distinct from hard real-time systems (like those used in pacemakers or avionics), which guarantee mathematically that tasks finish within strict, non-negotiable time limits.*

## 5. Graphical Process Management (`gnome-system-monitor`)
If you are working inside a graphical desktop environment (like Ubuntu's default GNOME desktop), you do not always have to use CLI tools like `ps`, `top`, `kill`, or `renice`.

You can use the GNOME System Monitor, which is the graphical equivalent of the Windows Task Manager.

**Command to launch it:**

```Bash
$ gnome-system-monitor &
```
*(Note: Appending the & launches it in the background so you get your terminal prompt back immediately).*

**Features of the System Monitor:**

* **Visual Tracking:** Sort running processes visually by CPU, Memory, or PID without remembering ps flags.
* **Right-Click to Kill:** You can right-click any unresponsive application and select End (SIGTERM) or Kill (SIGKILL).
* **Change Priority Visually:** You can right-click a process and select Change Priority to adjust its niceness via a simple slider or drop-down menu (Very High, High, Normal, Low, Very Low) instead of calculating numeric values.
