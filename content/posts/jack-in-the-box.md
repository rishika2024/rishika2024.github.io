---
title: "Jack-in-the-Box"
date: 2025-12-12T14:15:05+07:00
description: Simulating the dynamics of a jack in a shaking box from scratch
image: jack-in-box/Animation.gif
tags:
  - Dynamics
  - Python
draft: false
math: true
order: 2
github: https://github.com/rishika2024/Jack-in-a-Box
---

# Simulating the dynamics of a jack in a shaking box from scratch


## Outer Box Frames

{{< figure src="/jack-in-box/Box Frame.png" alt="Box Frames" width="65%" >}}

The frame at the center of the outer box is obtained by translating the world frame to the box center and then rotating it by the box orientation $\theta_{\text{box}}$.

## Jack Frames

{{< figure src="/jack-in-box/Jack Frame.png" alt="Jack Frames" >}}

The frame at the center of the jack is obtained by translating the world frame to the jack center and then rotating it by the jack orientation $\theta_{\text{jack}}$. The corner positions are defined relative to the jack center (fixed offsets), rotated by $\theta_{\text{jack}}$, and then expressed in the box frame.

## Kinetic Energy and Potential Energy

From the box and jack center frames, I compute the coordinates and their derivatives and use them to calculate kinetic and potential energy.

Kinetic energies are
$$T_{\text{box}} = \frac{1}{2} M (\dot{x}^2 + \dot{y}^2) + \frac{1}{2} J \omega^2,$$
$$T_{\text{jack}} = \frac{1}{2} m (\dot{x}^2 + \dot{y}^2) + \frac{1}{2} j \omega^2.$$

and the gravitational potential energies are
$$V_{\text{box}} = M g y, \qquad V_{\text{jack}} = m g y,$$
where $y$ is the vertical position of the corresponding center of mass.

## System Variables and Euler–Lagrange Equations

The system variables are
$[x_{\text{box}}, y_{\text{box}}, \theta_{\text{box}}, x_{\text{jack}}, y_{\text{jack}}, \theta_{\text{jack}}].$

The Lagrangian is defined as the difference between the total kinetic and total potential energy,
$$L = T_{\text{box}} + T_{\text{jack}} - (V_{\text{box}} + V_{\text{jack}}).$$

The Euler–Lagrange equation is
$$-\frac{\partial L}{\partial q} + \frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}} \right) = F,$$
where $q$ is the vector of system variables and $F$ is the force vector.

## Constraints

The motion is constrained so that all jack corners remain inside the outer box:

$$x_{\text{box}} - \frac{L}{2} < x_{\text{corner}} < x_{\text{box}} + \frac{L}{2},$$
$$y_{\text{box}} - \frac{L}{2} < y_{\text{corner}} < y_{\text{box}} + \frac{L}{2}.$$

A ground constraint is also applied so that the box does not move below the floor:
$$y_{\text{box}} \geq \frac{L}{2}.$$

## External Force and Torque

To make the system move, the box is given a desired angle that changes with time,
$$\theta_{\text{desired}}(t) = \frac{\pi}{15} + \frac{\pi}{3} \sin^3\left(\frac{t}{2}\right).$$

A torque is applied to the box that tries to make its actual angle $\theta_{\text{box}}$ follow this desired angle,
$$\tau_{\text{box}} = -15\,\big(\theta_{\text{desired}} - \theta_{\text{box}}\big).$$

Because the jack is connected to the box, it feels a restoring torque,
$$\tau_{\text{jack}} = -\frac{j}{J} \, \tau_{\text{box}}.$$

In the vertical direction, the box is given a constant downward force $F_{\text{box}} = M a$, producing a vertical acceleration $a$. A corresponding constant force $F_{\text{jack}} = -m a$ is applied to the jack in the opposite direction, using the same acceleration $a$.

So, the resulting force vector is as follows:
$$F = [0, F_{\text{box}}, \tau_{\text{box}}, 0, F_{\text{jack}}, \tau_{\text{jack}}].$$

## Active Constraints and Impact

During the simulation, each constraint $\phi_i(q)$ is evaluated at every time step. A constraint is treated as active when
$$|\phi_i(q)| < \varepsilon,$$
and inactive constraints use $\lambda_i = 0$.

When an active constraint indicates contact, I resolve a single dominant impact for that time step by enforcing constraint consistency and momentum balance.

## Graphs

{{< figure src="/jack-in-box/Graphs.png" alt="Dynamics Graph" >}}










 