FROM ros:humble as dev

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /root/ros2_ws/

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 42D5A192B819C5DA \
    && apt-get update \
    && apt-get install -qq -y --no-install-recommends \
    build-essential \
    cmake \
    git \
    libssl-dev \
    libusb-1.0-0-dev \
    pkg-config \
    libgtk-3-dev \
    libglfw3-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \    
    curl \	
    libusb-1.0-0 \
    udev \
    apt-transport-https \
    ca-certificates \
    curl \
    vim \
    swig \
    software-properties-common \
    python3-pip \
    ros-humble-joint-state-publisher-gui \
    ros-humble-rqt-robot-steering \
    ros-humble-teleop-twist-keyboard\
    ros-humble-cartographer-ros \
    ros-humble-navigation2 \
    ros-humble-nav2-bringup \
    ros-humble-rviz2 \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/YDLIDAR/YDLidar-SDK.git &&\
    mkdir -p YDLidar-SDK/build && \
    cd YDLidar-SDK/build &&\
    cmake ..&&\
    make &&\
    make install &&\
    cd .. &&\
    pip install . &&\
    cd .. && rm -r YDLidar-SDK 

RUN mkdir -p /root/ros2_ws/src/ && \
    . /opt/ros/humble/setup.sh && \
    cd /root/ros2_ws/src && \
    git clone -b foxy https://github.com/agilexrobotics/limo_ros2.git && \
    cd /root/ros2_ws && \
    colcon build && \
    bash /root/ros2_ws/install/setup.bash

RUN echo "source /opt/ros/humble/setup.bash" >> /etc/bash.bashrc && \
    echo "source /root/ros2_ws/install/setup.bash" >> /etc/bash.bashrc

