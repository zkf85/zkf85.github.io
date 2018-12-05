---
layout: post
comments: true
title: "How to make Bootable USB from an ISO with Command Line Tools on macOS?"
date: "2018-12-05 16:09:00"
category: [Operating System]
tag: [macOS, bash, usb-bootable-stick, ubuntu]
---

> Making things happen without any third party tools is joyful. In this article, I'd like to show how to make a bootable USB simply using built-in commands in a terminal on macOS.

![](/public/img/20181205-bootable-usb.jpg)

<!--more-->

## 1. Prepare Everything Needed
What you need:
1. A Ubuntu installer ISO file, you can download from either site below:
	
	[>> Ubuntu Official Installer Downloads Page](https://www.ubuntu.com/download/alternative-downloads)

	[>> Tsinghua Mirrors of Ubuntu Releases (extremely fast for users in China)](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/)

2. A Flash Drive that is free to use.

## 2. Clear up your USB Flash Drive (Optional)

[>> Disk Management From the Command-Line, Part 1](http://www.theinstructional.com/guides/disk-management-from-the-command-line-part-1)

### 2.1. Locate your USB disk
- List the partition of the disk:
	```bash
	$ diskutil list
	```

- Then find your usb disk info as below:
	```
	/dev/disk2 (external, physical):
	   #:                       TYPE NAME                    SIZE       IDENTIFIER
	   0:     Apple_partition_scheme                        *31.0 GB    disk2
	   1:        Apple_partition_map                         4.1 KB     disk2s1
	   2:                  Apple_HFS                         2.4 MB     disk2s2
	```

### 2.2. Erase the disk
Erasing disks from the command-line can be a dangerous process as there aren't any warnings or confirmations. One typo could lead to irreversible data loss if there's no backup to restore from. If you're not familiar with the command-line, Disk Utility is just as capable.

**To erase an entire disk**:
```bash
$ diskutil eraseDisk {filesystem} {Name_to_use} /dev/{disk_identifier}
```

Erasing a whole disk will clear any partitions and create a new, single partition, before formatting it as a volume.

- **`{filesystem}`**:

	You can specify the filesystem to format the partition in by using any that are supported. 
	To find out which filesystems you can use, enter:
	```bash
	$ diskutil listFilesystems
	```

	And you may get this:
	```
	Formattable file systems

	These file system personalities can be used for erasing and partitioning.
	When specifying a personality as a parameter to a verb, case is not considered.
	Certain common aliases (also case-insensitive) are listed below as well.

	-------------------------------------------------------------------------------
	PERSONALITY                     USER VISIBLE NAME
	-------------------------------------------------------------------------------
	APFS                            APFS
	  (or) APFSI
	Case-sensitive APFS             APFS (Case-sensitive)
	ExFAT                           ExFAT
	Free Space                      Free Space
	  (or) FREE
	MS-DOS                          MS-DOS (FAT)
	MS-DOS FAT12                    MS-DOS (FAT12)
	MS-DOS FAT16                    MS-DOS (FAT16)
	MS-DOS FAT32                    MS-DOS (FAT32)
	  (or) FAT32
	HFS+                            Mac OS Extended
	Case-sensitive HFS+             Mac OS Extended (Case-sensitive)
	  (or) HFSX
	Case-sensitive Journaled HFS+   Mac OS Extended (Case-sensitive, Journaled)
	  (or) JHFSX
	Journaled HFS+                  Mac OS Extended (Journaled)
	  (or) JHFS+
	```

- **`{Name_to_use}`**:

	This simply refers to the name of the volume that will be created. In this instance, I've just labelled the volume as "Test".

- **`{disk_identifier}`**:

	Only the primary part of the identifier (i.e. `disk1`, `disk2`, `disk3`...) is needed. 

**In my case, the actual command I used to erase the USB disk is**
```bash
$ diskutil eraseDisk fat32 KF-USB /dev/disk2
```
If sucessful, it will show information like this:
```
Started erase on disk2
Unmounting disk
Creating the partition map
Waiting for partitions to activate
Formatting disk2s2 as MS-DOS (FAT32) with name KF-USB
512 bytes per physical sector
/dev/rdisk2s2: 60161312 sectors in 1880041 FAT32 clusters (16384 bytes/cluster)
bps=512 spc=32 res=32 nft=2 mid=0xf8 spt=32 hds=255 hid=411648 drv=0x80 bsec=60190720 bspf=14688 rdcl=2 infs=1 bkbs=6
Mounting disk
Finished erase on disk2
```

Meanwhile the USB is automatically mounted to you system as well. In my case, it's mounted at `/Volumes/KF-USB`.

Now use command `diskutil list` again to show the disk partition information, you'll see the new disk's info:
```
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *31.0 GB    disk2
   1:                        EFI EFI                     209.7 MB   disk2s1
   2:       Microsoft Basic Data KF-USB                  30.8 GB    disk2s2
```

## 3. Create the Bootable  USB

[>> MacOS Terminal: Create a Bootable USB from an ISO Using “dd”](https://www.cybrary.it/0p3n/macos-terminal-create-bootable-usb-iso-using-dd/)

Assuming our flash drive is stll `disk2`, then
```bash
$ diskutil unmountDisk disk2
```

The `unmountDisk` command unmounts all volumes of the given disk drive but keeps the drive itself visible to the computer (as opposed to the eject option that disconnects it entirely)

Then, run the following command to create the bootable USB:
```bash
$ sudo dd if=/Users/kyle/Downloads/Linux.iso of=/dev/disk2 bs=8m
```
**`“sudo”`** tells the system to use root level (that is the system’s highest level) privileges to perform the following action.

**`“dd”`** is an extremely basic, but powerful block level copy command built into all Linux and Unix operating systems (MacOS is UNIX based)

**`“if”`** stands for input file (a.k.a the source file or location). In our use-case, this is the .ISO file. In MacOS, if you have a finder window open, you can drag and drop the .iso into the terminal and it will auto-fill this file path.

**`“of”`** stands for “output file” (a.k.a the destination file or location). For us, this is our USB drive, disk2. The specific path for external drives is in “/dev”, hence /dev/disk2

**`“bs”`** stands for block size. dd copies data in blocks rather than on a file by file basis (this is why it’s so fast) and this command gives you the option to set how big each block is. There is a science to the ideal block size, but I don’t know it. 8m (MegaBytes) has consistently worked well for my uses.

The command will not show any progress until it’s done, but you can press control+t for status updates. With an average computer, this takes less than 5min to complete.
Once complete:
```bash
$ diskutil eject disk2
```

The USB drive can now safely be removed. Assuming that the iso is EFI-compatible, you can reboot your mac to test it.


<br><br>***KF*** 
