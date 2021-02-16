# AutonomouStuff’s SSC Software

* AutonomouStuff vehicle platform
* simple interface to emulate typical human behavior
* receives ROS messages
  * **desired vehicle curvature**, max curvature, **desired speed**, acceleration limit and deceleration limit
* uses control algorithms to provide all required messages for lower level DBW(Drive-By-Wire) control system, such as [PACMod](./PACMod.md) 

### Main Features

1. Receive a desired curvature and calculate a desired steering wheel angle using a vehicle model defined by a set of user-configurable parameters and limits fed into a variable Ackermann-style steering model
2. Receive a desired speed and convert it into accelerator and brake pedal commands
   * Uses a set of user-configurable parameters (which are also provided, pre-tuned for the 450h, in a configuration file) as inputs and limits to a heavily-modified, multi-stage PID algorithm
3. Manages the enabled and disabled state of each individual system
   * automatically manages the desired gear based on the current gear, the requested speed, and the current system status
   * monitors and handles the failure of upstream and inter-node communication

---

## Refererences

[1] : [AutonomouStuff’s Product page](https://autonomoustuff.com/products/astuff-speed-steering-control-system)   
[2] : [Answer from ‘Control Lexus RX450h via AStuff PACMod w/o SSC’](https://discourse.ros.org/t/control-lexus-rx450h-via-astuff-pacmod-w-o-ssc/9860)  
