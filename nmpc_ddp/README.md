# nmpc_ddp
Non-linear model predictive control (NMPC) with differential dynamic drogramming (DDP)

![TestDDPCartPole](doc/images/TestDDPCartPole.gif)

## Features
- C++ header-only library
- Treats state and control input dimensions as template parameters
- Supports time-varying control input dimensions

## Install

### Requirements
- Compiler supporting C++17
- Tested with Ubuntu 18.04 / ROS Melodic

### Installation procedure
It is assumed that ROS is installed.

1. Setup catkin workspace.
```bash
$ mkdir -p ~/ros/ws_nmpc/src
$ cd ~/ros/ws_nmpc
$ wstool init src
$ wstool set -t src isri-aist/NMPC git@github.com:isri-aist/NMPC.git --git -y
$ wstool update -t src
```

2. Install dependent packages.
```bash
$ source /opt/ros/${ROS_DISTRO}/setup.bash
$ rosdep install -y -r --from-paths src --ignore-src
```

3. Build a package.
```bash
$ catkin build -DCMAKE_BUILD_TYPE=RelWithDebInfo --catkin-make-args all tests
# For best performance
$ catkin build -DOPTIMIZE_FOR_NATIVE=ON -DCMAKE_BUILD_TYPE=Release --catkin-make-args all tests
```

## Examples
Make sure that it is built with `--catkin-make-args tests` option.

### [Bipedal dynamics](tests/src/TestDDPBipedal.cpp)
Controlling on CoM-ZMP dynamics with time-variant CoM height.
System is linear and time-variant.
```bash
$ rosrun nmpc_ddp TestDDPBipedal
$ rosrun nmpc_ddp plotTestDDPBipedal.py
```
![TestDDPBipedal](doc/images/TestDDPBipedal.png)

### [Vertical motion](tests/src/TestDDPVerticalMotion.cpp)
Controlling vertical motion with time-variant number of contacts (including floating phase).
System is linear and time-variant.
The dimension of the control input changes (there are even time steps with an empty control input).
```bash
$ rosrun nmpc_ddp TestDDPVerticalMotion
$ rosrun nmpc_ddp plotTestDDPVerticalMotion.py
```
![TestDDPVerticalMotion](doc/images/TestDDPVerticalMotion.png)

### [Cart-pole](tests/src/TestDDPCartPole.cpp)
Controlling cart-pole (also known as inverted pendulum).
System is non-linear.
This is an example of a realistic setup where the control and simulation have different time periods.
```bash
# 10-second simulation
$ rostest nmpc_ddp TestDDPCartPole.test --text
# Endless simulation
$ rostest nmpc_ddp TestDDPCartPole.test no_exit:=true --text
```
![TestDDPCartPole](doc/images/TestDDPCartPole.gif)
