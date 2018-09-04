# Haptic Teleoperation in RAIN hub project
---
## Overview
This is a repository of visual/haptic-teleoperated industrial robotic manipulators in RAIN hub project (https://rainhub.org.uk/). 
At the moment, this source file is a gazebo model for UR5 and Robotiq 3-finger gripper. 

** Auther/Maintainer: Inmo Jang (inmo.jang@manchester.ac.uk), Robotics for Extreme Environment Group, University of Manchester**

---
## Installation

### Building from Source

#### Dependencies
The dependencies required are as follows:

(1) Gazebo model for Robotiq S three-finger gripper (https://bitbucket.org/wilsonz91/robotiq/src/master/)

(2) Universal robot package (https://github.com/ros-industrial/universal_robot)
  
  - If you clone this package, it is required to have additional files to control the UR gazebo model. Thus, I would recommend to clone this (https://inmojang@bitbucket.org/inmojang/universal_robot.git) 


#### Buliding
To build from source, clone the latest version from this repository into your catkin workspace and compile the package.
    
	        cd catkin_ws/src
	        git clone https://wilsonz91@bitbucket.org/wilsonz91/robotiq.git
            git clone https://inmojang@bitbucket.org/inmojang/universal_robot.git
	        git clone https://inmojang@bitbucket.org/inmojang/rain.git
            catkin_make

### Packages Overview

This repository consists of following packages:

* ***rain*** the meta-package for the teleoperation system meta package
* ***rain_description*** contains the urdf description of the integrated robot (UR5 + the gripper).
* ***rain_gazebo*** contains the launch file for the gazebo simulator.


---
## Usage

### Control the gripper
Launch the Gazebo simulator:

        roslaunch rain_gazebo ur5_robotiq.launch

To control the gripper, in another terminal, execute below:-

Closing the hand halfway:

        rostopic pub --once right_hand/command robotiq_msgs/SModelRobotOutput {1,0,1,0,0,0,127,255,0,155,0,0,255,0,0,0,0,0}

Fully open the hand:

        rostopic pub --once right_hand/command robotiq_msgs/SModelRobotOutput {1,0,1,0,0,0,0,255,0,155,0,0,255,0,0,0,0,0}

Change the grasping mode to pinch and close the gripper:

        rostopic pub --once right_hand/command robotiq_msgs/SModelRobotOutput {1,1,1,0,0,0,255,255,0,155,0,0,255,0,0,0,0,0}

Switch to wide mode and fully open the hand:

        rostopic pub --once right_hand/command robotiq_msgs/SModelRobotOutput {1,2,1,0,0,0,0,255,0,155,0,0,255,0,0,0,0,0}

Change to scissor mode and close the fingers:

        rostopic pub --once right_hand/command robotiq_msgs/SModelRobotOutput {1,3,1,0,0,0,255,255,0,155,0,0,255,0,0,0,0,0}

Open fingers:

        rostopic pub --once right_hand/command robotiq_msgs/SModelRobotOutput {1,3,1,0,0,0,0,255,0,155,0,0,255,0,0,0,0,0}


### Control the arm

Execute below in the command line: 

        rosrun ur_driver gazebo_move.py

Note that, in the python file, there are predefined trajectories. 