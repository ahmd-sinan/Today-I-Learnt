# Linux CLI: Process Management, States, and Signals ⚙️

**Date:** 2026-11-15

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #Processes #SysAdmin #Troubleshooting

Today I learned the architecture of how Linux handles running programs. Understanding process states, scheduling, identifiers, and termination signals is critical for monitoring system health and troubleshooting unresponsive applications.

## What Is a Process?
A process is an instance of one or more related tasks (called threads) executing on your computer. It is important to understand that a process is not the exact same thing as a program or a command: a single command can actually launch several processes at once. Some run independently of one another; others are related and work closely together. 

**Resource Consumption:**
Processes consume system resources: memory, CPU cycles, and peripheral devices (network cards, hard drives, displays). The operating system, specifically the kernel at its core, manages these resources, ensuring each process gets a fair share while keeping the system running smoothly.

## Process Types
Processes come in a few different types depending on the job they do. A terminal window is itself a process (one kind of command shell) that runs interactively, but programs can also be detached to run in the background.

| Process Type | Description | Examples |
| :--- | :--- | :--- |
| **Interactive** | Started by a user, either at the command line or through a graphical interface like an icon or menu. | `bash`, `firefox`, `top`, `Slack`, `LibreOffice`. |
| **Batch** | Automatic processes scheduled to run and then disconnected from the terminal. They're queued and handled on a first-in, first-out (FIFO) basis. | `updatedb`, `ldconfig`, log rotation scripts. |
| **Daemons** | Background server processes that run continuously. Many launch at system startup and then wait quietly until their service is needed. | `httpd`, `sshd`, `libvirtd`, `cupsd`, `systemd-resolved`. |
| **Threads** | Lightweight processes that run under the umbrella of a main process, sharing its memory and resources but scheduled individually. A thread can end without ending the whole process, and a process can spawn new threads at any time. Many non-trivial programs are multi-threaded. | `dconf-service`, `gnome-terminal-server`. |
| **Kernel Threads** | Kernel tasks that users don't start, stop, or control directly. They handle low-level work like moving a task between CPUs or making sure disk input/output completes. | `kthreadd`, `migration`, `ksoftirqd`. |

## Process Identifiers (IDs)
At any given moment, multiple processes are running, and the operating system keeps track of them all by assigning unique identifiers. PIDs are usually handed out in ascending order as processes are created. 

| ID Type | Description |
| :--- | :--- |
| **Process ID (PID)** | The process's unique ID number. |
| **Parent Process ID (PPID)** | The process that started this one. If the parent exits before the child, the orphaned child is "adopted" — normally by PID 1 (`init`/`systemd`), so its PPID becomes 1. Note: Certain supervisor programs can register to adopt their own orphaned descendants instead, but PID 1 is the default. |
| **Thread ID (TID)** | The thread's ID number. For a single-threaded process, this is the same as the PID. In a multi-threaded process, every thread shares the same PID but has its own unique TID. |

## Process Scheduling and States
A critical kernel function called the **scheduler** constantly shifts processes on and off the CPU, sharing time according to each process's priority, how much time it needs, and how much it has already had.

*   **Running/Runnable (R):** The process is actively executing on a CPU or ready and waiting its turn on a run queue. 
*   **Sleeping (S / D):** The process is waiting for something to happen (like user input or disk data) before it can continue. It sits in a wait queue.
*   **Zombie (Z):** Occasionally, a child process finishes, but its parent hasn't yet checked on it to collect its exit status. It is not truly alive, but still appears in the system's process list until the parent cleans it up.

**Viewing Process States:**
You can see these states using the `ps` command.
```bash
$ ps -eo pid,ppid,stat,comm
```

*Sample Output:*

```Plaintext
  PID  PPID STAT COMMAND
    1     0 Ss   systemd
  842     1 Ss   sshd
 1290  1187 R+   ps
 1305  1187 S    sleep
```
*(Note: The extra lowercase letters and symbols in the `STAT` column provide more detail. For example, `s` means it is a session leader, and `+` means it is running in the foreground).*

## Terminating a Process (`kill`)
Sooner or later, an application will stop responding. To get rid of it, you use the `kill` command. The name `kill` is historical and slightly misleading: the command actually sends signals to a process, not just a termination order.

#### Step 1: Find the PID
You must first find the application's unique Process ID. You can use` pgrep` to search by name:

```Bash
$ pgrep firefox
4820
```

#### Step 2: Send the Kill Signal
To forcefully stop the process immediately without waiting for it to clean up, you send signal `9` (SIGKILL). These two commands do the exact same thing:

```Bash
$kill -9 4820$ kill -SIGKILL 4820
```
> ***💡 SysAdmin Pro-Tip: SIGTERM vs SIGKILL***
> 
> Force-killing a process with `-9` (`SIGKILL`) can cause data corruption because the program is instantly destroyed without a chance to save files.
> 
> It is always best practice to try standard termination first by running `kill 4820` (which defaults to `-15` or `SIGTERM`). This politely asks the process to save its data and shut down. Only use `-9` if the process completely refuses to close!
