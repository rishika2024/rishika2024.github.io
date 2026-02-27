---
title: "Transformer Robot"
date: 2026-02-24T14:15:05+07:00
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

The goal of this project is to design and build a mobile robot that can operate both as a ground vehicle and as a drone, enabling versatile movement across different terrains and environments. 

This is an ongoing individual project being built as part of the Master of Science in Robotics (MSR) program at Northwestern University.

## Project Updates

### Week 7

Printed the wheels and tested it

*  Gear Meshing
{{< video "transformer-robot/week7/Wheel_gear.mp4.mp4" 640 360 >}}

*  Flight testing
{{< video "transformer-robot/week7/Wheeled flight .mp4" 640 360 >}}


### Week 6

Tested the wheels using both 3S and 4S batteries; however, the RPM was insufficient, and the drone was unable to take off. With a 4S battery, the payload capacity was tested and found to be 750g. Additionally, I designed a planetary gear-like system for the wheels to decouple them from the DC motor body.

*  Flight test with the wheels and 4s battery

{{< video "transformer-robot/week6/Flight_test_with_wheels.mp4" 640 360 >}}

*  Wheel designs
{{< slide "transformer-robot/week6/wheel_v1.png" "Wheel Design 1" "transformer-robot/week6/wheel_v2.png" "Wheel Design 2" "transformer-robot/week6/wheel_v3.png" "Wheel Design 3" >}}

*  Planetary Gear-like system for wheels
   *    Design 1
   {{< slide "transformer-robot/week6/wheel_assem1_complete.png" "Complete Assembly with Motor and Propeller" "transformer-robot/week6/wheel_assem_1.png" "Isometric View" "transformer-robot/week6/wheel_assem1_side.png" "Side View" "transformer-robot/week6/wheel_assem1_top.png" "Top View">}}

   *    Desgin 2
   {{< slide "transformer-robot/week6/wheel_assem2_complete.png" "Complete Assembly with Motor and Propeller" "transformer-robot/week6/wheel_assem2.png" "Isometric View" "transformer-robot/week6/wheel_assem2_side.png" "Side View" "transformer-robot/week6/wheel_assem2_top.png" "Top View">}}





### Week 5

Designed the hinge mechanism

{{< slide "transformer-robot/week5/hinge_mech1.png" "Hinge View1" "transformer-robot/week5/hinge_mech2.png" "Hinge View2" "transformer-robot/week5/hinge_mech3.png" "Hinge View3" >}}

### Week 4


Determined the payload capacity of the drone and designed a wheel to mount on the motor body below the propeller.

With a 3S battery, the drone can carry an extra payload of up to 366g—it is able to move but cannot gain significant altitude, even at full throttle. When the payload is reduced to 250g, the drone is able to hover properly.

*  Stable Hower
{{< video "transformer-robot/week4/stable_hower.mp4" 640 360 >}}

*  250g payload
{{< video "transformer-robot/week4/250_3.mp4" 640 360 >}}

*  366g payload
{{< video "transformer-robot/week4/366g.mp4" 640 360 >}}

{{< slide "transformer-robot/week4/wheel1.png" "Wheel View1" "transformer-robot/week4/wheel2.png" "Wheel View2" >}}



### Week 3
Got the drone flying 

{{< video "transformer-robot/week3/proper flight.mp4" 640 360 >}}

### Week 2

Assembled the drone kit (from Hawks drone kit)
{{< figure src="transformer-robot/week2/initial_assembly.png" alt="initial assembly" width="40%">}}

### Week 1

A simple conceptual CAD model

{{< slide "transformer-robot/week1/initial_cad.png" "Initial CAD Design" "transformer-robot/week1/initial_cad.gif" "CAD Animation" "/transformer-robot/week1/initial_hinge.png" "Hinge Design" "/transformer-robot/week1/initial_hinge.gif" "Hinge Animation" "transformer-robot/week1/initial_wheel.png" "Wheel Design" >}}









