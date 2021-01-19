# Autoware

## Overview[3]

* Autoware is the world's first "all-in-one" open-source software for self-driving vehicles hosted under the Autoware Foundation
* Autoware first began with the Autoware.AI project, based on [ROS 1](./ROS.md#ros11).
* The Autoware.Auto project, based on [ROS 2](./ROS.md#ros2), is the next generation successor of Autoware.AI.

### Autoware.AI[4] 
* Autoware.AI is ROS-based open-source software
* **Localization** depends on 3D high-definition map data and the NDT algorithm. The result of Localization is complemented by the Kalman Filter algorithm, using odometry information obtained from CAN messages and GNSS/IMU sensors.
* **Detection** is empowered by camera and LiDAR devices in combination with 3D high-definition map data. The Detection module uses deep learning and sensor fusion approaches.
* **Tracking and Prediction** are realized with the Kalman Filter algorithm and the lane network information provided by 3D high-definition map data.
* **Planning** is based on probabilistic robotics and rule-based systems, partly using deep learning approaches as well.
* **Control** defines motion of the vehicle with a twist of velocity and angle (also curvature). The Control module falls into both the Autoware-side stack (MPC and Pure Pursuit) and the vehicle-side interface (PID variants).

### Autoware.Auto[3]
Two major differentiators:

1. Improved system architecture and module interface design (including messages and APIs)
2. An emphasis on reproducibility and determinism at the library, node, and system levels

---

### References
[1] : [Autoware.AI](https://www.autoware.ai/)   
[2] : [Autoware.Auto](https://www.autoware.auto/)   
[3] : [Autoware.Auto Gitlab Manual](https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/)   
[4] : [Autoware.AI Wiki](https://github.com/Autoware-AI/autoware.ai/wiki)   

