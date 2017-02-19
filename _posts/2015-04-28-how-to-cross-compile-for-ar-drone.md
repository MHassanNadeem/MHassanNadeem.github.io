---
layout: post
title: How to Cross Compile C/C++ application for AR Drone
description: "How install cross compiler and cross compile a C/C++ program for AR Drone"
tags: ['arm', 'linux', 'drone']
---

#### Installing Prerequisites
First we need to download all the prerequisite packages that we will be needing in the series of these posts.

```
sudo apt-get update
sudo apt-get install build-essential linux-libc-dev wget bzip2 ncurses-dev git cmake cmake-curses-gui cmake-qt-gui config-manager wput
```

#### Downloading Compiler
Create a new folder named 'ARM_Files' in home folder. We will keep all toolchain and cross compiled binaries in this folder throughout the series of tutorials.

```
mkdir ~/ARM_Files
cd ~/ARM_Files
wget https://sourcery.mentor.com/public/gnu_toolchain/arm-none-linux-gnueabi/arm-2009q3-67-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2
tar jxf arm-2009q3-67-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2
```

[This cross-compiler is meant for 32-bit systems, if you are running a 64-bit linux you will have to install the following additional packages to be able to use the compiler](http://askubuntu.com/questions/297151/how-to-run-32-bit-programs-on-a-64-bit-system).

```
sudo apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0
```

#### Configuring System Paths
Create a new bash script to set up the environment variables that will help the system find the compiler.

```
gedit ~/ARM_Files/setupCrossCompiler
```

Paste the following bash script into the file. All this script does is add the recently downloaded compiler to the system path so that we can use it latter and define a new variable called CCPREFIX.

```bash
echo "Setting up the Cross Compiler Environment"

# Path to bin directory of the compiler
export PATH="/home/$USER/ARM_Files/arm-2009q3/bin":$PATH

# prefix of all the tools in a toolchain
export CCPREFIX="/home/$USER/ARM_Files/arm-2009q3/bin/arm-none-linux-gnueabi-"
```

Now make the script executable

```
chmod +x ~/ARM_Files/setupCrossCompiler
```

Add the script to .bashrc so that the cross compilation environment is set up every time you open a terminal.

```
echo "source /home/$USER/ARM_Files/setupCrossCompiler" >> ~/.bashrc
```

Close and Reopen the terminal, if you see a message in the terminal saying "Setting up the Cross Compiler Environment" then your cross compilation environment is ready and you can proceed to the next step.

#### Writing a simple C Program
We test our cross compilation setup by compiling a simple C/C++ program. Add the following hello word code to a file *hello.c*.

```c
#include <stdio.h>
int main(){
 printf("Hello Drone\n");
 return 0;
}
```

Compile and run the program on native system to test the code.

```
gcc hello.c -o PC_hello
./PC_hello
```
If this prints out *Hello Drone* on your terminal you can go ahead the compile the same code for AR Drone.

```
arm-none-linux-gnueabi-gcc hello.c -o AR_hello
```


#### Transfer the file to AR Drone
Now that you have the compiled AR Drone object file you can transfer the file to AR Drone via *ftp* and run it using *telnet*. Make sure you are in the directory of *hello.c* file and run the following commands. Just press enter when prompted for passwords.

<div class="yellow-box">
<strong>Note: </strong> Make sure that you are connected to the AR Drone's Wifi.
</div>

```bash
ftp 192.168.1.1
put AR_hello
exit
telnet 192.168.1.1
cd /data/video
chmod +x AR_hello
./AR_hello
```

Congratulations! You just ran your first cross-compiled project on AR Drone.

For more advanced projects like [cross compiling OpenCV libraries]({{ site.baseurl }}/blog/2015/04/29/cross-compile-opencv-with-ffmpeg-ar-drone-arm/) and [cross compiling OpenCV Projects]({{ site.baseurl }}/blog/2015/04/30/cross-compile-opencv-project-ar-drone/), see my [other posts]({{ site.baseurl }}/blog/archive.html).
