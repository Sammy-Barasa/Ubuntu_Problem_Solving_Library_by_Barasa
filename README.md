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
<details>
<summary>

## **attempt to read or write outside hd0 error**
</summary>

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
</details>

<details>
<summary>

## **mount: /mount/path: wrong fs type, bad option, bad superblock on /dev/sda3, missing codepage or helper program, or other error**
</summary>

## **mount: /mount/path: wrong fs type, bad option, bad superblock on /dev/sda3, missing codepage or helper program, or other error**

### Background

Upon upgrading from Ubuntu 23.01 to Ubuntu 23.10, I encountered a strange error when the Ubuntu desktop GUI file explorer Icon was active and I rushed to check my three partitions. The error read "wrong fs type, bad option, bad superblock on /dev/sda3, missing codepage or helper program, or other error". You can imagine how terrified I was!


### Problem: mount path error

![mount: /mount/path: wrong fs type, bad option, bad superblock on /dev/sda3, missing codepage or helper program, or other error](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m2x7roajiun3ginsea1s.png)

***Research ***   

Listing all the block devices showed two device partitions with no mount path with the following command:

```sh
lsblk
```

![Listing all the block devices](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zkktbhzlic29iughmw97.png)
navigating to the media folder to check the available directories, the two media paths were missing

```zsh
kesa@KESA-DESKTOP:/$ cd media/
kesa@KESA-DESKTOP:/media$ ls
kesa
kesa@KESA-DESKTOP:/media$ cd kesa/
kesa@KESA-DESKTOP:/media/kesa$ ls
XYZ
```

### Solution: device fail to mount, mount manually

Manually mount the device or partition. Create mount paths with the label of the partition that can be found using `lsblk -f`

![list block devices towith -f flag to see device labels](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jc0essktos9r5uaq7wwa.png)

In my case:
```sh
kesa@KESA-DESKTOP:/media/kesa$ sudo mkdir WORKSPACE
```
mount the device partition using the mount command to the newly created mount path
```sh
kesa@KESA-DESKTOP:/media/kesa$ sudo mount /dev/sda3 /media/kesa/WORKSPACE
```

![mount device command](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2z8o98psfxmmpsam7nmk.png)
```sh
kesa@KESA-DESKTOP:/media/kesa$ sudo mount /dev/sda1 /media/kesa/New\ Volume
```

change the device name and path in your scenario. Ensure you use the root user when you encounter permission issues. Check final block devices with the list block command.


![manual mount successful](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/v9lv0x8cj8vr739sjv2h.png)



[sources](https://discuss.devopscube.com/t/solved-wrong-fs-type-bad-option-bad-superblock-on-dev-sdb-error/206/4)

[sources](https://www.reddit.com/r/linux4noobs/comments/17gk04t/wrong_fs_type_bad_option_bad_superblock_on/)

[See Article Here](https://dev.to/sammybarasa/mount-mountpath-wrong-fs-type-bad-option-bad-superblock-on-devsda3-missing-codepage-or-helper-program-or-other-error-2l84)
</details>