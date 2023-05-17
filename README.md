# Ubuntu_Problem_Solving_Library_by_Barasa
Ubuntu Commands and Troubleshooting Solutions by Sammy Barasa. A comprehensive guide to Ubuntu CLI and desktop, based on personal experience and discovery. Disclaimer: results may vary. Tips, fixes, and solutions to common problems encountered by new ubuntu users

# Video updates

# Structure of posts

```
.
├── issue_structure
│   ├── Problem
│   ├── Research
│   └── Solution

```

# Posts


## **attempt to read or write outside hd0 error**

### Ubuntu does not find partition with boot file on start up.
I have several partitions on my drive and the latest partition is sda4 and not zero, so I was getting `attempt to read or write outside hd0`
 I was directed to `grub ui`

### access grib or grub rescue terminal on start up and set the partition that has the bootfile

from the grub/grub rescue terminal ls the drives
```
grub > ls
```
typical result is: `(hd0) (hd0, msdos9)`

check each drive to find the one with bootfiles using
```
grub> ls (hd0,msdos9)
```
set the root drive using the following command and the drive with the boot files
```
grub rescue > set root=(hd0,msdos9)
```

set the prefix with the following commands

```
grub rescue > set prefix=(hd0,msdos9)/boot/grub
grub rescue > insmod normal
grub rescue > normal
```
On start up ubuntu will default to checking for the bootfile in the set  partition

[sources](https://askubuntu.com/questions/397485/what-to-do-when-i-get-an-attempt-to-read-or-write-outside-of-disk-hd0-error)