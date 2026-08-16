## Minor TODO

- [ ] Although the estimation manager checks for data on the "control input" topic, the particular estimator might be subscribing a wrong topic, therefore, you might miss the error of bad remapping
- [x] Mavros config needs different configs for Garmin with old (id 33) and new fw (id 0, pitch_270)
- [ ] Batch visualizerer topics are not shown in rviz (**MrsLib**)
- [ ] tf for rtk antenna currently hardcoded in transform manager's launch file (**TransformManager**)
- [ ] Test for "Transform Reference List" (**ControllManager**)
- [ ] Documentation: **add information about Zenoh**
## makeprg compilation of a package in neovim

- `:make` in vim should trigger `colcon buiild` of the current packaage
	- we need to be able to detect which package does the currently opened file originates from
## Key-signed disarming
- implement key exchange mechanism for the disarming service: the caller should first get a random key from the hw api that will allow him to disarm. This will prevent a random caller from disarming the drone in mid air.

- [ ] implementation
- [ ] tests

## Controller and Tracker timeouting

- [ ] implementation
- [ ] tests

## Proper handling of UTM zones

- [ ] implementation
- [ ] tests

## Landing detection for all output modalities

Think of a way how to detect that we landed for all output modalities...

- [ ] implementation
- [ ] tests

## PCL specialization of mrs_lib (Transformer)
- [ ] implementation
- [ ] tests

## Speed tracker (Rethink and refactor?)

- I would rethink it as "Passthrough tracker" using the TrackerCommand

## MRS UAV Status

- the remote mode should use desired velocity instead of relative position command

## MRS Multirotor simulator plugin API

- allow loading UAV-plugins and WORLD-plugins
- [ ] implementation
- [ ] tests

## MidAir activation

- parametrize the sleep after arming by the rate if incoming odometry

## Launch file

- make debug_roslaunch conditional in the launch files

## Remote controller abstraction
- from HW API -> Control manager

## Ouster driver
- [ ] TF frames should be prefixed by "$UAV_NAME/"
- [ ] The parameters should not use the "/$UAV_NAME/ouster" prefix
- [ ] Set as default: timestamp_mode: 'TIME_FROM_ROS_TIME'

## PX4 API
- [ ] replicate the UTM fix https://github.com/ctu-mrs/mrs_uav_px4_api/pull/3

## ControlManager diagnostics
- [ ] trackers and controllers should be able to invoke publishing of ControlManager's diagnostics when there is a significant change in the state

## Basic tests sometimes produce segfaults
- [x] added more prints to min_height_check
- [ ] seems to fail near the end, when flying normally check should pass
- [ ] check later if it fails again
- [ ] e.g., min_height_check, goto_fcu_service

## [Docu] State machine diagrams of internal processes

- [ ] takeoff
- [ ] landing
- [ ] landing home
- [ ] escalating failsafe
	- [ ] failsafe
	- [ ] eland
- [ ] trajectory following
- [ ] rc remote mode

## Incorporate Yaml remapper from Jakub
- [ ] use in core packages
- [ ] use in example packages

## We are not using Service Server Handler, why?

## Explicitly set callback groups
- [x] core
- [ ] flightforge
- [ ] hw api

## MRS package version printed during system startup

- [ ] done

## Luxonis's depth-AI ROS

- [ ] fork or create a wrapper package and fix the launch files
	- [ ] it is publishing outside of its namespace

## Make mrs sim stepable

- [ ] done

## Make a way to fly with "local gps world file"

- user will select a gps_local_world.yaml
- the world_origin will be initialized at the current spot
- the safety are will be in fixed_origin
- [ ] done

## System for "doing stuff" before landing
- e.g., deploying landing gear
- [ ] done

# run build on push only for ctu-mrs

- check the github runner instance for ctu-mrs
- [ ] todo

## run test on push to ros2

- [ ] todo 
# clean up header installation and paths in mrs_multirotor_simulator

- [x] todo

# fix cmakelists and node export
- `rclcpp_components_register_nodes(MrsUavAutostart_AutomaticStart "mrs_uav_autostart::automatic_start::AutomaticStart")`

## Controller's common handlers should offer IMU subscribe handler

- [ ] done

## Service client handler
- should expose api for checking if the other side is ready

## Docker image tagging
- [x] tag images also with year_week tag to backup older versions

## Estimation manager should use this to wait for time

- https://docs.ros.org/en/jazzy/p/rclcpp/generated/classrclcpp_1_1Clock.html#_CPPv4N6rclcpp5Clock18wait_until_startedEN7Context9SharedPtrE
