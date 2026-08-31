---
title: "Lattice Print Path Planning with Reinforcement Learning"
date: 2026-08-10T14:15:05+07:00
description: RL based print path planning for 3D lattice structures using graph neural networks
featureimage: meca500_rl/cover_page.png
tags:
  - Reinforcement Learning
  - Graph Neural Networks
  - Path Planning
  - 3D Printing
  - Python
  - PyTorch
draft: false
---

This project tackles print path planning for lattice structures using reinforcement learning. Given a lattice (vertices + edges from LatticeQuery), the goal is to find the order to print every strut without the nozzle colliding with already printed material.

The approach is based on the AERO algorithm from [Weeks et al., *Adv. Mater.* 2022](https://doi.org/10.1002/adma.202206958), which uses Fleury's algorithm to find Eulerian paths through a lattice graph. This project reimplements the geometric constraint checking from the paper, then replaces the graph traversal algorithm with a learned policy using a Graph Attention Network (actor) and Graph Isomorphism Network (critic).

### Pre-computation (shared across all methods)

Before any path planning, two geometric checks run once on the lattice:

**Edge directionality.** Each strut's angle from vertical is computed. Struts within 30° of vertical must be printed top to bottom only (directed), since the nozzle body would collide with the strut if printing upward. All other struts are bidirectional.

**Blocking check.** Every pair of struts is checked: would printing strut A block future nozzle access to strut B? The check projects both struts onto the xy plane and examines parallel overlap, crossing intersections, shared vertex angles, and closest 3D distance. The result is a blocking table: for each strut, a list of struts it would block if printed now.

### AERO (baseline)

The paper's algorithm. Fleury's algorithm traverses the lattice graph, preferring non-bridge edges (edges whose removal would disconnect the remaining graph). When no valid edges remain at the current vertex, the nozzle lifts and travels to a new starting vertex. This repeats until all edges are printed.

On a 2×2×2 Simple Cubic lattice (27 vertices, 54 edges), AERO completes 54/54 edges in 20 separate paths with 19 travel moves.

{{< figure src="meca500_rl/aero.png" alt="AERO baseline result on a 2x2x2 Simple Cubic lattice" caption="AERO baseline: 54/54 edges printed in 20 paths with 19 travel moves." >}}

### Method 1 (RL, 4 features)

Replaces Fleury's algorithm with a GAT actor + GIN critic. The GAT scores each valid edge and outputs a probability distribution. The agent samples an edge, prints it, and receives a reward. The GIN evaluates the current graph state and predicts future reward, providing the learning signal for the actor via advantage based policy gradient.

Node features: `[x, y, z, is_nozzle_here]` (4 features). Travel is a learned action: when no valid edges exist at the current vertex, the GAT scores reachable vertices and picks a travel destination. Travel penalty: -2 per travel, -1 per print, +100 for completion.

**Observation:** Method 1 completes 54/54 but the travel count does not improve significantly over training. The network cannot distinguish between a state with 10 edges printed and 40 edges printed because the node features contain no information about print progress. The x, y, z coordinates never change, and the nozzle position alone is insufficient context.

{{< figure src="meca500_rl/meth1.png" alt="Method 1 (4-feature RL) result on a 2x2x2 Simple Cubic lattice" caption="Method 1: 54/54 edges printed, but travel count doesn't improve over training." >}}

### Method 2 (RL, 6 features, normalized)

Same architecture as Method 1 with two changes:

Node features expanded to 6: `[x/max_x, y/max_y, z/max_z, is_nozzle_here, printed_ratio, global_progress]`. The coordinates are normalized to [0,1] so they don't dominate the binary features. `printed_ratio` is the fraction of each vertex's adjacent edges that have been printed. `global_progress` is the fraction of all edges printed so far.

Travel penalty reduced to -1 (same as a print move). AERO needed 19 travels for 54 edges, so travel is a normal part of the solution, not a failure to penalize heavily.

**Observation:** Method 2 learns faster and achieves higher best reward. The printed_ratio feature lets the GAT see which parts of the lattice are "used up" and which still need work. The global_progress feature lets the GIN distinguish early game states from late game states, enabling different value estimates for the same local configuration at different stages of completion.

{{< figure src="meca500_rl/meth2.png" alt="Method 2 (6-feature normalized RL) result on a 2x2x2 Simple Cubic lattice" caption="Method 2: faster learning and higher best reward with progress-aware features." >}}

