# Robothon 2022 Grand-Challenge CARI Team Report
<p align="center">
Authors: Samuele Sandrini, Cesare Tonola, 
Michele Delledonne, Roberto Pagani
</p>

## Hardware Setup
The robot platform on which the challenge defined by robothon grand challange was tested is shown in the figure below.
<p align="center">
  <img height="600" src="https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_report/blob/master/images/Robothon_Setup_Without_Electromagnet.png">
</p>

The setup consists of:
- a UR3 six-degree-of-freedom collaborative robot mounted on a fixed table. The robot does not have a force sensor but it estimates it through the current sensor on the joints. 
- a two-finger gripper, the hand-e robotiq modello, mounted on the last joint of the robot. It was chosen not to add additional fixtures to the gripper but to exploit it in its general-purpose mode.
- a vision system consisting of an rgbd camera (depth not used for this purpose) and lighting system. Specifically, the camera is a RealSense d435, which was mounted on an articulated mechanical structure maintained statically at the ceiling. Instead, the lighting system was recovered from household applications, in the spirit of competition reuse.

## Software Solution
Our software framework is developed using ROS (Robot Operating System). The overall framework is summarized in figure below. At the beginning, the vision system localizes the board and its feature points and advertises the reference frames (Section 2.1). Then, the sequence of tasks is executed with respect to such frames. The execution of each task combines motion planning and different kind of controllers (Sections 2.2 and Section 2.3).

### Vision System (Board Localization)
First of all, a Hand-Eye Calibration was performed in the eye-to-hand version (camera maintained static respect robot), in order to know the reference system of the camera with respect to the robot reference system (extrinsic calibration). 

In addition, an intrinsic calibration was performed, in order to switch between the image reference system to the camera reference system.

These two calibrations were performed once offline, and the roto-translation matrices and others parameters were saved.
As for the online part related to the vision system, it is used to identify the position of the board relative to the robot base.





### Task execution management

### Planning and execution


### Controllers

The different phases of a task require different controllers. In our framework, controllers are implemented in ROS control and can be easily switched at runtime. In particular, during a "move to" skill, we use:
- a joint-space position controller
- a Cartesian-space position controller
- a Cartesian-space impedance controller

The joint-space position controller is used for trajectory tracking during the approach phase; the Cartesian-space position controller is used for small, relative positioning movements with respect to a Cartesian pose. 
The Cartesian-space impedance controller is used for interaction tasks such as insertions.

## Description of tasks

## Repository of software modules
- [Vision System](https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_vision)
- [Task Execution management](https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_tree)
- [Cell Configuration (Geometric configuration and controllers)](https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_cell)

## Dependencies 

- [Manipulation framework](https://github.com/JRL-CARI-CNR-UNIBS/manipulation)

## Authors

- Samuele Sandrini, [SamueleSandrini](https://github.com/SamueleSandrini)
- Cesare Tonola, [CesareTonola](https://github.com/CesareTonola)
- Michele Delledonne, [Michele Delledonne](https://github.com/MichiDelle)
- Roberto Pagani, [Roberto Pagani](https://github.com/Roby-Pagani)