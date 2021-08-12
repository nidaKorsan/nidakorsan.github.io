---
layout: post
title: "Tesseract For Android"
date: 2021-08-01
desc: "Tesseract OCR Shared Libraries Built Via Docker for Android arm64-v8a"
keywords: "Tesseract, OCR, Android, Docker, arm64-v8a"
categories: [Docker]
tags: [Tesseract, OCR, Android, Docker]
---

# Introduction

Tesseract is an optical character recognition (OCR) engine. It has different versions developed so far. Tesseract 3 works by recognizing character patterns. Tesseract 4 adds a new neural set (LSTM) based OCR engine focused on line recognition. ([To Learn More About Tesseract](https://github.com/tesseract-ocr/tesseract#about))

I needed Tesseract OCR for the mobile application that I am working on which should recognize different pages of a booklet and then do required operations. 

At first this was supposed to be done by Google ARCore's target image tracking support. Unfortunately, image tracking does not work efficiently with text images. In my opinion this problem occures because text view is similar to a pattern.

To overcome this issue I searched for different alternatives and encountered with Tesseract OCR. In their Github pages, they provide the source code and a detailed [documentation](https://tesseract-ocr.github.io/tessdoc/) for this technology.

If you are searching these libraries for arm64-v8a architecture you can visit my github [here](https://github.com/nidaKorsan/Tesseract-for-Android-arm64-v8a) and use the ready files. If you are going to build for another architecture or with another versions, you can follow my steps for building operation and adapt them to your taste.

# Building From Source Code For Android

In their documentation page, they provide two alternatives to build for [Android](https://tesseract-ocr.github.io/tessdoc/Compiling.html#android). One of them is using Android NDK. This method works but it only produced the static libraries which are not useful for me since I was building my application in Unity. This method is useful for those whom are working on Android Studio.

Hence I tried the alternative method they provided. Building for Android with Docker. It redirects us to a Github [repository](https://github.com/rhardih/bad) of [@rhardih](https://github.com/rhardih). 

Here the author explains why Docker is a useful method for cross compiling native libraries. Then shows us how to build the libraries according to our taste.

The author provides a **bad** command to make the building journey easier. Unfortunately I could not use this command due to some errors that I could not resolve(still cannot), so took some help from the makefiles that are present in the repository. 

I needed three shared libraries from this source code. *tiff*, *leptonica* and *tesseract*. Firstly, of course, you need Docker to build these source files with Docker. You can see [this page](https://docs.docker.com/engine/installation) for installation instructions.

After you install docker to your computer, you can clone the **bad** repository to use the files inside. Now, these files take different versions as parameters. Also since the libraries are depented on each other in a hierarchy of; tesseract > leptonica > tiff, we first need to build tiff. 

When you go into  <code>bad/tiff/tiff.Dockerfile</code>, you can see the comment that says <br># List of available versions can be found at<br>
(# https://download.osgeo.org/libtiff)<br>
When you click the link it redirects to a page where all the versions are listed. I chose **4.3.0** for my build. I embedded the version parameter into the dockerfile as <code>ARG VERSION=4.3.0</code> right under the comment. Then went into <code>../tiff/tiff.mk</code> and I changed the makefile command for **arm64-v8a** a bit to my taste as <code>docker build --build-arg VERSION=4.3.0 --build-arg HOST=aarch64-linux-android \<br> --build-arg TOOLCHAIN=aarch64-linux-android-4.9 -t bad-tiff:4.3.0-arm64-v8a \<br> -f tiff/tiff.Dockerfile .</code> since I did not need the BUILD_ARGS. I also defined the VERSION as 4.3.0 on command line. After build, you can run the following command to create the libtiff.so. Oh, and also you should create a directory to store the shared libraries named **extracted** in *bad* directory.<br><code> docker run -i -v `pwd`/extracted:/host "bad-tiff:4.3.0-arm64-v8a" \<br> cp -r "/tiff-build/." "/host/tiff-4.3.0-arm64-v8a-build"</code>

Next library to build is leptonica. Since I want these libraries for arm64-v8a architecture, in <code>bad/leptonica/leptonica.Dockerfile</code>, I changed <code>ARG ARCH=armv7-a</code> to <code>ARG ARCH=arm64-v8a</code>. Then I specified the version as 1.81.1 in Dockerfile. I modified the makefile command in <code>../leptonica/leptonica.mk</code> as<br>
<code>tiff-arm64-v8a/4.3.0 docker build --build-arg VERSION=1.81.1 --build-arg HOST=aarch64-linux-android --build-arg \<br>TOOLCHAIN=aarch64-linux-android-4.9 --build-arg ARCH=arm64-v8a -t bad-leptonica:1.81.1-arm64-v8a -f leptonica/leptonica.Dockerfile .</code>
But...It gave an error. Appearently, the folder name has been changed by the respective author of the library given in the link that takes place in the Dockerfile on row 25. Instead of archive it is updated as release... Well, good thing we reliazed it. So, I updated the row as<br>
<code>https://github.com/DanBloomberg/leptonica/releases/download/1.81.1/leptonica-1.81.1.tar.gz</code>. Just to warn you, it may again change in the future, so take care.<br>
After building the library, you should run "docker run" command. I run it as follows:<br>
<code>docker run -i -v `pwd`/extracted:/host "bad-leptonica:1.81.1-arm64-v8a" cp -r "/leptonica-build/." "/host/leptonica-1.81.1-arm64-v8a-build"</code><br>
This gives us liblept.so! Hurray!

Next and final library to build is tesseract. I used version 4.0.0, so tesseract-4.0.0.Dockerfile is the one I used during my build. Now, I went into dockerfile and made the following changes:<br>
On line 3, changed <code>ARG ARCH=armv7-a</code> as <code>ARG ARCH=arm64-v8a</code><br>
On line 15, changed <code>ARG ABI=armeabi-v7a</code> as <code>ARG ABI=arm64-v8a</code><br>
Added <code>-D BUILD_SHARED_LIBS=ON \</code> to line 53.

After these changes I runned the command:<br>
<code>tiff-arm64-v8a/4.3.0 leptonica-arm64-v8a/1.81.1 \ <br>
	docker build --build-arg VERSION=4.0.0 --build-arg  \ <br>TOOLCHAIN=aarch64-linux-android-4.9 --build-arg ARCH=arm64-v8a  \ <br>--build-arg ABI=arm64-v8a -t bad-tesseract:4.0.0-arm64-v8a -f tesseract/tesseract-4.0.0.Dockerfile .
</code>

Now I have all the shared libraries with extension ".os". The only thing I needed to do use them in my Unity project, was to simply CTRL+C + CTRL+V the shared libraries into <code>Assets/Plugins/Android</code> directory.

One more additional thing you need to bind these libraries together is the suitable libc++_shared.so file. There are different ways to acquire this file listed on any web browser if you type "how to build libc++_shared.so". 

Instead of this, I used the ready-to-use file which is in the 
<code>C:\Users\UserName\AppData\Local\Android\Sdk\ndk\xx.xxx\sources\cxx-stl\llvm-libc++\libs\arm64-v8a</code>[1]
This file is built via llvm project.You can find the documentation [here](https://llvm.org/docs/GettingStarted.html). With Android Studio, this is built automagically, so it saves us from a lot of work. It has the file for different architectures in the parent directory of [1].<br>

Thanks for reading my little tesseract journey! Should you have any questions, you can reach me through my mail or social media accounts.