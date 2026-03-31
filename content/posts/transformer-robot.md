---
title: "Teleoperated Ground-Air Morphing Robot"
date: 2026-03-11T14:15:05+07:00
description: A mobile robot that can operate both as a ground vehicle and as a drone
image: transformer-robot/ground_moving.gif
tags:
  - UAV
  - Onshape
  - ROS2
  - Pixhawk
  - Raspbery pi
  
draft: false
order: 1

---

# Teleoperated Ground-Air Morphing Robot

## Overview

This is a teleoperated transformer robot that can switch between flight and ground modes. I started with an F450 drone frame and designed a hinge mechanism on each arm: when the arms are straight, the robot flies as a quadcopter powered by brushless motors; when the arms fold down, it becomes a wheeled ground vehicle with wheels driven by mini servo motors. The folding itself is also actuated by servo motors.

https://private-user-images.githubusercontent.com/172546714/571893275-bb357b8e-f73c-4b09-b0f0-be0380d24280.mp4?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzQ5NjExMzMsIm5iZiI6MTc3NDk2MDgzMywicGF0aCI6Ii8xNzI1NDY3MTQvNTcxODkzMjc1LWJiMzU3YjhlLWY3M2MtNGIwOS1iMGYwLWJlMDM4MGQyNDI4MC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYwMzMxJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MDMzMVQxMjQwMzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT01MGUzMWJhNTc1ZmQxODQ0MTkwYzY5NDhjZTdkOWRkMDdiNDQ0ZmIzZDMwYjU4YzFmMWJlZGEzYWMzYWRiMTJmJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.P3Hg7FDSrJZO-n4m-lbXp1y3FNhSCxlJs6eYRbScSaQ


## Full System Architecture:
{{< figure src="transformer-robot/System.png" alt="Flight mode" width="250%">}}


## CAD Models:


Hinge Mechanism conceptual CAD model

{{< slide "transformer-robot/week5/hinge_mech1.png" "Hinge View1" "transformer-robot/week5/hinge_mech2.png" "Hinge View2" "transformer-robot/week5/hinge_mech3.png" "Hinge View3" >}}

*  Planetary Gear-like system for wheels

   *   Original Wheel Design
   {{< slide "transformer-robot/week6/wheel_assem1_complete.png" "Complete Assembly with Motor and Propeller" "transformer-robot/week6/wheel_assem_1.png" "Isometric View" "transformer-robot/week6/wheel_assem1_side.png" "Side View" "transformer-robot/week6/wheel_assem1_top.png" "Top View">}}

   *    Final Wheel Design
   {{< slide "transformer-robot/week6/wheel_assem2_complete.png" "Complete Assembly with Motor and Propeller" "transformer-robot/week6/wheel_assem2.png" "Isometric View" "transformer-robot/week6/wheel_assem2_side.png" "Side View" "transformer-robot/week6/wheel_assem2_top.png" "Top View">}}

*  Wheel designs I tried
{{< slide "transformer-robot/week6/wheel_v1.png" "Wheel Design 1" "transformer-robot/week6/wheel_v2.png" "Wheel Design 2" "transformer-robot/week6/wheel_v3.png" "Wheel Design 3" >}}

Original conceptual CAD model

{{< slide "transformer-robot/week1/initial_cad.png" "Initial CAD Design" "transformer-robot/week1/initial_cad.gif" "CAD Animation" "/transformer-robot/week1/initial_hinge.png" "Hinge Design" "/transformer-robot/week1/initial_hinge.gif" "Hinge Animation" "transformer-robot/week1/initial_wheel.png" "Wheel Design" >}}


## Flight Testing

I tested the drone with both 3S and 4S batteries. With a 3S, it can carry up to 250g and hover properly, but struggles to gain altitude beyond 366g. Switching to a 4S significantly improved performance, with a payload capacity of up to 750g.

### Stable Hover
{{< video "transformer-robot/week4/stable_hower.mp4" 640 360 >}}

















