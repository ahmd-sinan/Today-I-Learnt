# Linux CLI: Inspecting Processes with `ps` & `pstree` 

**Date:** 2026-11-25

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #Processes #ps #pstree #SysAdmin #Monitoring

Today I learned how to inspect running processes using `ps` and visualize parent-child hierarchies with `pstree`. I also learned the historical quirk behind `ps` supporting two different flag styles: System V and BSD.

## Overview of `ps` (Process Status)
`ps` reports detailed information about the processes currently running on the system, identified by their PID. 

*   **Snapshot vs. Live View:** `ps` takes a **one-time snapshot** of the system at the exact moment you run it, rather than updating continuously.
*   **Live Terminal Alternatives:** If you want a constantly refreshing live view, use `top` (always pre-installed), or friendlier variants like `htop`, `atop`, or `btop` (which may need to be installed first).
*   **Graphical Alternatives:** Most Linux desktops include a GUI monitor: `gnome-system-monitor` on Ubuntu's GNOME desktop, or `plasma-systemmonitor` on KDE Plasma.

**The Two Flag Conventions:**
Because of UNIX history, `ps` supports two distinct option styles:
1.  **System V Style:** Uses leading dashes (e.g., `ps -ef`).
2.  **BSD Style:** Uses **no** leading dashes (e.g., `ps aux`).

---

## System V Style (Options with Dashes)
Running `ps` on its own with no flags shows only the processes running inside your current terminal shell:

```bash
$ ps
  PID TTY          TIME CMD
 4102 pts/0    00:00:00 bash
 4231 pts/0    00:00:00 ps
```

### Common System V Variations
*   **`ps -u <username>`** — Shows all processes belonging to a specific user (e.g., `ps -u student`).
*   **`ps -ef`** — Shows **e**very process on the system in **f**ull-format detail (one of the most common commands in Linux).
*   **`ps -eLf`** — Goes a step further and shows one line per **thread** (`-L`), since a single process can contain multiple threads.

### Sample `ps -ef` Output
```bash
$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 09:14 ?        00:00:02 /sbin/init
root         842       1  0 09:14 ?        00:00:00 /usr/sbin/sshd -D
student     4102    4098  0 10:05 pts/0    00:00:00 bash
student     4231    4102  0 10:07 pts/0    00:00:00 ps -ef
```

**Column Breakdown:**
| Column | Meaning |
| :--- | :--- |
| **`UID`** | The username/owner who started the process. |
| **`PID`** | The unique Process ID. |
| **`PPID`** | The Parent Process ID (the process that spawned this one). |
| **`C`** | Processor utilization (rough CPU usage percentage). |
| **`STIME`** | The start time (or date) when the process was launched. |
| **`TTY`** | The controlling terminal (`?` means it's a background daemon with no terminal). |
| **`TIME`** | Cumulative CPU time consumed by the process so far. |
| **`CMD`** | The exact command and arguments that launched the process. |

---

## BSD Style (Options without Dashes)
Inherited from Berkeley Software Distribution (BSD) Unix, this style omits the leading dash. The most famous combination lists every process for every user along with CPU and memory usage:

```bash
$ ps aux
```
*   **`a`**: Show processes for all users.
*   **`u`**: Display the process's user/owner and resource usage (`%CPU`, `%MEM`, `VSZ`, `RSS`).
*   **`x`**: Include processes not attached to a terminal (like background daemons).

> 💡 **Tip: Avoid the `ps -aux` Typo!**
> In BSD style, write `aux` **without** a dash. Typing `ps -aux` is a common mistake that mixes System V and BSD syntax. Modern Linux `ps` will usually detect the typo, print a warning, and guess what you meant—but it is best practice to write `ps aux` (no dash) and `ps -ef` (with dash) cleanly.

### Custom Columns (`ps axo`)
You can pick the exact columns you want to display using `axo` (or `-eo` in System V style). For example, to view only the PID, niceness (`ni`), user, and command name:

```bash
$ ps axo pid,ni,user,comm
```

---

## Real-World SysAdmin `ps` Tricks 
Because `ps -ef` and `ps aux` output hundreds of lines on a busy server, admins almost always combine them with `grep`, `head`, or `--sort`:

**1. Find a specific running service (`ps` + `grep`):**
```bash
$ ps aux | grep sshd
```

**2. Find the top 5 memory-hogging processes:**
```bash
$ ps aux --sort=-%mem | head -n 6
```
*(The `-` before `%mem` sorts in descending order; `head -n 6` shows the header row plus the top 5 processes).*

**3. Find the top 5 CPU-hogging processes:**
```bash
$ ps aux --sort=-%cpu | head -n 6
```

---

## The Process Tree (`pstree`) 
While `ps` outputs a flat list, `pstree` displays running processes as a **visual tree diagram**, making it easy to see exact parent-child relationships (which process launched which).

```bash
$ pstree
```

### Sample Output
```text
systemd─┬─ModemManager───2*[{ModemManager}]
        ├─gnome-shell───{gnome-shell}
        ├─sshd───sshd───bash───pstree
        └─systemd───(sd-pam)
```

**How to Read `pstree` Output:**
*   **Collapsed Siblings (`2*[...]`):** To keep the tree clean, identical sibling processes are collapsed into a single entry (e.g., `2*[...]` means two identical copies).
*   **Sub-threads (`{...}`):** Threads running inside a parent process are shown inside curly braces.
*   **Lineage:** Reading left-to-right shows the exact chain of command (e.g., `systemd` started `sshd`, which started another `sshd` session, which launched your `bash` shell, which ran `pstree`).

### Useful `pstree` Options
| Command | Description |
| :--- | :--- |
| **`pstree -p`** | Displays the **PID** next to every process name in the tree. |
| **`pstree -u`** | Shows **user transitions** (highlights when a child process is running under a different user than its parent). |
| **`pstree <username>`** | Displays only the process tree belonging to a specific user (e.g., `pstree student`). |
| **`pstree -a`** | Includes the full command-line arguments used to launch each process. |