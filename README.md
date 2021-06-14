# Waypoint Robotics
## Minimal API
Revised: November 2020

The following is a small subset of all available interfaces on Waypoint Robotics robots, sufficient for basic programmatic functionality such as retrieving the coordinates of existing waypoints, issuing navigation goals and checking their outcome and setting the state of Digital IO. Such basic functionality assumes that a map has already been created using the the robot's web graphical user interface, Dispatcher.

Some of the interfaces (topics, services or actions) listed below can only be used via ROSBridge and not directly through ROS, because they use custom message or service definitions. Such interfaces are marked with a * symbol. While examples on how to establish communication to ROSBridge using a basic websocket is provided in [this repository][examples-repo], use of a established ROSbridge clients such as roslibjs, roslibpy or jrosbridge is encouraged.


### /dispatcher/app_state
**ROS Interface Type:** Topic

**Type:** [std_msgs/String][std_msgs/String]

The current state of the robot/Dispatcher (Mapping, Navigation, etc). "nav_running" indicates the robot is fully ready to accept goals, "nav_uninitialized" means that the navigation has been started but the robot has not been localized (i.e., the initial position has not been provided).



### /start_navigation , /stop_navigation
**ROS Interface Type:** Service

**Type:** [Empty service][empty-service]

Start and Stop autonomous navigation mode.



### /initialpose
**ROS Interface Type:** Topic

**Type:** [PoseWithCovarianceStamped][pose-with-covariance-stamped]

Publish to this topic is used to set the robot’s initial position after starting navigation.



### /waypoint_db/retrieve_waypoint
**ROS Interface Type:** Service

**Type:** Custom*

Returns the coordinates of the specified waypoint and whether the service call was successful. 

Example Request:

    waypointName: 'Loading Dock'

Example Response:

```
success: True
pose: 
  position: 
    x: -6.97752303775
    y: 5.95625468367
    z: 0.0
  orientation: 
    x: 0.0
    y: 0.0
    z: 0.0
    w: 1.0
```



### /waypoint_move_base/goal , /waypoint_move_base/result
**ROS Interface Type:** Topic/[Action][actions]

**Interface Type:** [MoveBase Action][move-base-action]

Issuing of navigation goals and obtaining the result upon completion



### /waypoint_move_base/cancel
**ROS Interface Type:** Topic/[Action][actions]

**Interface Type:** [GoalID][goal-id]/[MoveBase Action][move-base-action]

Publish to this topic or use the ROS Action API to cancel an active navigation goal.



### /robot_pose
**ROS Interface Type:** Topic

**Interface Type:** [Pose][pose]

The coordinates of the robot's current location on the map (Navigation must be active).



### /modbus_manager/set_digital_output
**ROS Interface Type:** Service

**Type:** Custom*

Sets existing digital IO High or Low.

Example Request:

    "io_name: 'lights' value: false"



### /waypoint/aux_battery_soc
**ROS Interface Type:** Topic

**Type:** [std_msgs/Float32][float-32]

The Battery State of Charge: (Read Only)



### /waypoint/battery_is_charging
**ROS Interface Type:** Topic

**Type:** [std_msgs/Bool][bool]

Whether the Battery is charging



### /waypoint/embedded_response_active
**ROS Interface Type:** Topic

**Type:** [std_msgs/Bool][bool]

Whether E-Stop is active (Read Only)



### /waypoint/dtz_response_active
**ROS Interface Type:** Topic

**Type:** [std_msgs/Bool][bool]

Whether DTZ is active (Read Only)



### /mission_control/run_mission_from_file
**ROS Interface Type:** Service

**Type:** waypoint_ui/SingleWaypointOp* (this is a custom service type so it can only be used via ROSBridge)

Starts a mission previously created in Dispatcher

Example Request:

    "request: 'run delivery route'"


[examples-repo]: <https://github.com/waypointrobotics/rosbridge-websocket-examples>
[move-base-action]: <http://docs.ros.org/en/melodic/api/move_base_msgs/html/action/MoveBase.html>
[empty-service]: <http://docs.ros.org/en/melodic/api/std_srvs/html/srv/Empty.html>
[pose-with-covariance-stamped]: <http://docs.ros.org/en/api/geometry_msgs/html/msg/PoseWithCovarianceStamped.html>
[pose]: <http://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Pose.html>
[goal-id]: <http://docs.ros.org/en/kinetic/api/actionlib_msgs/html/msg/GoalID.html>
[actions]: <http://wiki.ros.org/actionlib>
[bool]: <http://docs.ros.org/en/api/std_msgs/html/msg/Bool.html>
[float-32]: <http://docs.ros.org/en/melodic/api/std_msgs/html/msg/Float32.html>
[std_msgs/String]: <http://docs.ros.org/en/melodic/api/std_msgs/html/msg/String.html>
