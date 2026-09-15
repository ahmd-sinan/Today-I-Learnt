# Linux CLI: Disk Management, Partitions & Filesystems 

**Date:** 2026-09-15

Today I learned the core Linux utilities for managing physical and virtual storage devices. I explored how to list hardware blocks, monitor disk space allocation, format partitions, and safely mount or unmount filesystems into the Linux directory tree.

## 1. Storage & Disk Management Master Reference 
Unlike Windows, which uses letters (`C:`, `D:`), Linux represents raw storage drives as block devices in the `/dev` directory (e.g., `/dev/sda`). The following commands are used to manipulate, format, and monitor these blocks.

| Command / Flag | Core Purpose | Detailed Architectural Explanation | Example Usage |
| :--- | :--- | :--- | :--- |
| **`lsblk`** | **List Block Devices** | Queries the `/sys` virtual filesystem to display all attached physical hard drives, USBs, and partitions in a visual tree format. It shows the device name, size, and where it is currently mounted. | `$ lsblk` |
| **`lsblk -f`** | **Filesystem & UUID Info** | The `-f` flag extends the output to reveal the underlying Filesystem Type (e.g., `ext4`, `xfs`, `ntfs`) and the Universally Unique Identifier (UUID) for each partition. UUIDs are critical for configuring auto-mounting. | `$ lsblk -f` |
| **`df`** | **Disk Free (System Space)** | Reports the total amount of available and used disk space across all currently mounted filesystems. By default, it outputs values in archaic 1-kilobyte blocks, which are difficult to read quickly. | `$ df` |
| **`df -h`** | **Human-Readable Free Space** | The `-h` flag converts the 1K blocks into modern, human-readable units (Megabytes, Gigabytes, Terabytes). This is the standard command used to check if a server's hard drive is full. | `$ df -h` |
| **`df -hT`** | **Print Filesystem Type** | Combines human-readable output with the `-T` flag, which adds a column showing the exact architectural format of the partition (e.g., distinguishing between a physical `ext4` drive and a temporary `tmpfs` RAM drive). | `$ df -hT` |
| **`du`** | **Disk Usage (Folder Sizing)** | Recursively estimates and reports the physical file space usage of a specific directory and every single sub-directory inside it. | `$ du /var/log` |
| **`du -s`** | **Summarize Usage** | The `-s` flag stops `du` from printing a massive list of every sub-folder. It calculates the total weight of the directory and prints only one single summary line. | `$ du -s /var/log` |
| **`du -sh`** | **Summarize Human-Readable** | The ultimate disk cleanup tool. It combines the summary flag with human-readable units, instantly showing you exactly how big a specific folder is in GB or MB. | `$ du -sh /var/log` |
| **`fdisk`** *(Bonus)* | **Partition Manipulator** | A low-level, interactive command-line utility used to slice raw hard drives into smaller usable chunks (partitions) by manipulating the Master Boot Record (MBR) or GUID Partition Table (GPT). | `$ sudo fdisk /dev/sdb` |
| **`mkfs.*`** | **Make Filesystem (Format)** | The Linux equivalent of "formatting" a drive. After creating a raw partition, this command builds the actual filesystem structure (inodes, superblocks) so data can be stored. Variants include `mkfs.ext4`, `mkfs.xfs`, or `mkfs.vfat`. | `$ sudo mkfs.ext4 /dev/sdb1` |
| **`mount`** | **Attach Filesystem** | Maps a formatted physical partition (like `/dev/sdb1`) to a specific empty directory (the mount point, like `/mnt/data`) within the unified Linux `/` filesystem tree, making the data accessible. | `$ sudo mount /dev/sdb1 /mnt/data` |
| **`umount`** | **Detach Filesystem** | Safely flushes pending read/write operations from RAM to the physical disk and unlinks the partition from the filesystem tree so the drive can be safely removed without data corruption. | `$ sudo umount /mnt/data` |
| **`fsck`** | **Filesystem Consistency Check**| The "File System Check" utility. It scans for and repairs corrupt inodes, broken links, and bad sectors on a drive. **CRITICAL WARNING:** Never run `fsck` on a mounted, active partition, or it will permanently destroy data. | `$ sudo fsck /dev/sdb1` |
| **`blkid`** *(Bonus)* | **Block ID Identification** | A highly specific admin tool that instantly prints just the UUID and filesystem type of a target block device. Used strictly when writing automation scripts or configuring the `/etc/fstab` file. | `$ sudo blkid /dev/sda1` |