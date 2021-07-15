---
layout: post
title:  "GPU Optimized DL-Based Point Cloud Meshing"
date:   2021-06-24
desc: "GPU Optimized DL-Based Point Cloud Meshing"
keywords: "point cloud, cloud, point, meshing, water-tight, 3D, DL, deep learning"
categories: [Python]
tags: [Point Cloud, Deep Learning]
icon: icon-python
---

# Introduction

In  this project,  it  was  studied  to  develop  a  deep-learning  based  water-tight  surface fitting algorithm.The purpose of the algorithm is to fit a mesh system for the point cloud  data  in  the  local  environment.  GPU  optimization  is  deemed  necessary  for online operations on the tablet.

The scientific  and socioeconomic  effect of  the  project  is  that since the  process  of fitting a water-tightsurface to the point cloud is a much-needed operation in AR/VR and CAD applications, this project provides an alternative.

## Requirements
* Development device with Blender(_2.9+_)
* Tensorflow 2
* Python 3

You can see the simple flow diagram below:

![diagram](https://user-images.githubusercontent.com/32648694/125750045-11545641-6096-4a10-accd-eab81cc1874c.PNG)


# Dataset Preperation

Dataset needed for this project is prepared by me. To train the deep learning model, input and output sets were needed, hence I implemented a python script that handles ray-casting for point cloud data extraction from different 3D models and then applies Delaunay triangulation to generate surface fitted outputs. 

![alt text](https://mathworld.wolfram.com/images/eps-gif/DelaunayTriangulation_1000.gif "Delaunay Triangulation")
The input is 400 adjacent-looking points on camera view that are chosen from the point cloud.

The output is normally an 400x400 adjacency matrix. Since this matrix is symmetric, to increase memory efficiency the upper half of the matrix is taken and converted into an one dimensional vector of 400 binary (0, 1) elements. In this vector "0" represents that there is no edge between two vertices and "1" represents that there is an edge between two vertices.

# Deep Learning Model

One of the targets of the project was to do the surface fitting through a deep learning model. Considering the testing environment that is aimed, on a tablet, I decided to use MobileNetV2 as deep learning model.

As an input this model expects a 32x32 matrix with 3 channels each. The inputs that I generated were 20x20. Due to this, I added an extra of 12 rows and columns for each of the inputs that I gave to this model. Additionally, the channels are given as (x, y, z) coordinates of the points.

 1|2 |3
:-------------------------:|:-------------------------:|:-------------------------:
![1](https://user-images.githubusercontent.com/32648694/125749880-0f76bc98-bee9-4dd3-863b-dd29528f98c3.PNG)  |  ![2](https://user-images.githubusercontent.com/32648694/125749932-be7c2154-3841-4732-9e91-f3684aaf5d38.PNG)| ![3](https://user-images.githubusercontent.com/32648694/125749965-a736181f-def5-449b-a89a-9c6de7c83c92.PNG)






The pictures above show the meshed output of the model. The highlighted square respresents the 20x20 point cloud portion. After fitting faces to the returned edge vector, the result is the mesh. As you can see the depth of the model is preserved since the points have 3D coordinates even if the triangulation is done in 2D. 

# Future Goals

### Testing

Testing could not be done due to hardware reachability. If I can acquire a latest Android or IOS tablet, I would like to develop an application for my model and test in on tablet.

### Input Size Difference

For now the model does not work if there is less than 400 points in the given frame. To solve this problem I thought of a boolean channel addition to my input. Although I did not have enough time to implement this feature and embed it into my dataset. 

In the future I would like to implement and test this solution.
