# Heron USV

* Developed by Clearpath Robotics[1]

---

## Heron Project File Structure
### heron
Common packages for Heron

#### heron_control
Control package for Heron

#### heron_description
URDF description for Heron

#### heron_msgs
Provides standard messages specific for Heron,
especially for the MCU's rosserial interface
* msgs/Course.msg: command message(desired yaw, velocity)
* msgs/Drive.msg: command thrust message to MCU(left/right motor power)
* msgs/Helm.msg: command thrust capcity & turn rate message used for Moos-IVP(desired yaw rate, thrust)
* msgs/Sense.msg: MCU status messge(battery, motor status(A), etc
* msgs/Status.msg: system status message(uptime, temp, power status)

### heron_controller
PID controller for Heron

#### config
* heron_controller.yaml: Tuning for the Heron PID controller(fwd_vel, yaw_rate, yaw, max)

#### include
Contains header files for heron_controller

#### src
* controller.cpp
	* subscribes 'cmd_vel', 'cmd_wrench', 'cmd_helm', 'cmd_course', 'odometry/filtered'
	* publishes 'cmd_drive'
* force_compensator.cpp: calculates thrust electronic input into the motor controller


### heron_robot
Robot packages for Heron

#### heron_base
Heron's mobility and sensor base
* config: default configuration on location(magnetic centre)
* env-hooks
* launch/base.launch
* scripts/snmp_lights.py

#### heron_bringup
Provides upstart setup for Heron, and launch files for the standard config.   
Provides config. for u-blox GPS receiver(part of Heron's sensor suite)   
**Run_depend**: [axis_camera](http://wiki.ros.org/axis_camera), *heron_base*, [lms1xx](http://wiki.ros.org/LMS1xx), [nmea_comms](http://wiki.ros.org/nmea_comms), [nmea_navsat_driver](http://wiki.ros.org/nmea_navsat_driver), [robot_upstart](http://wiki.ros.org/robot_upstart)
* scripts/calibrate_compass: runs general system check and compute magnet calibration
* scripts/compute_calibration: Process IMU bag file for compass calibration
* scripts/install: install launch driver components of Heron's supported accessories & Heron base components
* scripts/navsat_rtk_relay
* scripts/netserial
* scripts/setup: install udev rules, configure ublox GPS

#### heron_nmea
[heron_nmea](https://github.com/heron/heron_robot/tree/kinetic-devel/heron_nmea/REFERENCE.md) package for users to control and receive data from the Heron in the form of NMEA sentences

#### heron_robot
Metapackage for the default behavior of Heron   
* Run_depend: heron_bringup, heron_controller, heron_firmware, heron_nmea

### heron_simulator
Simulator package for Heron USV   
Uses ROS Kinetic and Gazebo 7

---

## ROS Github Review
### Messages
#### heron_msgs/Course.msg
```
# Command an absolute yaw and velocity.

# Yaw is specified in radians counter-clockwise from true east.
float32 yaw

# Velocity is specified in meters/s. Negative values correspond to Heron
# reversing.
float32 speed
```

### heron_msgs/Drive.msg
```
# Command thrust amount to Heron thruster,
# transmitted from higher-level software to the MCU 
# on the /cmd_drive topic.

# Thrust amount ranges from [-1.0..1.0], where 1.0 pushes Heron forward.
float32 left
float32 right
```

### heron_msgs/Helm.msg
```
# Command a percentage amount of total thrust capacity, and an turn rate.
# On a conventional craft, turn rate would map to rudder.

# Thrust amount ranges from [-1.0..1.0], where 1.0 pushes Heron forward.
float32 thrust

# Yaw rate specified in radians/sec, where positive values cause Heron
# to turn toward the port deck. The controller will use feedback from the
# IMU's gyroscopes to attempt to match the command rate.
float32 yaw_rate
```

### heron_msgs/Sense.msg
```
# General MCU status for Heron transmitted from the MCU
# to higher-level software on the /sense topic.

Header header

# Voltage level of battery, in volts
float32 battery

# Instantaneous current drawn by each motor, in amps.
float32 current_left
float32 current_right

# Bitfield represents status of hobby R/C override.
uint8 RC_INRANGE=1
uint8 RC_INUSE=2
uint8 rc

# Pulse lengths received from the three R/C channels.
uint16 rc_throttle
uint16 rc_rotation
uint16 rc_enable
```

### heron_msgs/Status.msg
```
# Specific system status data, transmitted by the MCU at 1Hz on status topic.

Header header

# Commit of firmware source.
string hardware_id

# Times since MCU power-on and MCU rosserial connection, respectively.
duration mcu_uptime
duration connection_uptime

# Temperature of PCB as measured by internal AVR thermometer,
# reported in degrees centigrade.
float32 pcb_temperature

# Current sense available on platform, in amps.
# Averaged over the message period.
float32 user_current

# Integration of power consumption since MCU power-on, in watt-hours.
float32 user_power_consumed
float32 motor_power_consumed
float32 total_power_consumed
```

### geometry_msgs/Wrench.msg
```
# This represents force in free space, separated into
# its linear and angular parts.
Vector3  force
Vector3  torque
```

### geometry_msgs/Twist.msg
```
# This expresses velocity in free space broken into its linear and angular parts.
Vector3  linear
Vector3  angular
```

### geometry_msgs/Vector3.msg
```
# This represents a vector in free space.
# It is only meant to represent a direction. Therefore, it does not
# make sense to apply a translation to it (e.g., when applying a
# generic rigid transformation to a Vector3, tf2 will only apply the
# rotation). If you want your data to be translatable too, use the
# geometry_msgs/Point message instead.

float64 x
float64 y
float64 z
```

### Heron

### heron_robot



### heron_controller
#### `src/force_compensator.cpp`
* Class ForceCompensator(ros::NodeHandle &n): node_(n)
    * +cmd_pub_: "cmd_drive" topic publisher
        * msg type: heron_msgs::Drive
    * +eff_pub_: "eff_wrench" topic publisher
        * msg_type: geometry_msgs::Wrench
    * +calculate_motor_setting(double thrust): takes in a thrust req. and returns the electoronic input required (into motor controller_
    * saturate_thrusters(double thrust)
    * pub_thrust_cmd(geometry_msgs::Wrench output): takes in a wrench command and returns cmd_drive msg
        1. calculate guarantee the bound of yaw torque
	2. calculate max yaw torque for left/right thurst
	3. calculate max allowed thrust
	4. calculate electronic output for motor controller
	5. publish command
	6. calculate effective wrench being sent out & update

#### `src/controller.cpp`
**Class Controller(ros::NodeHandle &n): node_(n)**
1. 
2. 
3. Setup for Fwd Vel controller, Yaw Rate controller, Yaw controller

**main**
1. initialize controller
2. initialize Subscribers
    * "cmd_vel"
	* "cmd_wrench"
	* "cmd_helm"
	* "cmd_course"
	* "odometry/filtered"
3. initialize Timer
    * control_output timer: duration(1/50), call control_update
	* console_update timer: duration(1), call console_update




---

### References
[1] : [Clearpath Robotics Heron USV](https://clearpathrobotics.com/heron-unmanned-surface-vessel/)   
[2] : [Heron USV Github](https://github.com/heron)   
[3] : [
