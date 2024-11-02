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

<details>
<summary>

## **How to restore Ubuntu desktop UI After an Unexpected tty1 Boot without initial internet access**
</summary>
## How to restore Ubuntu desktop UI After an Unexpected tty1 Boot without initial internet access
### introduction
My Ubuntu 24.04 desktop unexpectedly boot into tty1 presenting a shell interface to interact with on bootup.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zsb9lm5908q561ifotjo.jpeg)
The above image shows the tty1 output.   

Sometimes this happens when one accidentally removes critical components such as the desktop environment or Python. In my case, I had installed Python while trying to solve other issues I encountered with Python3.

### Step 1: Login to tty1 {tty(n)} with your username and password   
First, log in with your username and password to access the system. This way we can run commands and possibly recover the desktop UI.   

### Step 2: Confirm you have access to the internet
Once logged in, you have to confirm if you have an internet connection. The easiest way to do this is to run the `ping` command to the internet, and select any address or domain name available.
```sh
ping google.com
```
If you have an internet connection you will receive packets from the address or domain. Therefore you can skip to [step 4](#Step-4--Check-and-repair-packages).
If you cant receive packets, don't worry, we've got you. You can connect to a LAN or move closer to a WLAN connection and proceed to establish a connection to the internet.

### Step 3: Recover an internet connection
In linux, everything can be represented using a file format and this also includes all the network interfaces on the Network Interface Card(NIC). NIC has the ethernet LAN interface and the Wireless LAN (Wi-Fi) interface. These interfaces are stored in the  `/sys/class/net` file. so we proceed to list them.
```sh
ls -l /sys/class/net
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1jojs27ln7ec5v4cd6ky.jpeg)
The LAN interface starts with an **e** while the WLAN interface starts with **w**. Note down the interface names.
Connecting the interfaces has several ways.
- use network management interface command`nmtui`   

If `nmtui` command is available you can easily get the connection established via the interface that pops up from running running the `nmtui`command. 
```sh
nmtui
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uzv2e9dx4vhjm3fj04a4.jpeg)
- Modify configs   

In somecases `nmtui` is not available and even `netplan` command was not available due to damaged packages and missing packages. As was in my case. Here we have to modify configs. For LAN or wired connection, we can establish a connection without modifying configs while for Wireless connection, we have to modify configs.
You can confirm the state of the link or interfaces whether they are up or down using the `ip a` command
```sh
ip a
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q4bigtlol5xnz02qun7x.jpeg)

As highlighted here the ethernet and wireless interfaces might be in down state.
#### wired connection
Connect the LAN cable from your router to the LAN port of your computer
```zsh
sudo ifconfig eth0 up
```
change ***eth0*** to your interface label. The interface will now show up on `ifconfig` command.
If you are using DHCP on your router then
```sh
sudo dhclient eth0
```
The LAN link will obtain an IP address and running simple ping command will return packets from the internet. If you have to assign an IP address you can use the following command.
```sh
ifconfig eth0 192.168.0.1 netmask 255.255.255.0 up
route add default gw GATEWAY-IP eth0
```
If it has worked up to here you can proceed to [step 4](#Step-4--Check-and-repair-packages). If the wired connection hasn't worked, lets try the wireless connection route.
#### wireless connection
For establishing a wireless connection from recovery terminal, checkout the following reference [Reference 1](#my-link), [Reference 2](#my-link2)
<h2 id="Step-4--Check-and-repair-packages"> Step 4: Check and repair packages</h2>
Update packages  and that there are no broken packages since we now have an internet connection.
```sh
sudo apt-get update
sudo apt-get upgrade
sudo apt-get --fix-broken install
```
let all these run and install
## Step 5: confirm dgm or dgm3
Confirm the display manager is installed or not
## Step 6: Install ubuntu-desktop
Now reinstall the Ubuntu desktop UI 
```sh
sudo apt-get install ubuntu-desktop
```
## Step 7: Install and configure the display manager dgm3
```sh
sudo apt-get install gdm3
```
Properly configure the display manager by running the following command
```sh
sudo dpkg-reconfigure gdm3
```
## Step 8: Reboot 
Reboot system to apply changes
```sh
sudo reboot
```
Upon bootup, your Ubuntu UI built on GNOME desktopenvironment should now pop up.

### Conclusion
Thank you. Hope this helps you recover your Ubuntu desktop when the internet connection is not available. When any other issues arise, you can proceed to trouble shoot further. It is important to back up configuration files to avoid loosing initial state during troubleshooting.

### References
1. [Starting network from Ubuntu recovery](https://serverfault.com/questions/21475/starting-network-connection-from-ubuntu-recovery)

2. <a id="my-link" href="https://askubuntu.com/questions/1249160/connecting-to-personal-wifi-on-ubuntu-server-20-04">Wireless connection in ubuntu terminal</a>

3. <a id="my-link2" href="https://linuxconfig.org/ubuntu-20-04-connect-to-wifi-from-command-line">Wireless connection by editing `/etc/netplan/`</a>

4. [Restoring the Ubuntu UI After an Unexpected tty1 Boot](https://medium.com/@elysiumceleste/restoring-the-ubuntu-ui-after-an-unexpected-tty1-boot-9f1042e03139)

</details>

<details>
<summary>

## **FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory on Digital OceanDroplet**
</summary>
## Introduction
A memory Heap error occurred on Digital ocean droplet while trying to build a react app during deployment. I do development primarily in Django and front end using ReactJs. During deployment, I have to do the final build for React to produce a compiled static file that can be served by Django. The memory error occurred during the build when running `yarn build`

```sh
<--- Last few GCs --->

[139947:0x6613440]    32615 ms: Mark-Compact 244.4 (258.4) -> 243.5 (258.6) MB, 858.51 / 0.00 ms  (average mu = 0.116, current mu = 0.034) allocation failure; scavenge might not succeed
[139947:0x6613440]    33392 ms: Mark-Compact 244.6 (258.6) -> 243.7 (258.9) MB, 711.03 / 0.00 ms  (average mu = 0.101, current mu = 0.085) allocation failure; scavenge might not succeed


<--- JS stacktrace --->

FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
----- Native stack trace -----

 1: 0xb86ecf node::OOMErrorHandler(char const*, v8::OOMDetails const&) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 2: 0xef74d0 v8::Utils::ReportOOMFailure(v8::internal::Isolate*, char const*, v8::OOMDetails const&) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 3: 0xef77b7 v8::internal::V8::FatalProcessOutOfMemory(v8::internal::Isolate*, char const*, v8::OOMDetails const&) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 4: 0x1109355  [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 5: 0x11098e4 v8::internal::Heap::RecomputeLimits(v8::internal::GarbageCollector) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 6: 0x11207d4 v8::internal::Heap::PerformGarbageCollection(v8::internal::GarbageCollector, v8::internal::GarbageCollectionReason, char const*) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 7: 0x1120fec v8::internal::Heap::CollectGarbage(v8::internal::AllocationSpace, v8::internal::GarbageCollectionReason, v8::GCCallbackFlags) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 8: 0x10f72f1 v8::internal::HeapAllocator::AllocateRawWithLightRetrySlowPath(int, v8::internal::AllocationType, v8::internal::AllocationOrigin, v8::internal::AllocationAlignment) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
 9: 0x10f8485 v8::internal::HeapAllocator::AllocateRawWithRetryOrFailSlowPath(int, v8::internal::AllocationType, v8::internal::AllocationOrigin, v8::internal::AllocationAlignment) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
10: 0x10d5ad6 v8::internal::Factory::NewFillerObject(int, v8::internal::AllocationAlignment, v8::internal::AllocationType, v8::internal::AllocationOrigin) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
11: 0x1531906 v8::internal::Runtime_AllocateInYoungGeneration(int, unsigned long*, v8::internal::Isolate*) [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
12: 0x196aef6  [/home/aislackbot/.nvm/versions/node/v20.17.0/bin/node]
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```
Build error
```sh

Creating an optimized production build...
The build failed because the process exited too early. This probably means the system ran out of memory or someone called kill -9 on the process.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
yarn run v1.22.22
$ react-scripts build
Creating an optimized production build...
The build failed because the process exited too early. This probably means the system ran out of memory or someone called kill -9 on the process.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```
## Resolution
The above error is as a result of the Node.js installation having the heap memory restricted. While executing the build process, this heap memory restriction is exceeded.   

Thankfully this value can be adjusted through `NODE_OPTIONS` and be exported to the linux system where Node.js is installed.

In my case I adjusted the value to `4096` with the following command:
```sh
export NODE_OPTIONS="--max-old-space-size=4096"
```
Running `yarn build` then works afterwords
```sh
export NODE_OPTIONS="--max-old-space-size=4096"
yarn build
```   

`--max-old-space-size` Sets the max memory size of V8's old memory section. According to NodeJs documentation:   
``` 
As memory consumption approaches the limit, V8 will spend more time on garbage collection in an effort to free unused memory.
```
## Research
1. [NodeJs Documentation Reference](https://nodejs.org/api/cli.html)  
2. [About --max-old-space-size=SIZE in node-options](!https://nodejs.org/api/cli.html#--max-old-space-sizesize-in-mib)

### [Link to article](https://dev.to/sammybarasa/fatal-error-ineffective-mark-compacts-near-heap-limit-allocation-failed-javascript-heap-out-of-memory-on-digital-oceandroplet-4m89/edit)
</details>