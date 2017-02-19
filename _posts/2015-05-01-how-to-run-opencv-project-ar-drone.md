---
layout: post
title: How to run Cross-Compiled OpenCV project on AR Drone 
description: "This tutorial explains how to run a cross compiled OpenCV project on your AR Drone by setting up relavent environment variable."
tags: ['arm', 'linux', 'drone', 'opencv']
---

If you have followed my last three [posts](/blog/archive/), you should have:

1. [The Cross-Compiler ready](/blog/2015/04/28/how-to-cross-compile-for-ar-drone/).
2. [Cross-Compiled OpenCV libraries](/blog/2015/04/29/cross-compile-opencv-with-ffmpeg-ar-drone-arm/).
3. [Cross-Compiled OpenCV project](/blog/2015/04/30/cross-compile-opencv-project-ar-drone/).


#### USB Flash Drive with AR Drone
If your compiled libraries are too large to be stored in AR Drone's internal memory, you will need to use a USB drive. Make sure that the USB drive is formatted with fat32 filesystem. I have heard that a lot of people have problems getting AR Drone to recognize the flash drive. I have gotten lucky in this regard, both my Kingston 16 GB and a Chinese No-Name 8 GB USB flash drives have worked flawlessly with AR Drone.

After you have plugged in the USB to the Drones USB port, wait for about 15-30 seconds for the Drone's firmware to auto-mount the drive.

You can run the following simple command to see a list of file systems in a human readable format.

```bash
df -h
```


I get the following list:

```
Filesystem                Size      Used Available Use% Mounted on
ubi1:system              26.3M     14.1M     10.9M  56% /
tmp                      57.7M    688.0K     57.0M   1% /tmp
dev                      57.7M         0     57.7M   0% /dev
ubi0:factory              4.8M     76.0K      4.5M   2% /factory
ubi2:update              13.2M     28.0K     12.5M   0% /update
ubi2:data                53.5M      8.1M     42.7M  16% /data
/tmp/udev/dev/sda1        7.6G      1.8G      5.8G  23% /tmp/udev/dev/.mnt/sda1
/tmp/udev/dev/sda1        7.6G      1.8G      5.8G  23% /data/video/usb0
```

Notice the last line. It means that my 8 GB usb flash drive in mounted to `/data/video/usb0`. I can navigate to this directory and see the contents of my flash drive.

#### Configuring the Linker
You have your cross-compiled binary and the shared libraries on the USB. Now all you have to do is to configure the dynamic linker so that it can *see* the libraries on your flash drive. The dynamic linker loads and links the shared libraries from the path specified by the environment variable `$LD_LIBRARY_PATH`. So we just need to add the path to lib folder in our flash drive to `$LD_LIBRARY_PATH`.

```bash
export LD_LIBRARY_PATH=/data/video/usb0/lib
```
<div class="yellow-box">
<strong>Note: </strong>You will have to do this on evey telnet session.
</div>

#### Running the Program
Now you can simply run the program like so:

```bash
./programName
```

#### Terminating the Program
If your program is in an infinite while loop, and you want to stop the program, `Ctrl + C` will not work here. If you want to exit a program you will need to kill the process.
Get the list of running processes with

```bash
ps
```
you will get a list like the following

```bash
1176 root      2740 S    /bin/sh 
1229 root     43264 R    ./AR_embedded 
1259 root      2604 S    sleep 10 
1260 root      2740 S    /bin/sh
```
my program AR_embedded has process ID of `1229`, now that I have noted the ID of the process I want to terminate, I can go ahead and kill it

```bash
kill 1229
```
