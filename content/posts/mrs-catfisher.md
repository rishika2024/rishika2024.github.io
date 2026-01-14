---
title: "Mrs. Catfisher - The Bug Sorter"
date: 2025-12-12T14:15:05+07:00
description: Catching moving bugs using the Franka Arm
image: mrs-catfisher/bug_sorter.gif
tags:
  - Franka Arm
  - ROS2
  - Onshape
  - Moveit
  - OpenCV Vision
  - Python
draft: false
math: true
github: https://github.com/rishika2024/Bug-sorter.git
order: 1

---

# Mrs. Catfisher - The Bug Sorter

{{< figure src="/mrs-catfisher/franka picking up a bug.png" alt="Franka picking up a bug" width="60%">}}

This was a group project by Halley Zhong, Nolan Knight, Miguel, and myself for **Northwestern's MECH_ENG 450: Embedded Systems in Robotics** course. We built an autonomous system to sort moving HexBugs using the Franka Arm. My role was to develop the vision system, which can detect, uniquely label, and track the moving bugs in real time.

The robot is controlled through the MotionPlanningInterface, which has three main components:

1. **RobotState** – keeps track of the robot’s current position.
2. **MotionPlanner** – plans safe movements for the robot.
3. **PlanningScene** – monitors obstacles to prevent collisions.

On top of this, the TargetDecision and Sort nodes use the vision system to locate colored bugs, decide which one to pick next, and move it to its designated spot.

{{< figure src="/mrs-catfisher/vision.jpeg" alt="Vision module" width="60%">}}

Because the HexBugs move in real time, the system can bypass MoveIt and control the robot joints directly using the fer_arm_controller. This allows the Franka Arm to follow moving targets smoothly and efficiently.





