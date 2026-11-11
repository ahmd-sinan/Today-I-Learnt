# Linux CLI: Raw Disk-to-Disk Copying (`dd`) 💽

**Date:** 2026-11-10
**Category:** Linux / System Administration
**Tags:** #Linux #CLI #dd #Storage #SysAdmin #Backups

Unlike `cp` or `rsync`, which copy files within an existing filesystem layout, `dd` operates at a much lower level. It makes direct, byte-for-byte copies of raw storage devices, partitions, or data streams. It does not care about files, folders, or formats—it strictly reads and writes raw binary data.

## 1. The Core Syntax
The `dd` command uses a unique syntax compared to other Linux commands, relying on `key=value` pairs rather than standard dash-flags (`-v`).

*   **`if=` (Input File):** The source you are reading from.
*   **`of=` (Output File):** The destination you are writing to.
*   **`bs=` (Block Size):** How many bytes `dd` reads and writes at a time (e.g., `512`, `1M`, `4M`). Larger block sizes speed up the transfer.
*   **`count=` (Count):** How many blocks to copy before stopping. If omitted, `dd` copies until it reaches the end of the input file.
*   **`status=progress`:** By default, `dd` is completely silent until it finishes. Adding this tells `dd` to print real-time transfer speeds and progress to the terminal.

## 2. The "Disk Destroyer" Warning
**Serious Warning:** `dd` copies raw data with absolutely no safety checks and no undo. Getting the source (`if=`) and destination (`of=`) backwards is catastrophic.

For example, typing:
```bash
$ sudo dd if=/dev/sda of=/dev/sdb
```

This makes an exact byte-for-byte copy of the first disk (`sda`) onto the second disk (`sdb`), completely erasing everything that was on the second disk. There is no confirmation prompt and no recovery. Because of this, `dd` is often jokingly said to stand for "disk destroyer."

**​⚠️ SysAdmin Rule:** Do not run `dd` commands like this experimentally. Always double- and triple-check the `if=` and `of=` values using tools like `lsblk` or `fdisk -l` before hitting Enter.


## 3. Real-World Practical Examples
###​ Example A: Backing Up the Master Boot Record (MBR)

​Historically, the very first 512-byte sector of a disk held the Master Boot Record (partition table) on older BIOS-based systems. You could back up just this sector using:

```bash

$ sudo dd if=/dev/sda of=sda.mbr bs=512 count=1

```

*Note: On modern UEFI systems, disks use the newer GPT partitioning scheme instead of MBR, so this particular example is mostly of historical interest.*

### ​Example B: Writing a Linux Installer to a USB Drive

​This is the most common modern use for `dd`. When you download an Ubuntu or Arch Linux `.iso` file, you write it directly to a USB stick to make it bootable.

```bash

$ sudo dd if=ubuntu-26.04-desktop.iso of=/dev/sdc bs=4M status=progress

```

*(Assuming `/dev/sdc` is the USB drive. This overwrites the entire USB stick with the installer's filesystem).*

### Example C: Securely Wiping a Drive

​If you are recycling an old hard drive, formatting it does not actually delete the data. You can use `dd` and the special `/dev/urandom` stream to overwrite every single sector on the drive with randomized garbage data.

```bash

$ sudo dd if=/dev/urandom of=/dev/sdb bs=1M status=progress

```

### Example D: Creating a Blank File of a Specific Size

`​dd` is often used to generate blank files for testing or to create swap files. By reading from the special `/dev/zero` stream, you can create a file filled with null characters.

```bash

# Creates a 1GB file (1M block size * 1024 count)
$ dd if=/dev/zero of=test_file.img bs=1M count=1024 status=progress

```