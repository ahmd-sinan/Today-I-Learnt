# Linux CLI: Background, Foreground & Job Control 

**Date:** 2026-11-24

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #Processes #JobControl #Bash #SysAdmin

Today I learned how to juggle multiple tasks within a single terminal session using Linux job control. Understanding how to send long-running commands to the background, pause them, and bring them back to the foreground is essential for efficient terminal workflows.

## Foreground vs. Background Processes

Linux lets you run **jobs** (any command launched from a terminal) in either the foreground or the background.

- **Foreground Jobs:** Run directly in the shell. While one is running, your terminal is locked and you cannot enter other commands until it finishes. This is fine for quick commands, but impractical for tasks that take minutes or hours.
- **Background Jobs:** Detach from the shell so they run quietly on their own, immediately returning your prompt so you can keep working.

### Starting a Job in the Background (`&`)

Append an ampersand (`&`) to the end of any command to launch it directly in the background:

```bash
$ updatedb &
[1] 4820
```

- `[1]` — The Job ID assigned by the current shell.
- `4820` — The system-wide Process ID (PID).

> **📝 Note on Priority:**
> Using `&` only detaches the job from the shell; it does not lower the job's CPU priority on its own. If you want a heavy background job to stay out of the way of your interactive work, combine it with `nice`:
>
> ```bash
> $ nice updatedb &
> ```

## Controlling Active Jobs (Ctrl-Z, Ctrl-C, bg, fg)

If you already started a command in the foreground and realize it is taking too long, you can take control of it using keyboard shortcuts and built-in shell commands:

| Shortcut / Command | Action |
|---|---|
| `Ctrl-Z` | Suspends (pauses) the job. It freezes in memory and stops consuming CPU, but is not terminated. |
| `Ctrl-C` | Terminates (kills) the job outright. |
| `bg` | Resumes a suspended job in the background. |
| `fg` | Brings a background or suspended job back to the foreground. |

### Complete Workflow Example

```bash
$ sleep 500
^Z                        # Ctrl-Z suspends it
[1]+  Stopped                 sleep 500
$ bg %1                   # Resume job 1 in the background
[1]+ sleep 500 &
$ fg %1                   # Bring job 1 back to the foreground
sleep 500
```

> **💡 Tip:**
> If you only have one background or suspended job, you don't need to type the job number (`%1`). Running `fg` or `bg` by itself automatically applies to the current default job (marked with a `+` sign in the `jobs` output).

## Managing Jobs (`jobs`)

The `jobs` command lists all jobs currently suspended or running in the background of your active terminal, showing each job's ID, state, and command name.

```bash
$ jobs
[1]-  Running                 updatedb &
[2]+  Stopped                 sleep 500
```

### Viewing Jobs with PIDs (`jobs -l`)

Adding the `-l` (long) flag displays the exact same information plus the PID of each job, which is useful if you need to `kill` or `renice` it:

```bash
$ jobs -l
[1]- 4820 Running                 updatedb &
[2]+ 4855 Stopped                 sleep 500
```

## Terminal Session Dependency

Keep in mind that background jobs are strictly tied to the terminal session that started them. If you log off or close that terminal window, jobs in a new session will not list them (and closing the terminal usually sends a hangup signal that terminates them).

> **🛡️ SysAdmin Pro-Tip (`nohup`):**
> If you need a long-running job on a remote server to survive even after you close your terminal or disconnect from SSH, launch it with `nohup` (no hangup):
>
> ```bash
> $ nohup ./long-backup-script.sh &
> ```   