Mobile Manipulator: Franka FR3 + AgileX Ranger Mini
A comprehensive ROS 2 URDF integration and simulation package for a composite mobile manipulator, featuring the Franka Emika FR3 robotic arm mounted on an AgileX Ranger Mini V2 omnidirectional mobile base.

This project was developed to solve common integration challenges such as inertia parameter loading, multi-hardware transmission conflicts, and correct physical mounting alignment in Gazebo simulations.

(![6f3591924e05bdeeb0e6ab4e0d67cba7](https://github.com/user-attachments/assets/b7a764ea-591e-4bab-aca3-7b24293f4b81)
![0257d4e60ed0f091ece1c59fadb00e58](https://github.com/user-attachments/assets/6f1487f7-835e-4e68-b222-c4aecf01b3b9)



🚀 Key Features
Composite URDF/Xacro: Seamlessly merges the ranger_mini_v2 base and franka_fr3 arm descriptions into a single robot description.

Physics-Ready Simulation:

Corrected mounting height to prevent chassis collision or floating.

Fixed Hand Inertia: Solved the argument of type 'bool' is not iterable error by properly loading inertia parameters from YAML using xacro.load_yaml.

Functional Gripper: Injected ros2_control interfaces for the Franka Hand fingers, enabling visualization in Rviz and control in Gazebo (which is missing in standard descriptions).

Omnidirectional Control: Supports holonomic movement (mecanum/crab steering) simulation.

Modular Design: Easy to extend for MoveIt 2 and Nav2 (Planned).

📦 Prerequisites
This package is designed for ROS 2 Humble Hawksbill on Ubuntu 22.04.

Dependencies
Ensure you have the standard ROS 2 simulation and control packages installed:

Bash

sudo apt update
sudo apt install ros-humble-gazebo-ros-pkgs \
                 ros-humble-ros2-control \
                 ros-humble-gazebo-ros2-control \
                 ros-humble-joint-state-publisher-gui \
                 ros-humble-xacro \
                 ros-humble-teleop-twist-keyboard
🛠️ Installation
Create a Workspace (if you haven't already):

Bash

mkdir -p ~/mobile_manipulation_ws/src
cd ~/mobile_manipulation_ws/src
Clone the Repository:

Bash

git clone https://github.com/fjc6666/mobile_manipulation_project.git .
Install Dependencies:

Bash

cd ~/mobile_manipulation_ws
rosdep install --from-paths src --ignore-src -r -y
Build the Package:

Bash

colcon build --symlink-install
source install/setup.bash
💻 Usage
1. Launch Simulation (Gazebo + Rviz)
This launch file loads the robot into an empty Gazebo world and opens Rviz2 for state visualization.

Bash

ros2 launch composite_robot_description display_sim.launch.py verbose:=true
Note: If Gazebo fails to launch or hangs, try disabling the model database download:

Bash

export GAZEBO_MODEL_DATABASE_URI=""
2. Teleoperation (Drive the Base)
To control the Ranger Mini base using your keyboard:

Bash

ros2 run teleop_twist_keyboard teleop_twist_keyboard
Use i, j, k, l, , to move.

The Ranger Mini supports omnidirectional movement (holonomic).

🔧 Technical Details & Fixes
1. Inertia Loading Fix
Standard Franka descriptions often fail to load inertials.yaml when used as a sub-macro. This package implements a robust fix in mobile_manipulator.urdf.xacro:

XML

<xacro:property name="hand_inertials" value="${xacro.load_yaml('$(find franka_description)/end_effectors/franka_hand/inertials.yaml')}"/>
<xacro:franka_hand ... ee_inertials="${hand_inertials}"/>
2. Gripper Visualization
By default, the Franka hand lacks ros2_control tags in simulation-only setups, causing the fingers to disappear in Rviz. We manually injected a FrankaHandFakeSystem interface to ensure joint states are published correctly.

🗺️ Roadmap
[x] URDF Integration & Physical Simulation

[x] Base Control (cmd_vel)

[ ] MoveIt 2 Integration: Motion planning for the arm (In Progress).

[ ] Nav2 Integration: Autonomous navigation for the base.

[ ] VR Teleoperation: Interface for remote control using VR headsets (Graduation Project Goal).

📝 License
This project is licensed under the Apache 2.0 License.

Author: fjc6666 Project: Mobile Manipulation System Design (Graduation Project)
And thank you to CharithDombawala for providing the description and configuration section for ranger_miniV2.
