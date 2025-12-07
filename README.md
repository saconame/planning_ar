
# Autonomous Driving

## Project Overview
This project implements a planning and control stack for an autonomous vehicle navigating mixed-traffic highway scenarios. The system is designed to execute safe lane change maneuvers while interacting with surrounding vehicles (which they do their own thing).

The solution utilizes a **sampling-based** approach in the **Frenet Frame**, allowing for the generation of dynamically feasible trajectories that optimize for safety and comfort.

## Problem Formulation
The agent operates within a simulated highway environment based on the **CommonRoad** framework.
- **Goal:** Navigate from an initial lane to a target lane within a limited time horizon.
- **Perception:** The agent receives LiDAR observations of surrounding vehicles).
- **Dynamics:** The vehicle follows a kinematic bicycle model subject to acceleration and steering limits.

## Methodology

### 1. Trajectory Sampling
The planner generates a manifold of candidate trajectories:
- **Lateral Planning:** Polynomials are sampled to transition from the current offset $d_0$ to various target offsets (e.g., center of current lane, center of target lane).
- **Longitudinal Planning:** Polynomials are sampled to achieve various target velocities or maintain distance to leading vehicles.
- **Re-planning:** The planner operates in a "receding horizon" way, re-planning every $T_{replan}$ seconds or when safety constraints (TTC) are violated.

### 2. Cost Evaluation & Selection
Each candidate trajectory is evaluated against a weighted cost function considering:
- **Safety:** Time-to-Collision (TTC) with predicted trajectories of surrounding vehicles.
- **Comfort:** Minimizing jerk and excessive lateral acceleration.
- **Feasibility:** Checking kinematic constraints (max steering angle, max acceleration).

## Demonstration

<div align="center">
  <a href="car.mp4">
    <img src="car.gif" alt="Highway Driving Demonstration" style="max-width:600px;">
  </a>
</div>

## Key Files
- `src/pdm4ar/exercises/ex12/planner.py`: Core logic for the `Planner` class, including the state machine for re-planning and emergency handling.
- `src/pdm4ar/exercises/ex12/sampler/frenet_sampler.py`: Generation of polynomial trajectories in Frenet coordinates.
- `src/pdm4ar/exercises/ex12/trajectory_evaluator.py`: Cost function implementation and collision checking.
- `src/pdm4ar/exercises/ex12/controller.py`: Low-level controller to track the selected trajectory.
