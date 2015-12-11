---
layout: project
title: Smart Wheelchair
date: September 21, 2015
image: wheelchair.jpg
---

## Overview 


### Motivation

There are basic maneuvering tasks with a powered wheelchair, such as passing through a doorway, traversing ramps, and avoiding drop-offs, which can be difficult for powered wheelchair users with severe motor impairments - not only because of limitations in the users own motor control, but also because of the limitations in the control interfaces available to them. [Professor Argall](http://users.eecs.northwestern.edu/~argall/) and her [lab](http://smpp.northwestern.edu/research/argallab/) are developing a partial-automation wheelchair that combines the intent of the user with the assistance of automated obstacle avoidance, navigation, route planning, and spatially constrained maneuvers. The current wheelchair platform has doorway assistance and docking assistance functionalities. The motivation for this project was to extend on the current capabilities of this partial-automated wheelchair. 


### Aim

The objective of this project is to detect traversable and non-traversable areas for safe and autonomous urban navigation of powered wheelchairs. Specifically, my work is aimed at adding ramp navigation and drop-off avoidance behaviors to the current wheelchair platform. 

### Outline

The major components of this work included: 

1. Develop ramp and drop-off detection algorithm
2. Create simulation Model 
	* Create a ramp playground in Gazebo
    * Clean and simplify the URDF 
3. Add a differential drive Controller 
4. Ramp assistance code: Goal following up a ramp
5. Drop-off avoidance: Modifying local cost-map
6. Integration within the current code framework of smart_wheelchair package. 
7. Adding ramp layer to the ROS navigation stack, using the algorithms developed from the above work

## Platform

Real Platform | Simulated Platform
------------ | -------------
![real_wheelchair](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/real_wheelchair.jpg) | ![sim_wheelchair](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/wheelchair_sim_model.png)

This work is based on the current wheelchair platform at the [Argallab](http://smpp.northwestern.edu/research/argallab/research.html). 

* Input: Point Cloud data from the on-board Asus Xtion
* Non-honolomic differential drive base

## Ramp Detection Algorithm

Raw Point Cloud | Planar Surface Extraction | Ramp Segmentation
------------ | ------------- | ------------ 
![ramp6_raw](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/ramp6_raw.png) | ![extract_planar_surface](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/extract_planar_surface.png) | ![real_extract_ramp](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/real_extract_ramp.png)

The image processing was done using the [Point Cloud Library](http://pointclouds.org/) framework [in ROS](http://wiki.ros.org/pcl). 

* PCL is a large standalone C++ library
* Split into smaller code libraries that can be compiled separately.
* Numerous algorithms for filtering, model segmentation, etc. 
* Many algorithms are already compiled as ROS nodelets.

The ramp segmentation code was first developed as a standalone C++ file that takes in PCD files as the input. The code is linked [here](https://github.com/MahdiehNejati/my_pcl/blob/master/ramp_segmentation/ramp_segmentation.cpp). After the basic algorithm was developed, it was incorporated into the ROS framework by taking point cloud topics as the input. 

Gazebo Sim Scene | Planar Surface Extraction | Ramp Segmentation
------------ | ------------- | ------------ 
![original_scene](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/original_scene.png) | ![rpg_point_cloud](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/rpg_point_cloud.png) | ![ramp_segmented](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/ramp_segmented.png)

Most of the image processing was implemented using nodelets, inside the [openni_layer](https://github.com/MahdiehNejati/smart_wheelchair/blob/vision/src/nuric_system/launch/system/openni_layer.launch#L87-L163) launch file. This not only ensures the computation costs are kept at a minimum, but also because nodelets have several desirable characteristics as opposed to writing a separate node that loads pcl_ros libraries: 

* Nodelets allow multiple algorithms in a single process.
* No copy costs incurred when passing messages intraprocess
* Dynamically loadable at runtime.
* Point cloud does not get copied or serialized.
* Provide separate namespaces such that nodelets act lik a separate node despite being in the same process. 
* Dynamically reconfigurable using 
	 ``` rosrun rqt_reconfigure rqt_reconfigure ```
	
The last point is especially useful when tuning parameters in the image processing pipeline and significantly speeds up fine-tuning the algorithm. 

The ramp detection algorithmic pipeline looks like this: 

![ramp_pcl_pipeline](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/ramp_pcl.jpg) 

This was my first foray into image processing with point cloud data, and I found the [pointcloud.org tutorials](http://pointclouds.org/documentation/tutorials/) helpful and well written. 

## Wheelchair Model

![updated_urdf](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/new_tf.png)

![deprecated_urdf](https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/old_tf.png)

## Differential Drive Controller

tested in different environement

## Code Structure


## Future Work

The over-arching goal of this project was to create a ramp layer and drop-off layer to the ROS navstack package, such that any wheeled robot can easily incorporate this algorithm. Work on this is currently under way. 