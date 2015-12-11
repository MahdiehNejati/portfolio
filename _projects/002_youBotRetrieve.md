---
layout: project
title: youBot Block Retrieve
date: September 22, 2014
image: youbot_retrieve.png
---

## Overview
The goal of this project was to develop a package for the KUKA youBot that enables the youBot to drive towards a predetermined object, align itself properly, pick up the object and return to the home position. The project required us to apply our knowledge of ROS (Robot Operating System) learned during the ["Embedded Systems in Robotics"](http://www.mccormick.northwestern.edu/mechanical/courses/descriptions/495-embedded-systems-in-robotics.html) course instructed by Dr. Schultz. This was the first time we used ROS to control a real robot. As such, we needed to be aware of the interaction between hardware and software during the development process.

This package was developed in two parts. The two packages were then combined into the youbot_retrieve_msr package: 

1. Navigation of the robot base

  * youbot_nav_msr

2. Grasp control of the 5 degree of freedom arm

  * youbot_grasp_msr

You can watch a clip of the youBot working with our package [here](https://vimeo.com/114483339 "vimeo clip for youbot_retrieve_msr"). 

At present, the robot can only grasp a block of limited dimensions, using a predefined static map. 

### Base Navigation
The ROS 2D navigation stack was used to control the robot base. The Hokuyu laser scanner mounted in front of the youBot base was used to fine-tune the youBot's position relative to the designated objects. 

The youbot_nav_msr package [source](https://github.com/jihoonkim92/youbot_nav_msr "youbot_nav_msr package") is available on github. 

### Manipulation
Through this project we realised the challenges of implementing the inverse kinematics for a 5 degree of freedom arm. We added  two "virtual joints" to the youBot urdf, making a 7 degree of freedom arm making it easier to implemented a robust inverse kinematics algorithm. 

The youbot_grasp_msr package [source](https://github.com/mattmongeon/youbot_grasp_msr "youbot_grasp_msr package") is available on github. 

### Code
The code for this project was written in Python. 
The full package [source](https://github.com/MahdiehNejati/youbot_retrieve_msr "youbot_retrieve_msr package") is available on github. 
