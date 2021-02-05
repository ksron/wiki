# ROS2 Concepts

## Key Elements

### Graph

* a network of ROS2 elements processing data together at one time
* encompasses all excutable ans the connections

### Nodes

* responsible for a single, modular purpose
* can send and receive data to other nodes via topics, services, actions or parameters
* command lists:
  * `ros2 node list` : shows the names of all running nodes
  * `ros2 node info <node_name>` : shows informations about the node 

### Topics

* acts as a bus for nodes to exchange messages
* publisher-subscriber model communication method
* allow point-to-point, one-to-many, many-to-one, many-to-many ROS2 graph
* command lists:
  * `ros2 topic list` : show the list of all the topics currently activated in the system
  * `ros2 topic echo <topic_name>` : see the data being published on a topic
  * `ros2 topic info <topic_name>` : shows info. about the topic
  * `ros2 interface show <type>.msg` : shows message data details(structure)
  * `ros2 topic pub <topic_name> <msg_type> ‘<args>’` : publish data(‘<args>’) onto a topic directly 
  * `ros topic hz <topic_name>` : report the rate that the node publishes

### Services

* call-and-response model communication method
* send only the requested data
* allow one-to-one, one(server)-to-many(client)

### Parameters

* configuration value of a node
* data types: integers, floats, booleans, strings and lists
* All parameters are dynamically reconigurable

### Actions

* intended for long running tasks
* client-server model
  * client -(goal)-> server -(feedback & result)-> client
* consists of 3 parts: a goal, a result, and feedback
* built on topics and services
* preemptable
* steady feedback (topic)
* 

---

## References

[1] : [ROS2 Tutorial](https://index.ros.org/doc/ros2/Tutorials)   
[2] : 