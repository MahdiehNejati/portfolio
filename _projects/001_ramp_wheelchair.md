---
layout: project
title: Smart Wheelchair
date: September 21, 2015
image: wheelchair.jpg
---

## Overview 

## Stages of Development

The smart_wheelchair code has been developed for planar surface navigation, with doorway detection and docking location detection. Every other object in the world is treated as an obstacle. However, incline navigation and drop-off avoidance is important features, especially for urban navigation of powered wheelchairs. The video below shows how the wheelchair behaves without my code. It sees the ramp as an obstacle and will turn around in order to move past it.  

<iframe src="https://player.vimeo.com/video/148692289?loop=1" width="500" height="282" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>  

I developed a ramp detection algorithm and implemented it within the code structure. By doing so, the ramp is cleared from point cloud that is sent to the cost map server, called "cloud small". The cost map does not mark the incline as an obstacle and it remains traversable. With this algorithm in place, the wheelchair does not try to avoid the ramp. However, the controller written for the simulation, as well as the URDF were needed to be fixed, because as you can see in the video below, as much as the wheelchair is trying, it cannot drive over the incline. 

<iframe src="https://player.vimeo.com/video/148693925" width="500" height="311" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

I updated and cleaned the URDF, and implemented a Differential Drive Controller. With the ramp detection algorithm and the new controller, the wheelchair can now successfully and safely  drive over inclines. 

<iframe src="https://player.vimeo.com/video/148693996" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> 

Below is some more detail about the different stages of the project development thus far. 

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
<img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/real_wheelchair.jpg" width="300" height="300" /> | <img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/wheelchair_sim_model.png" width="300" height="300" />

This work is based on the current wheelchair platform at the [Argallab](http://smpp.northwestern.edu/research/argallab/research.html). 

* Input: Point Cloud data from the on-board Asus Xtion
* Non-honolomic differential drive base

## Ramp Detection Algorithm

Raw Point Cloud | Planar Surface Extraction | Ramp Segmentation
------------ | ------------- | ------------ 
<img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/ramp6_raw.png" width="250" height="250" /> | <img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/extract_planar_surface.png" width="250" height="250" />|<img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/real_extract_ramp.png" width="250" height="250" />

The image processing was done using the [Point Cloud Library](http://pointclouds.org/) framework [in ROS](http://wiki.ros.org/pcl). 

* PCL is a large standalone C++ library
* Split into smaller code libraries that can be compiled separately.
* Numerous algorithms for filtering, model segmentation, etc. 
* Many algorithms are already compiled as ROS nodelets.

The ramp segmentation code was first developed as a standalone C++ file that takes PCD files as the input. My code is linked [here](https://github.com/MahdiehNejati/my_pcl/blob/master/ramp_segmentation/ramp_segmentation.cpp). After the basic algorithm was developed, it was incorporated into the ROS framework by taking point cloud topics as the input. 

Gazebo Sim Scene | Planar Surface Extraction | Ramp Segmentation
------------ | ------------- | ------------ 
<img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/original_scene.png" width="250" height="250" /> | <img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/rgp_point_cloud.png" width="250" height="250" /> | <img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/ramp_segmented.png" width="250" height="250" />

Most of the image processing was implemented using nodelets, inside the [openni_layer](https://github.com/MahdiehNejati/smart_wheelchair/blob/vision/src/nuric_system/launch/system/openni_layer.launch#L87-L163) launch file. This not only ensures the computation costs are kept at a minimum, but also because nodelets have several desirable characteristics as opposed to writing a separate node that loads pcl_ros libraries: 

* Nodelets allow multiple algorithms in a single process.
* No copy costs incurred when passing messages intraprocess
* Dynamically loadable at runtime.
* Point cloud does not get copied or serialized.
* Provide separate namespaces such that nodelets act like a separate node despite being in the same process. 
* Dynamically reconfigurable using 

     ``` rosrun rqt_reconfigure rqt_reconfigure ```
	
The last point is especially useful when tuning parameters in the image processing pipeline in that it significantly speeds up the fine-tuning of the algorithm.  

The ramp detection algorithmic pipeline looks like this: 

<img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/ramp_pcl_pipeline.jpg" width="650" height="250" />

This was my first foray into image processing with point cloud data, and I found the [pointcloud.org tutorials](http://pointclouds.org/documentation/tutorials/) helpful and well written. 

## Simulation Model

### Wheelchair Model

The URDF model in the smart_wheelchair package used deprecated packages, and the caster wheels were defined as rigid joints, and fixed parts of the wheelchair were described as links and joints. This made the code unnecessarily long, complicated, and unusable for my project. I cleaned up the URDF (down to 460 lines of code from >2000 lines) and updated it so that it only uses updated and maintained ROS plugins for the sensors and controllers. There were also issues with the Kinect mesh file that I fixed. 

The wheelchair has two main wheels, four caster wheels, and an ASUS Xtion camera. These elements are the only links and joints that make up the URDF, as seen in the diagram below:

<img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/new_urdf.jpg" width="800" height="600" />

Prior to my updates, the caster wheels were rigid links and haphazardly linked to the wheelchair body. All six wheels rotate and are correctly position in the new URDF. 

### Gazebo World

To further develop and improve upon the algorithm, I designed several different Gazebo worlds, each with unique properties to allow for a more expansive test environment. 

## Differential Drive Controller

There are many well written and well maintained [ros controllers](http://wiki.ros.org/ros_controllers?distro=indigo); one of which is the [diff_drive_controller](http://wiki.ros.org/diff_drive_controller?distro=indigo) package I used for the kinematic control of the wheelchair. 

The Clearpath Robotics [Jackal](http://wiki.ros.org/Robots/Jackal) and [Husky](http://wiki.ros.org/Robots/Husky) are two robots that also use the diff_drive_controller. Although there isn't a specific tutorial on how to use this plugin, the [jackal_control](https://github.com/jackal/jackal/tree/indigo-devel/jackal_control) and [husky_control](https://github.com/husky/husky/tree/indigo-devel/husky_control) packages are very well written, and I used them as templates for how to implement the controller within my wheelchair package. 

## Code Structure

The smart_wheelchair package has gone through a lot of development. The code is complex, but very well written and maintained. The code is currently private, because it is still under development. However, below is a diagram of the code architecture. In order to modify the software, I had to understand the software pipeline and add/modify the code appropriately. When adding a new package, such as the controller package, I kept with the rest of the code's naming and structuring standard. 

<img src="https://raw.githubusercontent.com/MahdiehNejati/portfolio/gh-pages/public/images/smart_wheelchair_structure.jpg" width="650" height="500" />

## Future Work

The over-arching goal of this project is to create a ramp layer and drop-off layer to the ROS navigation stack package, such that any wheeled robot can easily incorporate this algorithm. Work on this is currently under way. 
