# Printing contents of a message

What was done in ROS1 as

```cpp
std::cout << "Message: " << my_message << std::endl;
```

is not longer possible. In ROS2, you have to call a static method from the respective message package, that will convert the message to yaml, json, etc.

```cpp
std::cout << "Message: " << geometry_msgs::msg::to_yaml(my_message) << std::endl;
```

# Compilation profiles in a workspace

TODO
should be solved bycolcon mixin