FROM osrf/ros:humble-desktop-full

ENV ROS_WS=/root/panda_ws
ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /root

# Install base tools and MoveIt packages
RUN apt-get update && apt-get install -y \
    git \
    python3-rosdep \
    python3-colcon-common-extensions \
    python3-pip \
    ros-humble-moveit \
    ros-humble-moveit-common \
    ros-humble-moveit-ros-move-group \
    ros-humble-moveit-ros-planning \
    ros-humble-moveit-ros-planning-interface \
    ros-humble-moveit-visual-tools \
    ros-humble-moveit-configs-utils \
    ros-humble-moveit-setup-assistant \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
RUN pip3 install --no-cache-dir \
    opencv-python==4.10.0.84 \
    numpy==1.24.4 \
    transforms3d

# Initialize rosdep
RUN rosdep update --rosdistro=humble

# Source the workspace in bashrc
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc && \
    echo "if [ -f $ROS_WS/install/setup.bash ]; then source $ROS_WS/install/setup.bash; fi" >> ~/.bashrc
