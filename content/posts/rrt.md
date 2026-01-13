---
title: "RRT for 2D Path Planning"
date: 2025-09-11T14:15:05+07:00
description: Rapidly-exploring Random Tree (RRT) path planning in Python, with a simple visual demo.
image: rrt/RRT2.gif
tags:  
  - RRT Algorithm
  - Python
draft: false
math: true
order: 3
github: https://github.com/rishika2024/MSR_RRT.git
---


# Rapidly-exploring Random Trees (RRTs) for 2D path planning.

An RRT is a set of vertices (configurations) and edges (connections between configurations). Each iteration samples a random point in the domain, finds the nearest existing vertex, and takes a small step of size $\Delta$ toward the sample. If you run it for long enough, you get close to uniform coverage over the whole space.

## Simple RRT

{{< figure src="/rrt/RRT1.gif" alt="RRT growth" >}}

For implementing a Simple RRT:

- Domain: $D = [0,100] \times [0,100]$
- Initial configuration: $q_{\text{init}} = (50, 50)$
- Step size: $\Delta = 1$

At each iteration, the following steps are performed

1. Sample a random point $q_{\text{rand}}$ for $x$ and $y$.
2. Find the nearest vertex $q_{\text{near}}$ by looping through all vertices in $G$ and taking the one with minimum Euclidean distance.
3. Compute the unit direction vector from $q_{\text{near}}$ to $q_{\text{rand}}$ and step by $\Delta=1$ to get $q_{\text{new}}$:

$$q_{\text{new}} = q_{\text{near}} + \Delta\,\frac{q_{\text{rand}} - q_{\text{near}}}{\lVert q_{\text{rand}} - q_{\text{near}} \rVert}.$$

4. Add $q_{\text{new}}$ to $G$ and plot the edge $(q_{\text{near}}, q_{\text{new}})$ as it grows (I used interactive plotting with a small pause each iteration).

Running this for $K=1000$ iterations produces a tree that spreads outward from the start and begins to fill the entire domain. As the number of samples increases, the RRT provides increasingly uniform coverage of the free space.

## RRT with circular obstacles

{{< figure src="/rrt/RRT2.gif" alt="RRT growth" >}}

For this circular obstacles are added and a new vertex is only accepted if the edge from $q_{\text{near}}$ to $q_{\text{new}}$ is collision-free.

### Collision checking

Before adding $q_{\text{new}}$, check whether the line segment $(q_{\text{near}}, q_{\text{new}})$ intersects any obstacle circle. If it collides, ignore that sample and continue.

### Goal connection + path extraction

After adding a new vertex, check for a collision-free straight-line connection from that vertex to the goal. If it exists, terminate and recover the path by walking parent pointers back to $q_{\text{init}}$.

The final plot shows the obstacle circles, the full RRT, the start/goal, and the extracted path highlighted.

