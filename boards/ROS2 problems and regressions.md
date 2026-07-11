**Martin Pecka**'s notes: https://gist.github.com/peci1/52aea17faf5f7e33c8096b75dc9bcd2f

## TUI
- no `roscd`
- calling services got complicated (filling json dictionaries by hand sucks)
- no colors without exporting a variable (`export RCUTILS_COLORIZED_OUTPUT=1`)
- no code completion without modifying the environment
```	
eval "$(/usr/bin/register-python-argcomplete ros2)"
eval "$(/usr/bin/register-python-argcomplete colcon)"
```
- when switching RMW, the ROS daemon needs to be killed (`pkill -9 -f ros && ros2 daemon stop`)
- code completion suggest only the launch files that end with `.launch.py`
## colcon
- no colors
- no build-in profiles (mixin exists, but they need to be setup, are stored externally in a file)
- `colcon build` makes new workspace if not called in the root of the workspace
- including headers does not prioritize the ones in the workspace (https://colcon.readthedocs.io/en/released/user/overriding-packages.html#sort-include-directories-according-to-the-workspace-order)
- no direct control of the number of build threads without exporting an environment variable (export MAKEFLAGS=-j3)
### Launch
- respawning does not work
- not crashing in the launch file will not cause the launch file to crash
- launch files need to be "grouped" while being included, otherwise the arguments are going to be overlapping
- when node is started using ros2 launch, it does not have direct access to TTY, which make using libraries such as ncurses impossible
- launch files can not be launched using a relative path, they need to be part of a package installed in the install space
## Component Containers
- The container some times exists with nonzero exit code, nothing found in backtrace. This often happens during sig-terming the node, so checking the return code in the integration tests is not viable.
- The component can not be killed "nicely" from within itself
## FastRTPS
- Causes the node to freeze (slow down significantly) when service being called from another computer
## Zenoh
- When the router dies before other nodes (e.g., when stopping an integration test), the other nodes crash
- bug with data leakage, should be fixed with the release of rmw_zenoh=0.2.8.
## Parameters
- All parameters are shown in rqt_reconfigure and can not be removed, only "greyed out"
- All components are getting the callback for changing parameters, even for other component's parameters
- Missing dynamic parameter grouping from rqt_reconfigure
- Missing enums from dynamic parameters
- Cryptic error when loading an empty config file
- rqt_reconfigure does not show any params for the node if there is a param without a value
- if you set parameter range to, e.g., <0, 100>, it will let through -inf and inf :-D (https://github.com/ros2/rclcpp/issues/2898)
- Loading of "optional" parameters is broken: They can not be undeclared (https://answers.ros.org/question/395760/) if they were not loaded. This then brakes rqt_reconfigure.
## rclcpp
- the generic topic subscriber now needs to know the topic type in runtime
- the official Timer implementation is very CPU heavy (5x over ROS1, 4x over custom threaded implementation)
- comparing incompatible times will throw an exception! this is easy to miss or cause during runtime. Why is C++ library for realtime robotics throwing so many exceptions?
- service call is by default async, the sync one needs to be implemented manually; the boiler plate code for both is pretty large
- cannot change period of ROS Timers when they already exist
- the need to implement your own "onInit()" using a thread or a Timer
	- **update!** can be solved elegantly by having the node as a member variable of a "Wrapper", e.g., mrs_lib::Node.

## rclpy
- filling Int type into a Double type of a ROS message will cause runtime error
- no AnyMessage subscriber, you need to know the type when subscribing

## Pluginlib
- Does not kill the plugins nicely when exiting
	- You need to implement a shutdown callback in the parent node and reset the plugin's pointers by hand
## rosbag
- rosbag play does not show the progress anymore

## Documentation
- https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Domain-ID.html
	- nice philosofical info about the Domain ID, but ... how do you set it? There is no practical example. Is it an env variable? Is it and argument for the node? Who know?

## Rviz
- Not able to visualize compressed images in Jazzy

## Image Transport
- python implementation missing on Jazzy