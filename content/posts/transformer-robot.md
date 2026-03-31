---
title: "Transformer Robot"
date: 2026-03-11T14:15:05+07:00
description: A mobile robot that can operate both as a ground vehicle and as a drone
image: transformer-robot/drone-with-wheels.png
tags:
  - UAV
  - Onshape
  - ROS2
  - Pixhawk
  - Raspbery pi
  
draft: false
order: 1

---

# Transformer Robot

## Overview

This is a teleoperated transformer robot that can switch between flight and ground modes. I started with an F450 drone frame and designed a hinge mechanism on each arm: when the arms are straight, the robot flies as a quadcopter powered by brushless motors; when the arms fold down, it becomes a wheeled ground vehicle with wheels driven by mini servo motors. The folding itself is also actuated by servo motors.



<div style="display:flex; gap:1rem; justify-content:center;">
  {{< figure src="transformer-robot/ground_moving.gif" alt="Ground Mode" width="200%" >}}
  {{< figure src="transformer-robot/Wheeled flight.gif" alt="Flight mode" width="200%">}}
</div>

## Full System Architecture:
{{< figure src="transformer-robot/System.png" alt="Flight mode" width="120%">}}


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

## Future Work

Adding mecanum wheels would allow it to move like an omnidrive, enabling full 360° ground movement without turning.
















