# Linux CLI: Scheduling Future Tasks (`at`, `cron`, `anacron`, `sleep`) 

**Date:** 2026-11-28

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #Cron #Crontab #At #Anacron #Sleep #Automation #SysAdmin

Sometimes you need a task to run later—maybe once at a specific time when you won't be at the machine, or on a regular repeating schedule. Automating routine jobs like backups, log cleanups, and system updates without anyone needing to log in is what makes Linux such a powerhouse.

Linux offers four core tools for this, each suited to a different need:
*   **`at`** — For a single, one-off job at a future time.
*   **`cron`** — For jobs that repeat on a fixed clock schedule.
*   **`anacron`** — For catching up missed periodic jobs on machines that aren't always powered on.
*   **`sleep`** — For pausing a task or script for a set duration.

---

## Running a One-Off Task: `at`
Suppose you need to run a task on a particular day in the future, but you know you'll be away from the machine. The `at` utility runs a non-interactive command **once**, at a specific time you choose.

### Scheduling a Job with `at`
```bash
$ at 10:00 AM Jul 15
warning: commands will be executed using /bin/sh
at> /usr/local/bin/backup.sh
at> <EOT>          # press Ctrl-D to save and finish
job 3 at Tue Jul 15 10:00:00 2026
```

**More Handy `at` Time Formats:**
```bash
$ at now + 30 minutes        # Run 30 minutes from right now
$ at midnight                # Run at 00:00 tonight
$ at 3:00 PM tomorrow        # Run tomorrow afternoon
$ at 09:00 Monday            # Run next Monday morning
```

### Managing Pending `at` Jobs
*   **`atq`** (or `at -l`): Lists all pending jobs waiting in the queue along with their job numbers.
*   **`atrm <job_number>`** (or `at -d <job_number>`): Cancels and removes a scheduled job (e.g., `atrm 3`).
*   **`at -c <job_number>`**: Displays the exact commands and environment that will run for that job.

> 📝 **Note on the `atd` Service:**
> `at` relies on a background daemon called `atd` to execute queued jobs. On some systems (including Ubuntu 24.04), `at` isn't installed by default, or `atd` may not be running.
> ```bash
> $ sudo apt install at                 # Install on Ubuntu/Debian
> $ sudo systemctl enable --now atd     # Start and enable the daemon
> ```

---

## Running Recurring Tasks: `cron`
For jobs that need to run repeatedly on a schedule (every night, every Monday, first of the month), the classic tool is **`cron`**. It is a background daemon that checks the system clock every minute and launches jobs when their scheduled moment arrives.

`cron` reads jobs from:
*   System-wide files: `/etc/crontab` and `/etc/cron.d/`
*   Each user's personal **crontab** (managed via the `crontab` command).

### Essential `crontab` Commands
| Command | Action | Description |
| :--- | :--- | :--- |
| **`crontab -e`** | Edit | Opens your personal crontab in your default `$EDITOR` (`nano`/`vim`) to add or modify jobs. |
| **`crontab -l`** | List | Displays all currently scheduled cron jobs for your user. |
| **`crontab -r`** | Remove | **Deletes your entire crontab** without confirmation. (Use carefully!) |
| **`crontab -ri`** | Remove (Interactive) | Prompts for confirmation (`y/n`) before deleting your crontab. |
| **`sudo crontab -u <user> -l`** | Specific User | Lists (or edits with `-e`) another user's crontab (requires `sudo`). |

> ⚠️ **Safety Habit:**
> On a standard keyboard, `e` and `r` are right next to each other. Accidentally typing `crontab -r` instead of `crontab -e` wipes your entire schedule! Always back up your crontab before major edits:
> ```bash
> $ crontab -l > crontab_backup.txt
> ```

### Understanding the 5-Field Crontab Syntax
Each job is written on a single line as **five time fields** followed by the **command** to run:

```text
*  *  *  *  *  /path/to/command
│  │  │  │  │
│  │  │  │  └── Day of week (0 - 7)
│  │  │  └───── Month (1 - 12)
│  │  └──────── Day of month (1 - 31)
│  └─────────── Hour (0 - 23)
└────────────── Minute (0 - 59)
```

| Field | Description | Values |
| :--- | :--- | :--- |
| **MIN** | Minute | `0 - 59` |
| **HOUR** | Hour | `0 - 23` |
| **DOM** | Day of month | `1 - 31` |
| **MON** | Month | `1 - 12` |
| **DOW** | Day of week | `0 - 7` (`0` **and** `7` = Sunday) |
| **CMD** | Command | Any command to run |

### Special Operators & Shortcuts
*   **`*` (Asterisk):** Means "every" (every minute, every hour, etc.).
*   **`,` (Comma):** Specifies a list of values (e.g., `1,15` = 1st and 15th).
*   **`-` (Hyphen):** Specifies a range (e.g., `1-5` in DOW = Monday through Friday).
*   **`/` (Slash):** Specifies step intervals (e.g., `*/15` in MIN = every 15 minutes).
*   **Shorthand Strings:** `@reboot` (run once at startup), `@daily` (`0 0 * * *`), `@weekly` (`0 0 * * 0`), `@monthly` (`0 0 1 * *`).

### Practical `crontab` Examples
```cron
# 1. Run script.sh every single minute of every day
* * * * * /usr/local/bin/script.sh

# 2. Run a full backup at 8:30 AM on June 10 (regardless of day of week)
30 08 10 06 * /home/sysadmin/full-backup

# 3. Run a health check every 15 minutes
*/15 * * * * /usr/local/bin/check_disk.sh

# 4. Run a cleanup script every weekday (Mon-Fri) at 11:30 PM
30 23 * * 1-5 /usr/local/bin/cleanup.sh

# 5. Start a monitoring script automatically every time the server boots
@reboot /home/student/start_monitor.sh
```

> 💡 **Tip:**
> The five-field syntax is compact and easy to mess up. Use **[crontab.guru](https://crontab.guru/)** to type an expression and instantly read in plain English when it will run.

> 📌 **Service Names Across Distributions:**
> *   **Ubuntu / Debian:** The service is called `cron` and is installed and running by default (`systemctl status cron`).
> *   **RHEL / CentOS Stream / Fedora / openSUSE:** The implementation is `cronie` and the service is called `crond` (`systemctl status crond`). On minimal installs, install and enable it with:
>     ```bash
>     $ sudo dnf install cronie              # Fedora / RHEL (or sudo zypper install cronie)
>     $ sudo systemctl enable --now crond
>     ```

---

## When the Machine Isn't Always On: `anacron`
`cron` has one built-in assumption: **that the machine is powered on at the scheduled time.** If a laptop or desktop is turned off when a job is due, `cron` skips it completely and won't run it later.

**`anacron`** fills that gap. It is a companion to `cron` designed for periodic jobs (daily, weekly, or monthly) rather than exact clock times. If a scheduled run was missed because the computer was off, `anacron` catches up and runs the job in a staggered fashion once the machine powers back on.

*   **Configuration file:** `/etc/anacrontab`
*   Behind the scenes, `anacron` works alongside the `cron` directories (`/etc/cron.daily`, `/etc/cron.weekly`, `/etc/cron.monthly`).

> **A Note on Modern Systems (`systemd` Timers):**
> On modern `systemd`-based distributions (Ubuntu 24.04, RHEL 8/9, CentOS Stream, openSUSE), **`systemd` timers** exist as a modern alternative to `cron`/`anacron`. They add built-in logging (viewable with `journalctl`) and automatic catch-up of missed runs (`Persistent=true`). However, `cron` remains fully supported and is still the simplest, most portable choice for everyday scheduling.

---

## Pausing a Task: `sleep`
Sometimes a command doesn't need to run at a specific clock time—it just needs to **wait** before continuing (for example, waiting for a busy backup device or retrying a network connection).

`sleep` suspends execution for at least the duration you specify, then resumes automatically (unless interrupted by a signal).

### Syntax
```bash
$ sleep NUMBER[SUFFIX]
```

**Supported Suffixes:**
*   **`s`** — seconds (the default if you omit the suffix)
*   **`m`** — minutes
*   **`h`** — hours
*   **`d`** — days

### Practical `sleep` Examples
```bash
# Pause for 30 seconds
$ sleep 30

# Pause for 5 minutes
$ sleep 5m

# Chain commands: wait 10 seconds, then print a reminder
$ sleep 10s && echo "Time is up!"

# Retry loop in a script: wait 5 seconds between pings
$ while ! ping -c 1 google.com; do sleep 5; done
```

> **`sleep` vs. `at`:**
> *   **`sleep`** delays execution for a **set duration** (*"wait 10 minutes, then continue"*).
> *   **`at`** starts execution at a **specific clock time** (*"run this at 3:00 AM Friday"*).

---

## Job Scheduling at a Glance
Match the tool to the shape of the task—whether it runs once or repeats, needs an exact clock time, or just needs a pause:

| Utility | Best Used For | Example Scenario |
| :--- | :--- | :--- |
| **`at`** | Running a single, non-repeating command at a specific future time. | Running a script at 3:00 PM today while you're away from your desk. |
| **`cron`** | Running repeating jobs on a fixed, clock-based schedule. | Running a database backup every Sunday at midnight. |
| **`anacron`** | Catching up missed periodic jobs (daily/weekly/monthly) on systems that get turned off. | Making sure weekly log rotation still runs on a workstation after it powers back on. |
| **`sleep`** | Pausing a script or command for a set time interval. | Waiting 30 seconds before retrying a network connection. |
