---
layout: project
title: ASL Interpreter
date: May 26, 2015
image: aslF.jpg
---

## Overview
This project uses a combination of image analysis techniques and a nearest neighbor algorithm to design a sign language interpreter. Decision Tree, k-Nearest Neighbor, Naïve Bayes, and Random Forest machine learning techniques were applied to the data in order to correctly classify signed gestures belonging to the American Sign Language (ASL). Analysis of the results shows that 3-Nearest Neighbor is able to produce a model with a testing accuracy of 97% on this data. Using 3-NN, we were able to develop a program that correctly classifies ASL signs and returns the correct letter from the English alphabet in under 10 seconds. 

### Introduction
The purpose of this project is to identify machine learning algorithms and classification methods that can accurately understand signed hand gestures in real time. To achieve this task, the input to our algorithm is one static image of a sign and the output is the identity of the sign.

Hand gesture recognition for human computer interaction is an area of active research in machine learning and computer vision. The applications of gesture recognition can be extended to several useful fields,especially in robotics and human-computer interaction. The primary goal of our gesture recognition project was to create a system which can identify American Sign Language (ASL) gestures and translate them to the written English alphabet.

An estimated 360 million people worldwide suffer from hearing loss. Because the majority of hearing
individuals do not understand sign language, communication between the hearing and the deaf can be
challenging. A program such as what we have proposed in this paper is extremely important in the interest of breaking down this barrier.

Our goal was to create a learner that can recognize static signs with high accuracy. We aimed to optimize the speed with which the learner can understand the sign gesture. Thus, our measure of success throughout our investigation was both a measure of percent accuracy of detection as well as the time taken to identify signs.

### Method
We trained learning models with five different algorithms using the Weka 3.6 package. 
Training times, test times, and accuracies were measured and compared for the different mothods. All training experiments were performed with 10-fold cross validation. The five algorithms tested were: 
 
1. Decision Tree
2. Random Forest
3. 1 Nearest Neighbor
4. 3 Nearest Neighbor
5. Naive Bayes
 
For the experimental evaluation, we generated three different datasets with varying number of attributes and instances: 
 
* Dataset 1: 
10 ASL signs corresponding to the following letters in the alphabet: {B, E, F, H, J, L, O, V, W, Y}
Letters were chosen such that the signs were most distinct from each other. 
392 instances per letter
Total instances within the data set: 3920

* Dataset 2:
26 ASL signs corresponding to all 26 letters of the English alphabet
168 instances per letter
Total instances within the data set: 4368

* Dataset 3:
26 ASL signs corresponding to all 26 letters of the English alphabet
392 instances per letter
Total instances within the data set: 10192
 
Our goal was to create a learner that can recognize static signs with high accuracy. We aimed to optimize the accuracy and speed with which the learner can understand the ASL sign gestures. Thus, our measure of success throughout our investigation was both a measure of percent accuracy of detection as well as the time taken to identify signs.


### Results
We were able to successfully collect data of ASL gestures and represent the images by their most important features. We found that the most important feature set was an array of bit values after nine image processing steps. In this way, we collected a training data set with over ten thousand examples. 
 
We were also able to test different supervised classification machine learning methods on our dataset and compare their accuracies. We used the Weka 3.6 package. By doing so, we were able to choose the most accurate classifier for our prediction step. We discovered that for our training data, the 3 Nearest Neighbor classifier was the best choice with a classification accuracy of 96.78% on test data and a processing time of about 10 seconds.
 
Finally, we successfully implemented our data preprocessing code along with our chosen high-accuracy classifier into a program that can correctly classify signed gestures from individuals outside of the dataset space and return the correct corresponding letter, in real time. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/WusL_jwaQfc" frameborder="0" allowfullscreen></iframe>

### You can find more information on the project [website](http://mahdiehn.wix.com/aslinterpreter). 
