---
title: "Robotic 3D Printing with a Meca500"
date: 2026-07-10T14:15:05+07:00
description: A ROS 2 + MoveIt 2 package that turns a Meca500 6-DOF arm into a 3D printer
image: meca500_3d_printer/opening_pic.png
tags:
  - Meca500 6-DOF Arm
  - ROS 2
  - MoveIt 2
  - C++
  - Python
  - G-Code
  - Motion Planning
  - Robot Driver Development  
  - Reinforcement Learning
draft: false
github: https://github.com/rishika2024/MECA500_3D_Printing
---

# Robotic 3D Printing with a Meca500

## Overview

I built a ROS 2 package for controlling a Meca500 6-DOF robot arm (5 μm resolution) to perform robotic 3D printing via MoveIt 2. The project bridges the Meca500 proprietary API to MoveIt 2 through a custom ROS 2 hardware interface, enabling real trajectory planning and execution on physical hardware. 
The print pipeline sweeps the robot's reachable workspace, centers and clips sliced G-code onto the densest reachable region, and executes it move by move through a patched build of MoveIt 2's Pilz Industrial Motion Planner:

* `LIN` for straight/extruding moves
* `CIRC` for arcs, with the arc fitting tolerance lowered to match the Meca500's 5 μm resolution.

{{< figure src="/meca500_3d_printer/full_setup.jpeg" alt="Full setup" width="60%" >}}

## ROS 2 Package Breakdown

### meca500_hardware:

ROS 2 hardware interface bridging the Meca500 API to MoveIt 2 through ros2_control.

### meca500_moveit:

MoveIt 2 configuration and launch files for motion planning.

### meca500_robot:

Robot description in URDF/Xacro, including the mounted extruder end-effector and the `nozzle` tool frame.

### meca500_demo:

Reachability sweeping, planning scene setup, and the main print-execution node. Parses G-code moves and plans them through Pilz `LIN`/`CIRC`, recovering from occasional IK failures with Z-hopping for travel moves and midpoint bisection for extruding moves, without skipping a commanded point.

### gcode:

A Python preprocessing tool that centers a sliced print on the densest reachable region, drops moves outside the workspace as gaps, and validates and repairs arc geometry, plus a C++ parser library used by the trajectory node at execution time.

## Demos

In the RViz views below, the green line traces every sampled end effector position and the purple line traces only where the nozzle was extruding. The next two demos both rely on the pipeline's reachability-aware placement to fit the print onto the workspace.

With an extruder on a flat bed, the full print pipeline plans and executes a sliced Benchy boat (21 layers) move-by-move through Pilz `LIN`/`CIRC`:

{{< video "meca500_3d_printer/benchy_print.mp4" 640 360 >}}

With no extruder, the table is tilted to an arbitrary pose via `/table_service`, and the pipeline prints a cube (7 layers) onto the reoriented surface:

{{< video "meca500_3d_printer/surface_trace.mp4" 640 360 >}}

Straight-line G1 moves sent directly through `/goal_service` and executed via the Pilz `LIN` planner:

{{< video "meca500_3d_printer/gcode_line.mp4" 640 360 >}}

A smoke test of `meca500_hardware`, driving basic robot motion through the ros2_control hardware interface:

{{< video "meca500_3d_printer/moveit_test.mp4" 640 360 >}}

## What's Next

*  Generalizing the pipeline so any extruder can be mounted from just its URDF, with no hardcoded tool frame or offset
*  Moving from mock hardware to printing on the real robot
*  Reinforcement learning for adaptive tool orientation and extrusion

## Code

Full implementation on [GitHub](https://github.com/rishika2024/MECA500_3D_Printing).
