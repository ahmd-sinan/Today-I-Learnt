# Linux CLI: Live System Monitoring with `top` (and `htop`, `btop`, `atop`) 

**Date:** 2026-11-26

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #Monitoring #top #htop #btop #atop #SysAdmin #Performance

Today I learned how to monitor a Linux system in real time using the `top` command. While `ps` gives a static one-time snapshot, `top` provides a live, continuously updating dashboard of CPU, memory, and running processes—refreshing every few seconds until you press `q` to quit.

## Overview
If you're coming from Windows or macOS, `top` is the terminal equivalent of **Task Manager** or **Activity Monitor**: a live, sortable process list in text form that is available on virtually every Linux system, even headless servers with no graphical desktop.

The `top` interface is split into two parts:
1. **The Summary Area (Top 5 Lines):** A system-wide health dashboard.
2. **The Process List (Below):** A live, sorted roster of individual processes.

---

## Reading the Summary Area (The Top 5 Lines)
Here is what the full summary area looks like in practice:

```text
top - 14:32:07 up 3 days,  2:15,  2 users,  load average: 0.45, 0.17, 0.12
Tasks: 215 total,   1 running, 214 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.3 us,  0.7 sy,  0.0 ni, 96.8 id,  0.2 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7842.5 total,   3120.8 free,   2450.1 used,   2271.6 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   5045.9 avail Mem
```

### Line 1: System Uptime, Users, and Load Average
Summarizes the system's overall state: current time, how long it's been running (`up`), how many users are logged in, and the 1-, 5-, and 15-minute load averages. On a single CPU, a value around `1.00` means fully utilized but not overloaded; higher values mean processes are competing for CPU time (or a runaway process is stuck).

### Line 2: Tasks (Process States)
Breaks the total number of processes down by state: `running`, `sleeping`, `stopped`, or `zombie`. Comparing the `running` count against the load average helps you judge whether the system is genuinely at capacity or if one user is simply running many processes. An unexpected number of `stopped` or `zombie` processes is worth investigating.

### Line 3: CPU Breakdown (`%Cpu(s)`)
Shows how CPU time is being divided up as percentages:
*   **`us` (User):** Running user (non-kernel) processes.
*   **`sy` (System):** Running kernel (system) tasks.
*   **`ni` (Nice):** User processes running at a modified (`nice`) priority.
*   **`id` (Idle):** Idle time (should be low when load average is high, and vice versa).
*   **`wa` (I/O Wait):** Waiting for I/O, such as disk or network. High `wa` means a storage/network bottleneck.
*   **`hi` / `si` (Interrupts):** Servicing hardware (`hi`) vs. software (`si`) interrupts.
*   **`st` (Steal):** "Steal" time, seen on virtual machines (like cloud VMs), where the host hypervisor reclaims some of the VM's CPU time for other tenants.

### Lines 4 & 5: Physical Memory (RAM) & Swap
Reports memory usage, showing `total`, `free`, `used`, and `buff/cache` (or `avail Mem`):
*   **Line 4 (`MiB Mem`):** Physical memory (RAM).
*   **Line 5 (`MiB Swap`):** Swap space (temporary disk space used as overflow for RAM).

> ⚠️ **Watch Out for Heavy Swapping:**
> Once physical RAM is exhausted, Linux falls back on swap. Because disk access is dramatically slower than RAM, heavy swap usage noticeably drags down performance. Adding more swap helps slightly, but the real fix is usually adding more physical RAM (or finding a memory leak).

---

## Reading the Process List
Below the summary area is the detailed roster—one row per process, sorted by highest `%CPU` usage by default:

```text
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   1892 student   20   0  982340 145820  78200 S   4.7   1.8   1:12.44 firefox
    842 root      20   0   72180   6420   5210 S   0.3   0.1   0:02.18 sshd
   4231 student   20   0   11260   3980   3120 R   0.3   0.0   0:00.05 top
```

| Column | Meaning |
| :--- | :--- |
| **`PID`** | Process ID number |
| **`USER` (User ID)** | The process's owner |
| **`PR` / `NI`** | Priority and nice value |
| **`VIRT` / `RES` / `SHR`** | Virtual, physical (resident), and shared memory |
| **`S`** | Process state (e.g., `R` running, `S` sleeping, `D` uninterruptible sleep, `Z` zombie) |
| **`%CPU` / `%MEM`** | Percentage of CPU and memory in use |
| **`TIME+`** | Total CPU time the process has used |
| **`COMMAND`** | The command that started the process |

---

## Interactive Keys Inside `top`
`top` isn't just for watching—it's interactive. While running, press single-letter keys to sort, toggle views, or act on processes. **Keys are case-sensitive** (`P` and `p` are different).

| Key | Action |
| :--- | :--- |
| **`h`** or **`?`** | Display the help screen listing the interactive keys |
| **`P`** | Sort the process list by CPU usage (the default) |
| **`M`** | Sort the process list by memory usage |
| **`T`** | Sort the process list by cumulative CPU time |
| **`t`** | Display or hide the summary lines (tasks and CPU) |
| **`m`** | Display or hide the memory lines |
| **`1`** | Toggle between overall CPU stats and per-core stats |
| **`d`** | Change the refresh interval (in seconds) |
| **`r`** | Renice (change the priority of) a process |
| **`k`** | Kill (send a signal to) a process |
| **`q`** | Quit `top` and return to the prompt |

> 💡 **Tip: Memorize `P` and `M` First**
> They answer the two most common troubleshooting questions: *"What's eating my CPU?"* (`P`) and *"What's eating my RAM?"* (`M`). These sort keys work identically across Ubuntu, Fedora, RHEL, CentOS Stream, and openSUSE because they all ship `top` from the `procps-ng` package.

---

## Useful `top` Command-Line Launch Flags ⚡
You can also pass flags when launching `top` to filter or automate it:

```bash
# Monitor only processes owned by a specific user
$ top -u student

# Set a faster refresh interval (e.g., update every 1 second)
$ top -d 1

# Monitor only specific PIDs (comma-separated)
$ top -p 1892,842

# Batch mode: capture 1 static snapshot and save it to a log file (great for scripts)
$ top -b -n 1 > system_snapshot.txt
```

---

## Modern Alternatives to `top`
Popular alternatives like `htop`, `btop`, and `atop` offer colorful, modern interfaces with mouse support, scrolling, and graphs. They usually aren't pre-installed, so you add them via your package manager:

```bash
$ sudo apt install htop        # Ubuntu / Debian
$ sudo dnf install htop        # Fedora / RHEL / CentOS Stream
$ sudo zypper install htop     # openSUSE
```

### Quick Tricks in `htop`
*   **`F5`**: Toggle Tree View (shows parent-child hierarchy like `pstree`).
*   **`F6`**: Choose which column to sort by (`CPU%`, `MEM%`, `PID`, etc.).
*   **`F9`**: Send a kill signal to the highlighted process without typing its PID.
*   **`/`**: Search for a process by name.

### Quick Tricks in `atop`
*   **`d`**: Switch to **Disk I/O** view to see which app is hammering your drive.
*   **`m`**: Switch to **Memory** view for detailed RAM breakdowns.
*   **`c`**: Show the full command line for every running process.
*   **`g`**: Jump back to the default generic view.

### Quick Tricks in `btop`
*   **`Esc` → Options**: Open settings to swap Color Themes (Dracula, Gruvbox, Nord).
*   **`p`**: Cycle through layout presets (e.g., minimal CPU-only view or full dashboard).
*   **`f`**: Filter processes live as you type.
*   **`t`**: Toggle the tree view (just like `F5` in `htop`).

> **Why `top` Still Matters:** Despite how awesome `htop` and `btop` are, `top` has one unbeatable advantage—**it is always pre-installed** on virtually every Linux server and minimal container, with zero setup required.