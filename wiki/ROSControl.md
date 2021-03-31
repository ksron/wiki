# ROS Control

## Overview

### Goals

* Lower entry barrier for exposing HW to ROS
* Promote resue of control code
* Provide ready-to-use tools
* Real-time ready implementation

### hardware_interface
The hardware interfaces are used by ROS control in conjunction with one of the available ROS controllers to send(`write()`) commands to the hardware and receive(`read()`) joint states from it.

* Robot HW abstraction
    * SW representation of robot
    * Abstracts HW(resource, interface, robot)
    * Handles resource conflicts
* Resources and interfaces
    * Read-Only : Joint state, IMU, Force-torque sensor
    * Read-Write : Position joint, Velocity joint, Effort joint

### Controllers

* Dynamically loadable plugins
* Interface defines a simple state machine
* Interface clearly separates (Non-RT / RT safe operations)
* Computation:
    * Controller update: periodic, RT safe
    * ROS API callbacks: asynchronous, non-RT


---

### References

[1] : ["ROS Control, and overview", ROSCon 2014](https://roscon.ros.org/2014/wp-content/uploads/2014/07/ros_control_an_overview.pdf)   
[2] : [ROS 로봇 프로그래밍](https://www.notion.so/2-5-ros_control-8f81e547ef384b278de7322ece07be0b)   
[3] : [ros_control package](http://wiki.ros.org/ros_control)   
