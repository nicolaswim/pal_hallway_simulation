
# PAL Hallway Simulation — Social Navigation Stress Tests

High-stress hallway scenarios for validating **social navigation** on a PAL TIAGo (or any ROS 2 robot) in Gazebo/RViz.  
The environments progressively challenge a robot’s ability to navigate safely and *socially* — aligning with human flow, yielding correctly, maintaining personal space, and scaling to crowded scenes.

> Developed for the thesis *“A Framework for the Validation of Social Navigation Algorithms in Simulated Care Environments.”*

---

## Overview

Robots in healthcare must not only avoid collisions but also respect **social dynamics** such as proxemics, group formations, and crowd flow. These worlds implement reproducible validation scenarios grounded in empirical pedestrian and wheelchair dynamics research (Helbing et al., 1995; Kretz et al., 2006; Zanlungo et al., 2011; Blue & Adler, 2001; Johnson et al., 2013).  
They provide a structured testbed to assess the **Social Momentum MPPI** or any social navigation planner through measurable Key Performance Indicators (KPIs).

---

## Worlds and Scenarios

### Scenario 1 — Bidirectional Flow Gauntlet
Long corridor (2.5 m × 20 m) with two opposing human flows containing nurses, patients, and a wheelchair user.  
Tests the robot’s ability to form or join **emergent pedestrian lanes** and maintain safe passing distances at different approach speeds (Helbing et al., 1995).  

**KPIs:** lateral deviation from lane centre, minimum clearances, PSI (personal-space intrusion), jerk and acceleration smoothness.

---

### Scenario 2 — Crossroads Dilemma
T-intersection where a **rushing nurse**, a **powered wheelchair**, and a **visitor overtaking from behind** create a right-of-way conflict.  
Evaluates predictive planning at **blind corners**, speed negotiation, and yielding priority (Seyfried et al., 2009; Hoogendoorn & Daamen, 2005).  

**KPIs:** decision timing, conflict avoidance, time-to-goal versus social cost.

---

### Scenario 3 — Clustered Obstacle Course
Two static nurses stand mid-corridor engaged in conversation, forming an **o-space** group (Kendon, 1990).  
A **manual wheelchair user** exits a doorway and performs a multi-point turn exhibiting **lateral drift** during turning (Cooper et al., 2008).  
The robot must route around the group rather than through it and maintain clearance from the drifting wheelchair.

**KPIs:** group-cutting violations, clearance to turning wheelchair, freeze duration, motion smoothness.

---

### Scenario 4 — Converging Emergency
A slow patient ahead and a high-speed nurse approaching from the opposite direction create a potential collision corridor.  
The robot must predict and mitigate the interaction—by slowing, positioning itself protectively, or increasing distance (Helbing et al., 2000).  

**KPIs:** pre-emptive deceleration, minimum three-agent separation, time-to-goal.

---

### Scenario 5 — Scalability and Crowd Clusters
Two dense human clusters plus random background motion test how the algorithm scales with many simultaneous actors.  
Inspired by crowd-robot interaction studies and benchmarks such as CrowdNav and Arena-ROSnav (Chen et al., 2019; Zamora et al., 2022).  

**KPIs:** navigation success rate versus number of actors, throughput, average PSI, computation latency.

---

## Structure

```

pal_hallway_simulation/
├── launch/        # launch files per scenario
├── worlds/        # Gazebo world files (1–5)
├── models/        # human & wheelchair models
├── INFO.txt
└── build/, install/, log/ (ignored)

````

---

## Key Performance Indicators

| Category | Metrics |
|-----------|----------|
| **Task Efficiency** | Time to goal, path length, deviation from optimal |
| **Smoothness** | Linear / angular velocity, acceleration, jerk |
| **Social Compliance** | Minimum clearance, PSI integral (< 1.2 m zone), group-cutting flag |
| **Scalability** | Success rate vs. actors, deadlocks, CPU load |

---

## Implementation Notes

- Corridor width: 2.5 m (standard hospital hallway).  
- Wheelchair turning radius: ≈ 1.1 m; allow +10 % buffer for lateral drift (Cooper et al., 2008).  
- ADA turning space guideline: 1.525 m diameter (U.S. Access Board, 2010).  
- Actors: animated Gazebo models with stochastic velocity noise for realism.  
- Visualise social and wheelchair spaces in RViz (e.g., 1.2 m personal space ring, 1.525 m turn circle).

---

## Useful Commands

### Launching your world with Scenario 1 launch file
```bash
ros2 launch pal_gazebo_worlds scenario_1.launch.py world_name:=my_world
````

### Launch with PAL TIAGo

**With arm**

```bash
ros2 launch tiago_gazebo tiago_gazebo.launch.py navigation:=True is_public_sim:=True world_name:=my_world
```

**No arm**

```bash
ros2 launch tiago_gazebo tiago_gazebo.launch.py navigation:=True is_public_sim:=True world_name:=my_world arm_type:=no-arm
```

**With NAV2**

```bash
ros2 launch tiago_gazebo tiago_gazebo.launch.py navigation:=True is_public_sim:=True world_name:=scenario_2 slam:=True
```

or

```bash
ros2 launch tiago_gazebo tiago_gazebo.launch.py navigation:=True is_public_sim:=True world_name:=scenario_2
```

---

## References (text only)

* Helbing D., Molnár P. (1995). Social force model for pedestrian dynamics. *Physical Review E*.
* Kretz T. et al. (2006). Experimental study of pedestrian counterflow. *J. Stat. Mech.*
* Hoogendoorn S., Daamen W. (2005). Pedestrian behavior at bottlenecks. *Transportation Science*.
* Kendon A. (1990). Conducting interaction: Patterns of behavior in focused encounters.
* Cooper R. et al. (2008). Wheelchair kinematics and turning characteristics. *Arch. Phys. Med. Rehab.*
* U.S. Access Board (2010). ADA Accessibility Guidelines.
* Chen C. et al. (2019). Crowd-Robot Interaction: Benchmark and dataset. *IEEE IROS*.
* Zamora I. et al. (2022). Arena 3.0: Scalable crowd navigation benchmark. *arXiv preprint*.
* Helbing D. et al. (2000). Simulating collective pedestrian dynamics. *Nature*.
* Blue V., Adler J. (2001). Cellular automata model of bidirectional pedestrian flows. *Transportation Research B*.
* Johnson D. et al. (2013). Human proxemics and robot approach behaviors. *Human Factors*.
* Zanlungo F., Ikeda T., Kanda T. (2011). Potential-based modeling of proxemic behavior. *Robotics and Autonomous Systems*.


