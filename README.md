# DTU_Robotics
Repository for the project work of GROUP 17 regarding the course 34753 -  Robotics at the Technical University of Denmark

Overview

This project implements an automatic ring detection and insertion system for a robotic manipulator with 4 revolute joints (4-DOF).
The robot uses a camera mounted on the end effector to analyze the environment, locate a ring, approach it, and insert the end effector through it.

The system includes:
- Camera calibration
- Circle/ring detection
- Forward and inverse kinematics
- Robot motion control
- Motor communication
A complete execution pipeline managed by main.py

project/
│
├── calibration.py    # Camera calibration & visualization of calibration results
├── kinematics.py     # Forward and inverse kinematics functions
├── control.py        # Ring detection, robot motions, motor communication
└── main.py           # Main entry point of the system


1. Camera Calibration (calibration.py)
This module:
 - Loads a set of images from the BATSA/ directory
 - Performs camera calibration (e.g., chessboard detection)
 - Computes intrinsic and extrinsic camera parameters
 - Visualizes reprojection errors and calibration results
 - Saves calibration data for use in ring detection


2. Robot Kinematics (kinematics.py)
Includes all necessary kinematic functions:
 - Forward Kinematics (FK):
     Computes the end-effector position and orientation based on the four joint angles.
 - Inverse Kinematics (IK):
     Computes the joint angles required to reach a given target position.
     Used extensively by control.py for robot motion planning.


3. Ring Detection & Robot Control (control.py)
This module contains:

    a. Motion Functions

       - go_home() function to move the robot to a safe starting configuration
       - Circular and incremental exploratory movements for expanding the camera’s field of view
       - Trajectory execution via IK for approaching the detected ring

    b. Ring Detection

       - Captures images from the onboard camera
       - Detects circular shapes using Hough Transform or similar methods
       - Performs filtering and validation to ensure stable detection
       - Moves the end-effector in a circular search pattern if no ring is found

    c. Motor Communication

       - Sends commands to motor drivers (serial, CAN, or other protocols)
       - Receives position or status feedback
       - Handles low-level motion execution

4. Execution Pipeline (main.py)
This is the main entry point of the project.
A typical run follows this sequence:

    1.Initialization

       - Initialize camera communication
       - Initialize motor communication
       - Move robot to the home position

    2.Ring Search

       - Camera captures images in front of the robot
       - If the ring is not detected, the robot performs controlled circular scanning movements → gradually expanding the visible area

    3.Ring Detection

       - Once a circle is detected:
       - The ring’s 3D position is computed using camera calibration data
       - A target pose is computed through inverse kinematics

    4.Ring Insertion

       - The end effector is driven through the ring using smooth IK-based motion
