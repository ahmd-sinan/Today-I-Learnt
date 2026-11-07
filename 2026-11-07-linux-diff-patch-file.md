# Linux CLI: Comparing Text (`diff`), Patching, & Identifying Files (`file`) 

**Date:** 2026-11-07

**Category:** Linux / System Administration
**Tags:** #Linux #CLI #Diff #Patch #File #SysAdmin

As you manage Linux systems, a handful of questions come up repeatedly: *How is this version of a configuration file different from the backup? How do developers share code changes? What kind of file is this, really?* 

This document covers the everyday utilities used to compare text, apply patches, and identify the true nature of a file regardless of its cosmetic name.

## Comparing Text Files (`diff`)
The `diff` command is the standard UNIX tool for comparing two text files line-by-line. It pinpoints exactly what changed between them, which is critical when verifying changes made to a configuration file (e.g., `/etc/ssh/sshd_config` vs `/etc/ssh/sshd_config.bak`).

### Standard `diff` Output
By default, `diff` prints only the lines that differ using a compact, instructional notation. It uses `<` to denote lines from the first file and `>` to denote lines from the second file.

```bash
$ diff old_config.txt new_config.txt
2c2
< Port 22
---
> Port 2222
```

*Translation*: At line 2, the old file had "*Port 22*", but it was changed to "*Port 2222*" in the new file.

### Essential `diff` Options
| Option | Description | 
| :--- | :--- |
| `-u` | Shows differences in unified format, the compact, widely used standard you'll see in modern tools and patches. |
| `-c` | Shows differences in the older context format, with several lines of surrounding context. |
| `-r` | Compares entire directories recursively, including all subdirectories (useful for checking if two project folders have drifted apart). | 
| `-i` | Ignores differences in uppercase vs. lowercase letters. |
| `-w` | Ignores differences in spaces and tabs (whitespace). | 
| `-q` | Reports only whether the files differ, without listing the actual textual differences. |

**Example: Recursive Directory Comparison**

If you want to know if two entire folders of source code are identical without seeing every single line of code that changed, combine `-r` (recursive) and `-q` (quiet):

```Bash
$ diff -rq project-v1/ project-v2/
Files project-v1/main.c and project-v2/main.c differ
Only in project-v2/: new_feature.c
```

### The Unified Format (`-u`)
In professional environments, you will almost exclusively use the `-u` flag. It replaces the `<` and `>` symbols with `+` (added) and `-` (removed), alongside lines of unchanged context. This is exactly how git diff displays changes!

```Bash
$ diff -u old_config.txt new_config.txt
--- old_config.txt
+++ new_config.txt
@@ -1,3 +1,3 @@
 ListenAddress 0.0.0.0
-Port 22
+Port 2222
 PermitRootLogin no
```

> **⚠️ Text vs. Binary Comparison:**
> `diff` is explicitly designed for human-readable text files. To compare binary files (like compiled programs or images), use the `cmp` command instead. `cmp` simply reports the exact byte position where two files first diverge.

### Graphical `diff` Alternatives
If you prefer a visual, side-by-side comparison with color highlighting (similar to a Git merge view), you can use GUI-based tools available on most Linux desktop environments:

* `meld`
* `vimdiff` (Terminal-based side-by-side using Vim)
* `diffuse`

## Patches and `diff3`
In the Linux and open-source development world, if you change 5 lines in a 10,000-line source code file, it is wildly inefficient to send the entire new file to your team. Instead, you share a Patch.

### The `patch` Workflow
A patch is a tiny file containing only the mathematical differences between the old file and the new file.

#### Step 1: Create the Patch
You generate a patch file by redirecting the output of a unified `diff` into a `.patch` file.

```Bash
$ diff -u original.c updated.c > fix.patch
```

#### Step 2: Apply the Patch
The `patch` command reads this difference file and automatically injects those exact changes into an older file to bring it up to date. This is the traditional, hyper-efficient method for distributing kernel updates.

```Bash
$ patch original.c < fix.patch
patching file original.c
```

#### Step 3: Reverse a Patch (Undo)
If a patch breaks your system, you can mathematically reverse it using the `-R` flag, which removes the injected code and restores the file to its previous state.

```Bash
$ patch -R original.c < fix.patch
patching file original.c
```

### Three-Way Merges (`diff3`)
When two developers independently edit the exact same original file, you must figure out how their respective versions diverged.

```Bash
$ diff3 MY-FILE COMMON-FILE YOUR-FILE
```
`diff3` compares three files at once to show how each person's version departed from the common starting point. This specific logic is the mathematical foundation of modern collaboration tools like `git merge`.

