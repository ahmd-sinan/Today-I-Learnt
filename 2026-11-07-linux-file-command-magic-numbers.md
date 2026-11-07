# Linux CLI: Identifying True File Types (`file`) & Magic Numbers 

**Date:** 2026-11-07

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #FileCommand #MagicNumbers #SysAdmin #Security

Today I learned how Linux actually determines what a file is. Unlike Windows, which relies heavily on file extensions, Linux looks directly into the binary contents of the file itself to determine its true identity.

## 1. The Linux Philosophy: Extensions are Cosmetic
In Windows, the OS relies heavily on extensions; a file ending in `.exe` is executed, and `.txt` is opened in Notepad. 

On Linux, a file's extension is purely cosmetic. You could easily name an executable binary program `report.txt` or a plain text file `data.exe`, and the Linux kernel would not care. Extensions are just a convenience for the human reading them.

## 2. The `file` Command & Magic Numbers
Because you cannot trust file extensions on Linux, system administrators rely on the `file` command. This utility completely ignores the filename and examines the actual contents to determine the file's true nature.

It does this by looking for **"Magic Numbers"**. A magic number is a specific, standardized byte signature located at the very beginning of a file (the header) that definitively dictates its structural format and identity. 

## 3. Real-World Examples
Here is how the `file` command exposes the true identity of different files:

**Identifying a standard executable:**
```bash
$ file /bin/ls
/bin/ls: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked...
```

**Identifying a plain text file:**
```Bash
$ file /etc/hostname
/etc/hostname: ASCII text
```

**Identifying a shell script:**
Even if the file has no `.sh` extension, file reads the `#!/bin/bash` (shebang) at the top:
```Bash
$ file backup_task
backup_task: Bourne-Again shell script, ASCII text executable
```

**Discovering a disguised file:**
```Bash
$ file report.txt
report.txt: PNG image data, 800 x 600, 8-bit/color RGB, non-interlaced
```
In this example, a file explicitly named `report.txt` was revealed to actually be a `PNG` image. The `file` command exposes what you are actually dealing with, ensuring you never accidentally execute a malicious script disguised as a text document.

**Identifying compressed archives:**
```Bash
$ file system_data.bak
system_data.bak: gzip compressed data, from Unix, original size modulo 2^32 10240
```

## Essential `file` Command Options
When writing automation scripts or doing deep-dive troubleshooting, these flags are incredibly useful:

| Option | Description |
| :--- | :--- |
| `file *` | Instantly scans and identifies the true file types of every single item inside the current directory. |
| `-b` (Brief) | Strips the filename from the output, printing only the file type. Perfect for writing bash scripts where you need to assign the output to a variable. |
| `-i` (MIME Type) | Outputs the standardized MIME type string (e.g., `text/plain; charset=us-ascii` or `image/png`) instead of the human-readable description. Highly useful for web servers and API configuration. |
| `-h` (No Dereference) | When checking a symbolic link, this forces `file` to identify the link itself, rather than identifying the target file the link points to. |

