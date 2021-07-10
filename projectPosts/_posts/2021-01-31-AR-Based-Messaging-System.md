---
layout: post
title:  "AR Based Messaging System (Notes Hub AR)"
date:   2021-01-31
desc: "AR Based Messaging System (Notes Hub AR)"
keywords: "AR,ARCore,arcore,messaging,project,noteshub,hub"
categories: [C#]
tags: [AR, ARCore, NotesHub AR]
icon: icon-csharp
---

# Introduction
A mobile app which allows users to interact with each other by leaving virtual notes that are attached to the real world through augmented reality technology.

Benefits include :
* 	Multi-user content sharing through public AR notes
* Developed for indoors
*	Environment friendly media types for notes such as text, images or 3D models.

## Requirements
*	Mobile android device with Google ARCore support
*	Development device with Unity (_2019.4.3f1+_)
*	Suitable medium for testing (_100+ texture points_) 

# Development

This project is developed in _C#_ on _Unity Platform_, with ARCORe extensions for _Unity's AR Foundation_. Also I want to specify that this is my first attempt on developing an augmented reality app, so this project have not reached its best version, yet.

## Application and Technologies

Since this is an augmented reality application developed for indoor use, I aimed to use media recording and comparison algortihms after scanning existing objects indoors and identifying natural markers instead of GPS technology that is used for applications developed for outdoors. For scanning indoors, I got help from Google's ARCore since it has advanced environmental understanding algorithms and this step becomes more efficient.

For multi-user experience I used Google Cloud, and cloud anchors from ARCore. This way two users could use the application concurrently and see the changes at the same time. Additionally using cloud anchors helped me to recognize the current environment and place the augmented images accordingly.

Due to hardware complications, I could not find enough time for optimization and adding more features to the application. Despite these problems, I could manage to stabilize augmented object placement precision around 5 cm. 

Here are the samples of UI and usage of application.
<p align="middle">
<img src="/static/assets/img/blog/arMessage/texturepoints.jpg" width="30%" hspace="50"> 
<img src="/static/assets/img/blog/arMessage/hifromnoteshub.jpg" width="30%">
</p>


# Future Goals

### Multiplatform Application

With the help of Unity AR Foundation with a simple configuration change, I can publish my application on IOS in addition to Android.

### User Experience Testing

Due to lack of enough mobile devices, I could not test the final product with a lot of users. In the future, I want to test my application in a more systematic way so that I can update my application accordingly.