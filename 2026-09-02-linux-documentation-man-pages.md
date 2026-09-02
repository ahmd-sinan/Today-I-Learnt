# Linux CLI: System Documentation & The `man` Pages 

**Date:** 2026-09-02

Today I learned how to navigate and utilize the built-in Linux documentation systems. Consulting help documentation is a daily necessity for system administrators to verify command options, system calls, and configuration syntax. 

## The Architecture of Linux Help Systems 
Because Linux is assembled from a wide range of software sources, documentation lives in multiple places. The four main sources of Linux documentation are:
*   **The `man` pages:** The classic and most widely used reference manual covering commands, config files, and programming interfaces.
*   **GNU Info:** A more structured, cross-linked documentation system used predominantly by GNU tools.
*   **Command Help (`--help`):** Quick, built-in reminders of a command's usage available directly at the terminal prompt.
*   **Distribution-specific documentation:** Guides provided by specific OS creators (e.g., Ubuntu Documentation or Gentoo Handbook) covering distro-specific features.

## The `man` Command Basics & Navigation 
The `man` (manual) system dates back to the early 1970s UNIX environments. It searches, formats, and displays reference material. Because manuals contain extensive detail, the output is piped through a pager program (usually `less`) so you can read it one screen at a time.

*   **Scroll:** Use the Up/Down arrow keys or the Spacebar to move through the document.
*   **Search:** Type `/` followed by your keyword and press Enter. Press `n` to jump to the next matching result.
*   **Quit:** Press `q` to exit the pager and return to the terminal prompt.

## Searching for Commands (`-f` and `-k`) 
When you need to find a command or related documentation, `man` includes built-in search helpers that dictate how you query the system. The default search order for these queries is defined in configuration files like `/etc/manpath.config` or `/etc/man_db.conf`.

*   **`man -f <topic>`:** Lists available pages that match a name exactly. This functions identically to the `whatis` command. For example, `man -f sysctl` will show exact matches for runtime kernel parameters.
*   **`man -k <keyword>`:** Searches page descriptions for a keyword, surfacing related pages even if the keyword isn't the exact command name. This functions identically to the `apropos` command. For example, `man -k sysctl` retrieves a broader list including configuration files and systemd services.

## Manual Sections (The 9 Chapters) 
The `man` pages are strictly organized into numbered sections (1 through 9). This is critical because multiple tools or system calls might share the exact same name. 

For example, typing `man 7 socket` opens the Linux Programmer's Manual for the socket interface, whereas `man 2 socket` opens the manual for creating an endpoint for communication.

| Section | Contents | Examples |
| :--- | :--- | :--- |
| **1** | Executable programs and shell commands | `ls`, `cp`, `grep` |
| **2** | System calls (functions provided by the kernel) | `open()`, `read()`, `write()` |
| **3** | Library calls (functions in program libraries) | `printf()`, `malloc()` |
| **4** | Special files | Device files found in `/dev`, such as `/dev/null` |
| **5** | File formats and conventions | Configuration files like `/etc/fstab`, file formats like `crontab(5)` |
| **6** | Games | Games and screensavers |
| **7** | Miscellaneous | Conventions, macro packages, standards |
| **8** | System administration commands | Privileged commands used by root, such as `fdisk` and `mount` |
| **9** | Kernel routines | Non-standard internal kernel interfaces (inconsistently populated across distros) |

### Code Implementation Example:
```bash
# Target a specific section to avoid naming collisions
$ man 2 socket

# Display every page matching a given name, one after another across all sections
$ man -a socket
```
