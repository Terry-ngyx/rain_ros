# Haptic Teleoperation in RAIN hub project
---
## Overview
This is a repository of visual/haptic-teleoperated industrial robotic manipulators in RAIN hub project (https://rainhub.org.uk/). 
At the moment, this source file is a gazebo model for UR5 and Robotiq 3-finger gripper. 

** Author/Maintainer: Inmo Jang (inmo.jang@manchester.ac.uk), Robotics for Extreme Environment Group, University of Manchester**

## Version Information
Version 1.1 uses effort_controllers for joint control of UR5. (Ver 1.0 uses position_controllers). 
This update enables the gazebo model to pick and grasp an object in the gazebo environment. In Ver 1.0, this was not the case. 
Instead, it becomes necessary to set PID gains, which were just set by trial and errors in this version. 

---
## Installation

### Environment setting

- Install ROS as in http://wiki.ros.org/kinetic/Installation/Ubuntu
- Install Gazebo ROS pakcage as in http://gazebosim.org/tutorials?tut=ros_installing (Instruction for ROS Kinetic)

### Building from Source

#### Dependencies
The dependencies required are as follows: 

(1) Universal robot package (https://bitbucket.org/rain_epsrc/universal_robot/)


(2) Robotiq 3-finger gripper package (https://bitbucket.org/rain_epsrc/robotiq)


Note: Some of the dependencies are originally from other repositories (please refer to each repo's git), but modified for this project. 




### Packages Overview

This repository consists of following packages:

* ***rain*** the meta-package for the teleoperation system meta package
* ***rain_description*** contains the urdf description of the integrated robot (UR5 + the gripper).
* ***rain_gazebo*** contains the launch file for the gazebo simulator.


---
## Usage: Use the Gazebo Model Only

Launch the Gazebo simulator:

        roslaunch rain_gazebo ur5_robotiq.launch


#### Control the arm

##### Via LEAP Motion

Execute below to see the sensed information in RViz:

        roslaunch leap_motion demo.launch
        
Then, execute

        rosrun ur_driver gazebo_teleop_leap.py
        
        

##### Via Keyboard Teleoperation

Execute below in the command line to control each joint using a keyboard: 

        rosrun ur_driver gazebo_teleop_key.py


Or you may control the position/orientation of the end effector by:

        rosrun ur_driver gazebo_teleop_key_xyz.py
        



#### Control the gripper

##### Via Keyboard Teleoperation

To control the gripper, in another terminal, excute below in the command line:
        
        rosrun robotiq_s_model_control SModelController_gazebo.py

The gripper has Basic/Pinch/Wide/Scissor modes, and the fingers can also be individually controlled. 


##### Via ***rostopic pub*** 

Closing the hand halfway:

        rostopic pub --once right_hand/command robotiq_s_model_control/SModel_robot_output {1,0,1,0,0,0,127,255,0,155,0,0,255,0,0,0,0,0,0}

Fully open the hand:

        rostopic pub --once right_hand/command robotiq_s_model_control/SModel_robot_output {1,0,1,0,0,0,0,255,0,155,0,0,255,0,0,0,0,0,0}

Change the grasping mode to pinch and close the gripper:

        rostopic pub --once right_hand/command robotiq_s_model_control/SModel_robot_output {1,1,1,0,0,0,255,255,0,155,0,0,255,0,0,0,0,0,0}

Switch to wide mode and fully open the hand:

        rostopic pub --once right_hand/command robotiq_s_model_control/SModel_robot_output {1,2,1,0,0,0,0,255,0,155,0,0,255,0,0,0,0,0,0}

Change to scissor mode and close the fingers:

        rostopic pub --once right_hand/command robotiq_s_model_control/SModel_robot_output {1,3,1,0,0,0,255,255,0,155,0,0,255,0,0,0,0,0,0}

Open fingers:

        rostopic pub --once right_hand/command robotiq_s_model_control/SModel_robot_output {1,3,1,0,0,0,0,255,0,155,0,0,255,0,0,0,0,0,0}


![picture](rain/UR5_robotiq.png)

## Usage: Use the Gazebo Model while intefacing with Unity via Rosbridge

In the ROS side, launch the Gazebo simulator:

        roslaunch rain_unity ur5_robotiq_unity.launch

In the Unity side, run the scene with rosbridge. 


  