---
title: "Underwater Manipulator"
date: 2024-10-15T22:45:37+07:00
description: Developing a simulation and control system for an underwater robotic manipulator
image: underwater-arm/Robotic_Manipulator.gif
tags:
  - MATLAB
  - Arduino  
order: 5
draft: false

---

<style>
.side-by-side {
  display: flex;
  gap: 2rem;
  align-items: center;
  justify-content: center;
  margin: 2rem 0;
  flex-wrap: wrap;
}
.side-by-side figure {
  flex: 1;
  min-width: 300px;
  max-width: 45%;
}
</style>

# Developing a simulation and control system for an underwater robotic manipulator

#### This was a group project done in the Vibration Lab in IIT Jodhpur, Rajasthan, India under the guidance of Prof. Barun Pratiher and Nithin Tripathi

#### Team Members: Rishika Bera, Vellore Sahithi

This project focuses on building a simulation and control system for an underwater robotic arm designed to pick and place objects in submerged environments. The system combines servo motors, a gripper, encoders, and path-planning algorithms to achieve accurate and reliable object handling. A rack and pinion mechanism is used for vertical motion, and the whole setup was tested using both physical hardware and Simulink simulations.

## How the gripper works
The gripper is controlled using a stator motor. The linear displacement of the gripper depends on the angle of rotation of the motor. A min angle is set for grasping the object and a maximum angle is set for releasing the object.
The gripper was set such that there is a delay of few seconds after it grasps the object and drops it at the designated location. It was observed for every 10 degrees the gripper moves linearly by 0.5cm

{{< figure src="/underwater-arm/gripper.gif" alt="Gripper Working" >}}


## Rack and Pinion setup
A rack and pinion setup is used to move the gripper up and down.
The servo motor rotates the pinion, which drives the rack in a straight line. Since the gripper is attached to the rack, it moves vertically based on the motor's rotation, allowing it to align with objects at different heights.

<div class="side-by-side">

{{< figure src="underwater-arm/gripper_rack_pinion_cad.png" alt="Gripper CAD" >}}

{{< figure src="underwater-arm/gripper_rack_pinion.png" alt="Gripper Mechanism" >}}

</div>

## Encoder

1. Motor 1 uses an incremental optical encoder, which produces pulses as the motor rotates. These pulses help measure the motor’s position and speed accurately.
2. Motor 2 uses a Hall-effect encoder, which detects magnetic field changes from the rotating shaft to determine speed and direction, making it reliable for underwater use.

## Mounting the gripper

The gripper is mounted on the first link of the manipulator due to weight constraints.
The final setup includes the encoder linked to the first link and then to the rack and pinion casing.
The end effector is attached at the end of the rack.

{{< figure src="/underwater-arm/mounted_manipulator.png" width="75%">}}

## Final Demo

[{{< figure src="/underwater-arm/Robotic_Manipulator.gif" width="50%">}}](https://private-user-images.githubusercontent.com/172546714/534910357-35cca60f-55f5-460a-959c-c522ae3a753a.mp4?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjgyODA1NTgsIm5iZiI6MTc2ODI4MDI1OCwicGF0aCI6Ii8xNzI1NDY3MTQvNTM0OTEwMzU3LTM1Y2NhNjBmLTU1ZjUtNDYwYS05NTljLWM1MjJhZTNhNzUzYS5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYwMTEzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MDExM1QwNDU3MzhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02ZGZhYzQxYjEzYzhjNzFiODcwNjM0NDU5YjFkZWNkY2Q5NTk3MWM0MDdkYzBjYmM5N2E1NmJjMGY2YTJhNDAyJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Hs_cYNMHRqMDDDIEBxLf8lG3gqlfV7btm6FtxxxVaXg)






