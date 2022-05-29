# Robothon 2022 Grand-Challenge CARI Team Report
<p align="center">
Authors: Samuele Sandrini, Cesare Tonola, 
Michele Delledonne, Roberto Pagani
</p>

## Hardware Setup
The robot platform on which the Robothon Grand Challange was tested is shown in the figure below.
<p align="center">
  <img height="600" src="https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_report/blob/master/images/Robothon_Setup_Without_Electromagnet.png">
</p>

The setup consists of:
- a UR3 six-degree-of-freedom collaborative robot mounted on a fixed table. The robot does not have a force sensor but it estimates torques and forces through the current sensor on the joints. 
- a two-finger gripper, the hand-e robotiq model, mounted on the last joint of the robot. It was chosen not to add additional fixtures to the gripper but to exploit it in its general-purpose mode.
- a vision system consisting of an rgbd camera (depth not used for this purpose) and lighting system. Specifically, the camera is a RealSense D435, which was mounted on an articulated mechanical structure maintained statically at the ceiling. Instead, the lighting system was recovered from household applications, in the spirit of competition reuse.

## Software Solution
Our software framework is developed using ROS (Robot Operating System). The overall framework is summarized in a figure below. At the beginning, the vision system localizes the board and its feature points and advertises the reference frames (Section 2.1). Then, the sequence of tasks is executed with respect to such frames. The execution of each task combines motion planning and different kind of controllers (Sections 2.2 and Section 2.3).

### Vision System (Board Localization)
First of all, a Hand-Eye Calibration was performed in the eye-to-hand version (camera maintained static respect robot), in order to know the reference system of the camera with respect to the robot reference system (extrinsic calibration). 

In addition, an intrinsic calibration was performed, in order to switch between the image reference system to the camera reference system.

These two calibrations were performed once offline, and the roto-translation matrices and others parameters were saved.

As for the online localization, the vision system is used to identify the position of the board relative to the robot base. Specifically, an rgb frame is acquired as the first task, then features such as the center of the red button, key lock, and screen are identified. The features are recognized by applying border detection, color clustering, canny detection, Hough transform and custom designed vision algorithms. Once the features position is detected in the image frame, we move on to the camera reference system and finally that of the robot base, thanks to the instrinsic and extrinsic parameters estimated in the offline part. The procedure is illustrated in the following image:
<p align="center">
  <img height="600" src="https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_report/blob/master/images/Vision_System.png">
</p>

Regarding the software implementation, this module was developed in Python language, and two open-source libraries for image processing (OpenCv and Skimage) were used.


### Task execution management
The automatic execution of the tasks is managed by a Behavior Tree Executor. Each Robothon’s task corresponds to a SubTree. BTs are a very efficient way of creating complex systems that are both modular and flexible. This makes it easy to manage the ability to change the order in which tasks are executed, thus creating a flexible application.
<p align="center">
  <img height="600" src="https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_report/blob/master/images/bt.png">
</p>

### Planning and execution
Each task is managed by the Action Planner "Manipulation", which implements pre-defined skills such as pick, place and move. A skill takes as input the goal Cartesian location, plans the sequence of motion to carry out the task and execute them. 
In Robothon we only use skills of type "move to", based on the following pipeline:
- Plan and execute motion to desired Cartesian pose
- Execute custom jobs

### Controllers
The different phases of a task require different controllers. In our framework, controllers are implemented in ROS control and can be easily switched at runtime. In particular, during a "move to" skill, we use a joint-space position controller for trajectory tracking during the approach phase.

Other controllers, such as a Cartesian-space position controller and a Cartesian-space impedance controller, are available to implement the custom jobs.

### Custom jobs
Custom jobs could be implemented using the available controllers, but for this year, to simplify the implementation of the tasks and improve the transferability of the tasks manager, we have chosen to implement each single job using the UR teach pendant program editor. So, the Action Planner takes care of planning (es. using an OMPL planner) and executing the motion to the Cartesian goal, and then it loads the corresponding program to carry out the job. In this way, each job can easily be programmed, even by non-expert operators.

## Repository of software modules
The software implemented is based on:

Third parties software:
- [ROS](https://www.ros.org/)
- [Universal robot ros driver](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver)
- [Intel realsense ros driver](https://github.com/IntelRealSense/realsense-ros)

Our research group software needed:
- [Manipulation framework](https://github.com/JRL-CARI-CNR-UNIBS/manipulation)
- [Controllers framework](https://github.com/CNR-STIIMA-IRAS/cnr_ros_control)
- [Trajectory controller](https://github.com/CNR-STIIMA-IRAS/cnr_motion_control)

Software developed specifically for Robothon:
- [Vision System](https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_vision)
- [Task Execution management](https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_tree)
- [Cell Configuration (Geometric configuration and controllers)](https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_cell)

If you want run the entire code, follow this [guide](https://github.com/JRL-CARI-CNR-UNIBS/installation).

## How to run
First, load the robotic cell:

``
roslaunch robothon2022_configurations real_start.launch
``

Locate the board with the vision system:

``
rosservice call /robothon2022/boardlocalization
``

Compute ik corresponding to each task location:

``
roslaunch robothon2022_tree load_locations.launch
``

Finally, run the behaviour tree to execute the tasks:

``
roslaunch robothon2022_tree run_tree.launch
``

## Authors

- Samuele Sandrini, [SamueleSandrini](https://github.com/SamueleSandrini)
- Cesare Tonola, [CesareTonola](https://github.com/CesareTonola)
- Michele Delledonne, [Michele Delledonne](https://github.com/MichiDelle)
- Roberto Pagani, [Roberto Pagani](https://github.com/Roby-Pagani)
