---
layout: post
title: How to Cross-Compile OpenCV with FFmpeg (x264 and xVid) for AR Drone (ARM Processor)
description: "How to cross-compile OpenCV libraries with FFmped, x264 and xVid support for AR Drone's ARM processor"
tags: ['arm', 'linux', 'drone', 'opencv']
---

#### Setting Up Environment Variables

Remember the *setupCrossCompiler* file that we created in the [previous post](/blog/2015/04/28/how-to-cross-compile-for-ar-drone/), we need to edit this file again to specify the path were all the cross-compiled libraries will go. We want to keep the compiled libraries in the *ARM_Install* folder in the home directory.

Running the following command will export the environment variable *ARMPREFIX* that defines the install path *~/ARM_Install*.

```bash
echo "export ARMPREFIX=/home/$USER/ARM_Install #path where the cross compiled programs will be installed">>~/ARM_Files/setupCrossCompiler
```

at this point you should open a new termial so that the new environment variable can effect.

#### Compiling xVideo
```bash
cd ~/ARM_Files
wget http://downloads.xvid.org/downloads/xvidcore-1.3.3.tar.gz
tar -zxvf xvidcore-1.3.3.tar.gz
cd xvidcore/build/generic/
./configure --prefix=${ARMPREFIX} --host=arm-none-linux-gnueabi --disable-assembly
make
make install
```

#### Compiling x264
```bash
cd ~/ARM_Files
git clone git://git.videolan.org/x264
cd x264
./configure --enable-shared --host=arm-none-linux --disable-asm --prefix=${ARMPREFIX} --cross-prefix=${CCPREFIX}
make
make install
```

#### Compiling FFmpeg
```bash
cd ~/ARM_Files
git clone git://source.ffmpeg.org/ffmpeg.git
cd ffmpeg
git checkout release/2.6
./configure --enable-cross-compile --cross-prefix=${CCPREFIX} --target-os=linux --arch=arm --enable-shared --disable-static --enable-gpl --enable-nonfree --enable-ffmpeg --disable-ffplay --enable-ffserver --enable-swscale --enable-pthreads --disable-yasm --disable-stripping --enable-libx264 --enable-libxvid --prefix=${ARMPREFIX} --extra-cflags="-I"${ARMPREFIX}"/include" --extra-ldflags="-L"${ARMPREFIX}"/lib"
make
make install
```

#### Compiling OpenCV
Download opencv from github and checkout version 2.4.10

```bash
cd ~/ARM_Files
git clone https://github.com/Itseez/opencv.git
cd opencv
git checkout 2.4.10
```
Create a new toolchain file *my.toolchain.cmake* in OpenCV directory, this file will tell cmake how to build the OpenCV.

```bash
gedit my.toolchain.cmake
```
Paste the following code in the file
<script src="https://gist.github.com/MHassanNadeem/2ec00cd9ae1502e8321e.js?file=my.toolchain.cmake"></script>

Now generate the build files using toolchain file you just created.

```bash
mkdir build
cd build
cmake -DENABLE_PRECOMPILED_HEADERS=OFF -DWITH_FFMPEG=ON -DCMAKE_TOOLCHAIN_FILE=../my.toolchain.cmake ../
```
After this is done you can scroll up to confirm if OpenCV has found FFMPEG. You should see something like the following

```
--   Video I/O:
--     DC1394 1.x:                  NO
--     DC1394 2.x:                  NO
--     FFMPEG:                      YES
--       codec:                     YES (ver 56.35.101)
--       format:                    YES (ver 56.30.100)
--       util:                      YES (ver 54.23.101)
--       swscale:                   YES (ver 3.1.101)
```


<div class="green-box">
<strong>Note: </strong> At this point you have the option to add/remove OpenCV components using cmake-gui by doing a <i><b>cmake-gui .</b></i>
</br>Don't forget to "Configure" and "Generate" after making any changes.
</div>

```bash
make -j2
```
Make will take a while, so grab a coffee or something.
If you get a compilation error regarding some of OpenCV component(s), you can try and disable that component if you don't need it.
At this point OpenCV has been compiled now all that is left to do is install it.

```bash
make install
```
This will copy the cross-compiled OpenCV to your /home/$USER/ARM_Install directory, the directory you defined in the first step.

You have successfully cross compiled OpenCV, you are now ready to [build your first OpenCV program]({{ site.baseurl }}/blog/2015/04/30/cross-compile-opencv-project-ar-drone/).

