---
layout: project
title: ros_control and Gazebo
date: October 15, 2014
image: rrrbot.jpg
---

## Overview
The goal of this project was to understand how ros_control and Gazebo can be used together in order to model robot sensing and actuation in a simulated environment. The 2R robot arm from [the rr_bot] (https://github.com/ros-simulation/gazebo_ros_demos) repository was modified to a 3R robot arm. 

ros_control has been used with real robots, however, it is important to be able to test control laws in simulation before implementing them on real robots. This is especially the case when working with expensive robots. That was the motivation behind learning how to implement ros_control in the Gazebo simulation environment. 

We used Rviz to visualize our robot model. We were able to moniter the sensor feed from the Hokuyu laser scanner attached at the RRRbot end effector using ros_control. 

Subsequently, we were able to implement torque control to stabilize the simulated RRRbot arm with an added load at the end-effector.

### Learn with us
We documented our understanding in the form of an easy to follow README tutorial for anyone who is new to the world of ROS, Gazebo and ros_control.

#### Code
The project [source](https://github.com/JoshMarino/gazebo_and_ros_control/tree/ca24b8c1f7f1fd799ac3c9211240e5f0b76e1357) code is available on github. 
