---
title: "EKF SLAM with unknown data association"
date: 2026-03-31T14:15:05+07:00
description: EKF SLAM with unknown data association
image: SLAM/SLAM.gif
tags:
  - ROS 2
  - EKF SLAM
  - Mahalanobis Data Association
  - Circle Fitting
  - Turtlebot Robot
 
  
  
draft: false
order: 2
github: https://github.com/rishika2024/Turtlebot_SLAM

---

# EKF SLAM With Unknown Data Association

## Overview

Built a complete EKF SLAM pipeline from scratch in C++ using ROS 2, simulated on a TurtleBot3. The robot localizes itself and maps cylindrical landmarks simultaneously using wheel odometry and 2D LiDAR. Three robots are visualized: red (ground truth, known only to the simulator), blue (odometry estimate, which drifts over time), and green (SLAM corrected estimate). The video shows the robot driving a closed path with sensor noise and limited detection range. As odometry drifts, the EKF corrects it by fusing landmark observations, keeping the green path close to ground truth while blue diverges. Landmark estimates in green converge toward their true positions with repeated observations.

{{< video "SLAM/SLAM_video.mp4" 640 360 >}}


## ROS 2 Package Breakdown

### turtlelib:

Standalone C++ library for 2D rigid body transforms, SE(2) geometry, twist integration, and differential drive kinematics. No ROS dependency.

### nuturtle_description:

Custom URDF/Xacro for TurtleBot3 with configurable wheel radius, track width, and collision geometry. Supports multiple color coded robots for simultaneous visualization.

### nusim:

Simulation node that models the robot's environment including wheel slip, Gaussian sensor noise, obstacle collision detection, fake landmark sensing, and simulated 2D LiDAR via circle line intersection.

## nuturtle_control

Converts twist commands to wheel velocities, publishes odometry from encoder data, and provides a circle driving node for testing. Includes integration tests using catch_ros2.

### nuslam:

The SLAM package. Runs the Extended Kalman Filter with unknown data association via Mahalanobis distance. Contains a landmark detection node that clusters LiDAR points, fits circles using regression, and classifies valid landmarks using the inscribed angle theorem.

## Landmark Detection

The landmark detection node takes raw 2D LaserScan data and extracts cylindrical landmarks. First, scan points are clustered based on a distance threshold between consecutive readings, wrapping around the full scan. Clusters with fewer than 3 points are discarded. Each remaining cluster is passed through a circle fitting regression algorithm to estimate the center and radius. Clusters are then classified as circle or not circle using the inscribed angle theorem: if the standard deviation of inscribed angles is low and the mean angle falls within a valid range, the cluster is accepted as a landmark. Circles with unreasonable radii are filtered out as a final sanity check.



## Simulated Sensor Data

{{< video "SLAM/fake_sensor.mp4" 640 360 >}}

The simulator generates fake sensor data to test the SLAM pipeline without a physical robot. Landmark positions are published relative to the robot at 5 Hz with Gaussian noise, and landmarks beyond a configurable max range are hidden. A simulated LiDAR publishes a full LaserScan by raycasting against obstacles and arena walls using circle line intersection, also with Gaussian noise on each range reading. Wheel commands are corrupted with input noise and random slip per wheel, so encoder readings diverge from true motion over time, forcing the EKF to rely on landmark corrections.


## Odometry and Control

{{< video "SLAM/odom_sim.mp4" 640 360 >}}

The video shows the TurtleBot3 driving in circles both in simulation and on the physical robot. In simulation, the blue (odometry) and red (ground truth) robots overlap perfectly since there is no noise or slip. On the real robot, the blue robot gradually drifts from the actual position as odometry error accumulates over repeated loops. The final odometry pose after returning to the start position gives a measure of the cumulative drift.

