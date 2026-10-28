# 🦾 Vision-Guided Robotic Pick-and-Place Using ROS 2 and OpenCV

A **ROS 2-based intelligent color sorting system** powered by the **Franka Emika Panda robotic arm**. This project integrates **OpenCV computer vision**, **MoveIt 2 motion planning**, and **Gazebo simulation** to create an autonomous pick-and-place system that detects and sorts colored objects with precision.

## 🌟 Features

* 👁️ **Color Detection**: Real-time OpenCV-based vision system for Red, Green, and Blue objects.

* 🧭 **Motion Planning**: Advanced trajectory planning using MoveIt 2.

* ⚡ **Autonomous Operation**: Complete pick-and-place automation with gripper control.

* 🔀 **Dynamic Target Selection**: Switch between colors without restarting the system.

* 🖥️ **Visual Feedback**: Integrated RViz visualization for motion planning and execution.

* 📦 **Dev Container Ready**: Pre-configured VS Code Dev Container environment with GUI and GPU acceleration support for instant deployment.

## 📚 What You'll Learn

* Setting up a complete ROS 2 robotic system from scratch.

* Integrating computer vision with robotic manipulation.

* Motion planning and execution with MoveIt 2.

* Using PyMoveIt2 for high-level Python control.

* Building modular ROS 2 packages.

* Dev Container containerization for robotics applications.

## 🗂️ Table of Contents

1. [⚡ Quick Start with VS Code Dev Containers (Recommended)](#-quick-start-with-vs-code-dev-containers-recommended)

2. [🖥️ Manual Installation on Local PC](#-manual-installation-on-local-pc)

3. [▶️ Running the Project](#-running-the-project)

4. [📁 Project Structure](#-project-structure)

5. [🛠️ Troubleshooting](#️-troubleshooting)

# ⚡ Quick Start with VS Code Dev Containers (Recommended)

Get up and running in minutes with our pre-built Dev Container! This method eliminates dependency conflicts, provides a consistent environment, and automatically mounts your code so you can edit locally while running in Docker.

## Prerequisites

* **Docker** installed on your system.

* **Visual Studio Code** installed.

* **Dev Containers extension** installed in VS Code.

* **X11 display server** (for GUI applications like RViz and Gazebo).

## ⚙️ Step 1: Clone and Open

1. Clone the repository to your local machine:

   ```
   git clone https://github.com/Programiz-007/Vision-Guided-Robotic-Pick-and-Place-Using-ROS-2-and-OpenCV.git
   cd Vision-Guided-Robotic-Pick-and-Place-Using-ROS-2-and-OpenCV
   ```

2. Open the folder in VS Code:

   ```
   code .
   ```

## 🚢 Step 2: Launch the Dev Container

1. Allow Docker to access your local X11 display (run this in your host terminal):

   ```
   xhost +local:root
   ```

2. In VS Code, press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac).

3. Type and select **Dev Containers: Reopen in Container**.

4. VS Code will build the ROS 2 Humble image, install all MoveIt and OpenCV dependencies, and mount your workspace. (This may take a few minutes the first time).

## 🔨 Step 3: Build the Workspace

Once the container is running, open the integrated terminal in VS Code (`Ctrl+` \`) and build the project:

```
cd /root/panda_ws
colcon build --symlink-install
source install/setup.bash
```

🎊 **You're now ready to run the project!** Skip to the [Running the Project](#-running-the-project) section.

# 🖥️ Manual Installation on Local PC

If you prefer not to use Docker, follow this setup guide for a local Ubuntu 22.04 installation.

## 📥 Step 1: Install ROS 2 Humble & Dependencies

1. Follow the official guide to [install ROS 2 Humble Desktop](https://docs.ros.org/en/humble/Installation.html).

2. Install build tools and MoveIt 2:

   ```
   sudo apt update
   sudo apt install -y python3-rosdep python3-colcon-common-extensions python3-pip \
     ros-humble-moveit ros-humble-moveit-common ros-humble-moveit-ros-move-group \
     ros-humble-moveit-ros-planning ros-humble-moveit-ros-planning-interface \
     ros-humble-moveit-visual-tools ros-humble-moveit-configs-utils \
     ros-humble-moveit-setup-assistant ros-humble-ros2-control ros-humble-ros2-controllers \
     ros-humble-gazebo-ros2-control ros-humble-ros-gz ros-humble-cv-bridge \
     ros-humble-image-transport ros-humble-rqt-image-view
   ```

3. Install Python requirements:

   ```
   pip3 install --no-cache-dir opencv-python==4.10.0.84 numpy==1.24.4 transforms3d
   ```

## 🔨 Step 2: Build Workspace

```
mkdir -p ~/panda_ws/src
cd ~/panda_ws/src
git clone https://github.com/Programiz-007/Vision-Guided-Robotic-Pick-and-Place-Using-ROS-2-and-OpenCV.git .
cd ~/panda_ws
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --skip-keys=opencv_python -r -y
colcon build --symlink-install
source install/setup.bash
```

# ▶️ Running the Project

Open **two terminal windows** (if using Dev Containers, you can split the VS Code terminal).

### Terminal 1: Launch the System

```
source ~/panda_ws/install/setup.bash
ros2 launch panda_bringup pick_and_place.launch.py
```

*Wait for Gazebo, RViz, and MoveIt to fully initialize.*

### Terminal 2: Start Pick-and-Place Node

```
source ~/panda_ws/install/setup.bash
ros2 run pymoveit2 pick_and_place.py --ros-args -p target_color:=R
```

**Change colors by stopping** (`Ctrl+C`) **and rerunning with a different parameter:**

* For Green objects: `ros2 run pymoveit2 pick_and_place.py --ros-args -p target_color:=G`

* For Blue objects: `ros2 run pymoveit2 pick_and_place.py --ros-args -p target_color:=B`




# 📁 Project Structure

```text
src/
├── .devcontainer/               # VS Code Docker environment config
├── panda_description/           # Robot URDF, meshes, visuals
├── panda_controller/            # Joint/gripper controllers
├── panda_moveit/                # MoveIt 2 configuration
├── panda_vision/                # OpenCV color detection
├── panda_bringup/               # Launch files for full system
└── pymoveit2/                   # Python MoveIt 2 interface
    └── examples/
        └── pick_and_place.py    # Main logic
```


# 🧩 Package Overview

| Package | Description | Key Files |
|---------|-------------|-----------|
| **panda_description** | Robot model and visualization | `panda.urdf.xacro`, `meshes` |
| **panda_controller** | Controller configurations | `panda_controllers.yaml` |
| **panda_moveit** | Motion planning setup | MoveIt configs, `SRDF` |
| **panda_vision** | Computer vision system | `color_detector.py` |
| **panda_bringup** | System launcher | `pick_and_place.launch.py` |
| **pymoveit2** | High-level control | `pick_and_place.py` |

# 🚀 How It Works

## System Architecture

```text
Camera Feed → Color Detection → Object Localization
                                      ↓
                               Motion Planning (MoveIt 2)
                                      ↓
                             Trajectory Execution
                                      ↓
                    Pick Object → Move to Bin → Place
```

## Detailed Workflow

1. **Vision System** continuously monitors the workspace.
2. **Color detector** identifies target color and computes 3D position.
3. **PyMoveIt2** receives object coordinates.
4. **MoveIt 2** plans collision-free trajectory.
5. **Robot controller** executes motion.
6. **Gripper** closes to grasp object.
7. **Motion planner** computes path to sorting bin.
8. **Robot** moves to designated area.
9. **Gripper** opens to release object.
10. **System** returns to home position and repeats.

# 🛠️ Troubleshooting

### ❌ RViz/Gazebo Display Issues (Docker)

**Error:** `Cannot connect to display` or RViz shows a black screen.
**Solution:** Ensure you ran `xhost +local:root` on your host machine before launching the Dev Container.

### ❌ MoveIt Planning Failures

**Error:** `Unable to plan` or `No valid plan found`
**Solutions:**

1. Check if all controllers are active: `ros2 control list_controllers`

2. Ensure the Gazebo physics engine is not paused.

### ❌ Camera Not Publishing / Color Detection Fails

**Solutions:**

1. Check if the Gazebo camera plugin loaded: `ros2 topic list | grep camera`

2. Test the raw feed: `ros2 run rqt_image_view rqt_image_view`

3. If lighting in Gazebo is too dark/bright, adjust HSV thresholds in `panda_vision/color_detector.py`.

# 📜 License

This project is licensed under the **MIT License**.

**Third-party code:** [`pymoveit2`](https://github.com/AndrejOrsula/pymoveit2) by Andrej Orsula is vendored in `pymoveit2/` with its own license.
