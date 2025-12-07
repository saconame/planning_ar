
# Spaceship trajectory planning via SCvx

## Project Overview
This project implements a robust planning and control stack for a spaceship agent operating in a 2D environment. The objective is to navigate through cluttered environments containing both static obstacles (planets) and dynamic "threats" (satellites) to reach a target state or perform a precise docking maneuver.

The core of the adopted solution is a **Sequential Convex Programming (SCP)** algorithm, specifically **SCvx** [^1].

## Problem Formulation
The spaceship is modeled as a rigid body controlled by a single gimbaled thruster. The system dynamics are non-linear, governed by:
- **State Space:** $X = [x, y, \psi, v_x, v_y, \dot{\psi}, \delta, m]^T$ representing position, orientation, linear/angular velocities, thruster angle, and fuel mass.
- **Control Inputs:** $U = [F_{thrust}, \dot{\delta}]^T$ representing thrust force and gimbal angular rate.
- **Constraints:**
  - **Actuation Limits:** Bounded thrust magnitude and gimbal angle.
  - **Dynamics:** Newtonian physics including mass depletion.
  - **Safety:** Hard collision avoidance constraints for planets and moving satellites.
  - **Terminal Conditions:** position, velocity, and orientation targets (for docking).

## Demonstration

<table>
  <tr>
    <td align="center"><strong>Planetary Navigation</strong></td>
    <td align="center"><strong>Satellite Avoidance</strong></td>
  </tr>
  <tr>
    <td align="center">
      <a href="planets.mp4">
        <img src="planets.gif" alt="Planetary Navigation" style="max-width:300px;">
      </a>
    </td>
    <td align="center">
      <a href="satellites.mp4">
        <img src="satellites.gif" alt="Satellite Avoidance" style="max-width:300px;">
      </a>
    </td>
  </tr>
</table>

## Files
- `src/pdm4ar/exercises/ex11/planner.py`: Implementation of the `SpaceshipPlanner` class, A* initialization, and the SCvx loop.
- `src/pdm4ar/exercises/ex11/discretization.py`: Methods for ZOH and FOH discretization of dynamics.
[^1]: Danylo Malyuta et al.,  "Convex Optimization for Trajectory Generation", 2021, doi: [10.1109/MCS.2022.3187542](https://arxiv.org/abs/2106.09125)
