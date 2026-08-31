---
title: "Robotic 3D Printing with a Meca500"
date: 2026-07-10T14:15:05+07:00
description: ROS 2 package driving a Meca500 6-DOF arm (5 μm resolution) as a 3D printer via MoveIt 2, with a custom ros2_control hardware interface and reachability-aware G-code execution
featureimage: meca500_3d_printer/output.gif
tags:
  - Meca500 6-DOF Arm
  - ROS 2
  - MoveIt 2
  - C++
  - Python
  - G-Code
  - Motion Planning
  - Robot Driver Development
draft: false
github: https://github.com/rishika2024/MECA500_3D_Printing
---

<style>
.video-row {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem;
  width: 100vw;
  margin-left: calc(50% - 50vw);
  margin-right: calc(50% - 50vw);
}
.video-row > figure {
  margin: 0 !important;
  flex: 1 1 320px;
  max-width: 400px;
}
</style>

## Overview

I built a ROS 2 package that drives a Meca500 6-DOF arm (5 μm resolution) as a 3D printer through MoveIt 2. A custom `ros2_control` hardware interface bridges the Meca500's proprietary TCP API to MoveIt 2, so real trajectory planning and execution run on the physical arm. A mounted Ender3 extruder handles filament feed and heating over USB serial — the arm does all of the XYZ motion.

The print pipeline sweeps the robot's reachable workspace, centers and clips sliced G-code onto the densest reachable region, then executes it through a patched build of MoveIt 2's Pilz Industrial Motion Planner. Consecutive moves of the same type are grouped into batches, and each batch is planned as one blended sequence:

* `LIN` for straight / extruding moves
* `CIRC` for arcs, with Pilz's colinearity tolerance lowered to match the Meca500's 5 μm resolution

Before the arm moves, a **pre-planning pass** runs the whole file plan-only to discover the real feed-rate range, so extrusion can be rescaled to the Ender3's physical limits.

{{< figure src="/meca500_3d_printer/full_setup.jpeg" alt="Full setup" width="60%" >}}

## System Architecture

{{< figure src="meca500_3d_printer/system.png" alt="System architecture diagram" >}}

## ROS 2 Package Breakdown

I gave every first-party package an `msr_` prefix to keep it separate from vendor code.

### msr_meca500_hardware

The `ros2_control` hardware interface (`Meca500System`) I wrote to bridge the Meca500's TCP API to MoveIt 2. It streams joint commands and state over control/monitoring ports 10000/10001; `use_mock_hardware:=true` swaps in `mock_components` for offline runs.

### msr_meca500_moveit

My MoveIt 2 configuration and `demo.launch.py`, which spawn `move_group`, `robot_state_publisher`, `ros2_control`, and the arm controller. I set up two planning pipelines: OMPL for free-space moves and a patched Pilz Industrial Motion Planner for `LIN` / `CIRC` / `PTP`.

### msr_meca500_robot

The robot description I modeled in URDF/Xacro — the arm, the mounted Ender3 extruder, the `nozzle` tool frame, and the print-bed environment. I made the extruder and background meshes toggleable (`use_extruder`, `use_background`) and swappable, so I can drop in a different end-effector or rig later.

### msr_meca500_print_pipeline

The orchestration package I wrote — four nodes plus the launch scripts:

* **`bed_from_touches`** — takes joint configurations recorded with the nozzle touching the bed, runs forward kinematics on each, and fits the bed plane by SVD. Publishes the result through `/table_service`. A flat default pose is used when no touch data is supplied.
* **`planningscene`** — holds the bed pose, publishes `/table_marker`, and registers the bed as a MoveIt collision object.
* **`reachability`** — sweeps an N×N grid over the bed for collision-free IK with an approach-standoff search, writing `reachable_points.csv`.
* **`gcode_print_executor`** — the print node. Parses the preprocessed G-code, groups moves into Print / Retract / Travel batches, runs the plan-only pre-planning pass to calibrate feed rates, then plans each batch as one blended Pilz sequence and executes it while driving the Ender3's extrusion and temperatures over serial. When a point won't plan, it retreats 1 cm, retries `LIN`/`CIRC`, and demotes a degenerate arc to a straight line — without ever skipping a commanded point.

Configuration lives in three YAML files: `machine_settings.yaml` (your hardware), `bed_settings.yaml` (touch-probe output), and `print_tuning.yaml` (author-tuned constants, including a first-layer `nozzle_bed_gap` trim).

### msr_gcode

A Python preprocessor I wrote that unzips the sliced `.3mf`, centers the print on the densest reachable region, drops out-of-workspace moves as gaps (no scaling or distortion), and repairs degenerate arc geometry — plus a C++ parser library the executor calls at run time.


## Demos

Below are clips of my setup printing a 5 cm × 5 cm × 5 cm cube on real hardware.

<div class="video-row">
{{< video src="meca500_3d_printer/real_cube2.mp4" ratio="9/16" >}}
{{< video src="meca500_3d_printer/real_cube3.mp4" ratio="9/16" >}}
{{< video src="meca500_3d_printer/real_cube1.mp4" ratio="9/16" >}}
</div>

In the RViz views below, the green line traces every sampled end-effector position and the purple line traces only where the nozzle was extruding. Both of the next two demos rely on my pipeline's reachability-aware placement to fit the print onto the workspace.

My full pipeline plans and executes a sliced boat model (21 layers) in batched Pilz `LIN` / `CIRC` sequences on a flat bed:

{{< video src="meca500_3d_printer/benchy_print.mp4" >}}

With no extruder, I tilt the bed to an arbitrary pose via `/table_service`, and my pipeline prints a cube (7 layers) onto the reoriented surface:

{{< video src="meca500_3d_printer/surface_trace.mp4" >}}

Straight-line G1 moves sent directly through `/goal_service` and executed with the Pilz `LIN` planner:

{{< video src="meca500_3d_printer/gcode_line.mp4" >}}

A smoke test of `msr_meca500_hardware`, driving basic robot motion through the `ros2_control` hardware interface:

{{< video src="meca500_3d_printer/moveit_test.mp4" >}}

## What's Next

Next up: adaptive nozzle orientation for steep struts and overhangs.

## Code

My full implementation is on [GitHub](https://github.com/rishika2024/MECA500_3D_Printing).
