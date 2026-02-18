# Robotics Simulation Challenge - Autonomous Navigation with ROS 2

## Project Overview
This project demonstrates autonomous robot navigation using ROS 2, Gazebo, and Nav2. A TurtleBot3 robot navigates from a start position to a goal while avoiding obstacles in a simulated environment.
## Demonstration Video
<video src="Robot_Navigation_Challenge/assets/Demonstration video.mp4" controls width="600">
  Your browser does not support the video tag.
</video>

### Features
- ROS 2 Jazzy (Ubuntu 24.04) setup
- Gazebo simulation with TurtleBot3
- Nav2-based autonomous navigation
- Obstacle avoidance using costmaps
- RViz visualization and teleoperation
- Complete documentation and reproducibility

## System Requirements

### Hardware
- Windows 10/11 with WSL2 enabled
- 8GB RAM minimum (16GB recommended)
- 20GB free disk space

### Software
- Windows Subsystem for Linux 2 (WSL2)
- Ubuntu 24.04 LTS (Noble)
- ROS 2 Jazzy
- Gazebo Harmonic

## Installation Guide

### Step 1: Enable WSL2 on Windows
1. Open PowerShell as Administrator
2. Run:
```powershell
wsl --install -d Ubuntu-24.04
wsl --set-default-version 2
```
### Step 2: Install ROS 2 Jazzy
Open Ubuntu terminal and run:
```
# Update system
sudo apt update && sudo apt upgrade -y

# Set locale
sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

# Add ROS 2 repository
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Install ROS 2 Jazzy Desktop
sudo apt update
sudo apt install ros-jazzy-desktop

# Source ROS 2
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
``` 
run each line at a time to avoid issues

### Step 3: Install TurtleBot3 Packages
```
# Install TurtleBot3 simulation packages
sudo apt install ros-jazzy-turtlebot3*

# Set default robot model
echo "export TURTLEBOT3_MODEL=waffle_pi" >> ~/.bashrc
source ~/.bashrc
```
### Running the Simulation
## Method 1: Manual Launch (Recommended for Development)
Terminal 1 - Launch Gazebo World:
```
export TURTLEBOT3_MODEL=waffle_pi
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
Terminal 2 - Launch Navigation Stack:
```
export TURTLEBOT3_MODEL=waffle_pi
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True
```
## Method 2: Using Launch Script
```
# Make script executable
chmod +x scripts/launch_navigation.sh

# Run the script
./scripts/launch_navigation.sh
```
### Using the Navigation System
## Critical Setup Steps
Localize the Robot (MUST DO FIRST):

In RViz, click the "2D Pose Estimate" button

Click on the map where the robot actually is in Gazebo

Drag to set the robot's current orientation

Send Navigation Goal:

Click the "Nav2 Goal" button in RViz

Click on an empty spot on the map (destination)

Drag to set the robot's final orientation at the goal

⚠️ Common Mistake: Do NOT click on the robot and drag away. Always click on an empty destination spot.(i spent 20 minutes reviewing the installation and debugging because of this mistake and i will gladly admit it)

### Navigation Demonstration
## Expected Behavior
Green global path appears from robot to goal

Red local path shows immediate trajectory

Robot moves smoothly, avoiding obstacles

"Distance remaining" in Navigator panel counts down to 0

Robot reaches exact goal position and orientation

Monitoring Navigation Status
Check the "Navigation2" panel in RViz:

Navigation: Shows "active" during movement

Distance remaining: Decreases as robot approaches goal

ETA: Estimated time to arrival

Recoveries: Number of recovery behaviors triggered

### Troubleshooting
## Issue: Robot turns but doesn't move
Solution: Likely incorrect goal tolerance settings. Run:
```
ros2 param set /controller_server goal_checker.yaw_goal_tolerance 0.2
ros2 param set /controller_server goal_checker.xy_goal_tolerance 0.3
```
## Issue: RViz crashes or freezes
Solution: WSL graphics limitations. Reduce Gazebo quality:
```
export GZ_GRAPHICS_ENGINE=ogre
export GZ_GRAPHICS_ANTIALIASING=0
```
## Issue: No map appears in RViz
Solution: Check if all nodes are running:
```
ros2 node list
# Should show: /amcl, /bt_navigator, /controller_server, etc.
```
### Video Demonstration
[Link to 2-3 minute screen recording showing:]

Simulation launching

Robot localization with 2D Pose Estimate

Navigation goal setting

Autonomous obstacle avoidance

Goal achievement

### Metrics (Optional Enhancement)
To add performance metrics:
```
# Time navigation performance
time ros2 action send_goal /navigate_to_pose ...
```

### Implementation Details
## What I Implemented
Complete ROS 2 environment setup from scratch

Navigation stack configuration and tuning

Documentation and reproducibility instructions

Troubleshooting guide for common issues

## What I Reused
TurtleBot3 robot model and Gazebo world (ROS package)

Nav2 navigation stack (ROS 2 package)

Standard ROS 2 launch files (modified for this project)

### Future Improvements
Custom World Design: Create complex obstacle courses

Performance Metrics: Track success rate, collision count, time-to-goal

Multiple Robot Types: Support different robot models

Web Interface: Simple web dashboard for navigation control

SLAM Integration: Simultaneous localization and mapping

### References
[ROS 2 Documentation
](https://docs.ros.org/)

[Nav2 Documentation
](https://navigation.ros.org/)

[TurtleBot3 Manual
](https://emanual.robotis.com/docs/en/platform/turtlebot3/overview/)

[Gazebo Tutorials
](http://gazebosim.org/tutorials)

### Author
[Mohamad Moallem] - Data Scientist
