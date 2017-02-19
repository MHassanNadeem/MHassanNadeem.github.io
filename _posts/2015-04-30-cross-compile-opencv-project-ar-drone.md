---
layout: post
title: How to Cross-Compile OpenCV project for AR Drone (ARM Processor)
description: "How to cross-compile a sample OpenCV project so that I can run on AR Drone 2.0"
tags: ['arm', 'linux', 'drone', 'opencv']
---

You have already [cross compiled OpenCV]({{ site.baseurl }}/blog/2015/04/29/cross-compile-opencv-with-ffmpeg-ar-drone-arm/) in your *home/$USER/ARM_INSTALL* directory. All you need to do now is to specify the include directory of the header files and path to shared libraries to the linker.

You can do this by using the following compiler options:

- **-I** option to *"add the directory dir to the list of directories to be searched for header files. Directories named by -I are searched before the standard system include directories"*

- **-L** option to *"specify directory of linker files"*

- **-l** option to *"search the named library when linking"*


#### Creating a Test Project
Lets create a simple OpenCV project that takes an image and saves a grey-scale version of that image to disk.

```bash
mkdir ~/testProject
cd ~/testProject
gedit imgToGrey.cpp
```
paste the following code into the file.
<script src="https://gist.github.com/MHassanNadeem/2ec00cd9ae1502e8321e.js?file=imgToGrey.cpp"></script>

#### Compiling for Host Machine
If you have OpenCV installed on your host computer, it would be good idea to compile and test the code on host machine. Else you can skip this step.

```bash
g++ -I/usr/local/include/opencv -I/usr/local/include/opencv2 -L/usr/local/lib/ -g imgToGrey.cpp -o PC_imgToGrey -lopencv_core -lopencv_imgproc -lopencv_highgui
./PC_imgToGrey test.jpg
```


#### Compiling for ARM
```bash
arm-none-linux-gnueabi-g++ -I$ARMPREFIX/include -I$ARMPREFIX/include/opencv -I$ARMPREFIX/include/opencv2/imgproc -I$ARMPREFIX/include/opencv2 -L$ARMPREFIX/lib -g -o AR_imgToGrey imgToGrey.cpp -lopencv_core -lopencv_imgproc -lopencv_highgui
```
This will create an executable *AR_imgToGrey*, which you can transfer to your AR Drone.

Writing these lengthy commands for compilation can get cumbersome, so I have written a bash script that makes the cross-compilation process easier and automatically transfers the cross-compiled executable to AR Drone. See Cross Compilation made easy for more details.
