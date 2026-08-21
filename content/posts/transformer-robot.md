---
title: "Teleoperated Ground-Air Morphing Robot"
date: 2026-03-31T14:15:05+07:00
description: A mobile robot that can operate both as a ground vehicle and as a drone
featureimage: transformer-robot/Final_Final.gif
tags:
  - CAD
  - UAV  
  - Pixhawk
  - Raspbery pi
  - Servo Motor
  
  
draft: false
github: https://github.com/rishika2024/transformer_bot

---

# Teleoperated Ground-Air Morphing Robot

## Overview

This is a teleoperated transformer robot that can switch between flight and ground modes. I started with an F450 drone frame and designed a hinge mechanism on each arm: when the arms are straight, the robot flies as a quadcopter powered by brushless motors; when the arms fold down, it becomes a wheeled ground vehicle with wheels driven by mini servo motors. The folding itself is also actuated by servo motors.

{{< video "transformer-robot/Final_Final.mp4" 640 360 >}}


## Full System Architecture:
{{< figure src="transformer-robot/System.png" alt="Flight mode" width="250%">}}


## CAD Models:

<b>Full Assembly </b>

{{< slide "transformer-robot/final_cad2.gif" "Aerial Mode" "transformer-robot/final_cad3.gif" "Ground Mode" >}}

<b>Hinge Mechanism </b>

{{< slide "transformer-robot/Final_HInge_mech.png" "Hinge Mechanism" "transformer-robot/Final_Hinge_mech2.png" "Hinge Mechanism"
"transformer-robot/final_cad4.gif" "Hinge Mechanism with the Motors" 
"transformer-robot/Final_hinge_side_closed.png" "Hinge Mechanism Side View" "transformer-robot/Final_Hinge_side_open.png" "Hinge Mechanism Side View" >}}

<b>Planetary Gear-like system for wheels</b>

{{< slide "transformer-robot/Final_planetary_wheel_side.png" "Side View" "transformer-robot/Final_planetary_wheel_top.png" "Top View" >}}



## Design Iterations

These are some of the iterations I went through before finalizing the design.

<b>Conceptual Hinge Mechanism</b>

{{< slide "m" "transformer-robot/week5/hinge_mech1.png" "view 1" "transformer-robot/week5/hinge_mech2.png" "view 2" "transformer-robot/week5/hinge_mech3.png" "view 3" >}}

<b>Wheel Assembly</b>

I initially mounted the wheel directly on the motor body, but the inertia on the brushless motors was too much and the wheels could not reach a reasonable RPM. This is why I designed a planetary gear-like system to decouple the wheel from the motor.

{{< slide "m" "transformer-robot/week6/wheel_assem1_complete.png" "v1 Full Assembly" "transformer-robot/week6/wheel_assem_1.png" "v1 Isometric" "transformer-robot/week6/wheel_assem2_complete.png" "v2 Full Assembly" "transformer-robot/week6/wheel_assem2.png" "v2 Isometric" >}}

<b>Wheel Design</b>

{{< slide "m" "transformer-robot/week6/wheel_v1.png" "v1" "transformer-robot/week6/wheel_v2.png" "v2" "transformer-robot/week6/wheel_v3.png" "v3" "transformer-robot/wheel_v4.png" "v4" "transformer-robot/wheel_v5.png" "v5">}}


## Flight Testing

I tested the drone with both 3S and 4S batteries. With a 3S, it can carry up to 250g and hover properly, but struggles to gain altitude beyond 366g. Switching to a 4S significantly improved performance, with a payload capacity of up to 750g.

### Stable Hover
{{< video "transformer-robot/week4/stable_hower.mp4" 640 >}}



















