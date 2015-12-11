---
layout: project
title: A* Search
date: December 12, 2014
image: astar.jpeg
---
 
## Overview

The A* (pronounced "A star") algorithm is a popular computer algorithm that uses a best-first search approach to find the least cost path between two points in a grid. The cost could be any metric, be it distance, time, effort, and more. The grid can also be many different things, be it a map, a string of letters, a network of computers, social networks (e.g. the degree of connection in LinkedIn), and many more. Because of its robustness and accuracy, it is a popular algorithm. One obvious application for this algorithm is in off-line or on-line path-planning. 

## Algorithm 

The beauty of the A* method is in its simplicity. The search works by evaluating the true cost from the origin node and the heuristic cost to the goal node. There are many different ways of describing the true cost and heuristic costs, depending on the application. Some common examples for the heuristic cost include Euclidean distances, Manhattan, and Diagonal distances. 

## Off-line A* Search

This is a basic implementation of the A* search algorithm in a 2D grid map, using a Euclidean distance heuristic. 

The code generates a random set of obstacles, a random set of traversable nodes, and a random origin and goal node. It then finds the shortest path from origin to goal using the A* search algorithm. 

The code repository can be found  [here]().  

## On-line A* Search

This code is an on-line implementation of the A* algorithm for non-holonomic mobile robot motion planning. 

This means that at any point, the robot's view of the map is limited to the grid cells immediately around it. This means the path found may not necessarily be the shortest path, but it is still a viable path that avoids obstacles. 
Two different heuristics were used for this algorithm, the Euclidean and the Chebyshev distance. The Chebyshev heuristic performed better resulting in less back-tracking of the robot. 

The code repository can be found [here](). 