# ベースイメージ
FROM ros:humble as dev

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /root/agilex_ws/

# 必要なキーの取得とパッケージインストール
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
        udev \
        apt-transport-https \
        ca-certificates \
        vim \
        swig \
        software-properties-common \
        python3-pip \
        python3-colcon-common-extensions \
        # ROS2 基本ツール
        ros-humble-joint-state-publisher-gui \
        ros-humble-rqt-robot-steering \
        ros-humble-rqt-graph \
        ros-humble-teleop-twist-keyboard \
        ros-humble-cartographer-ros \
        ros-humble-navigation2 \
        ros-humble-nav2-bringup \
        ros-humble-rviz2 \
        ros-humble-turtlesim \
        ros-humble-examples-rclpy-minimal-publisher \
        ros-humble-examples-rclpy-minimal-subscriber \
        # ROS2 カメラ関連パッケージ
        ros-humble-camera-info-manager \
        ros-humble-image-geometry \
        ros-humble-image-publisher \
        ros-humble-cv-bridge \
        ros-humble-image-transport \
        # 追加依存パッケージ
        libgflags-dev \
        libgoogle-glog-dev \
        libeigen3-dev \
    && rm -rf /var/lib/apt/lists/*

# libuvc のビルドとインストール
RUN mkdir -p /root/agilex_ws/src && \
    cd /root/agilex_ws/src && \
    git clone https://github.com/libuvc/libuvc.git && \
    cd libuvc && mkdir build && cd build && \
    cmake .. && make -j$(nproc) && make install && ldconfig && \
    cd /root/agilex_ws/src && rm -rf libuvc

# YDLidar-SDK のビルドと pip インストール
RUN cd /root/agilex_ws/src && \
    git clone https://github.com/YDLIDAR/YDLidar-SDK.git && \
    mkdir -p YDLidar-SDK/build && cd YDLidar-SDK/build && \
    cmake .. && make -j$(nproc) && make install && \
    cd .. && pip3 install . && cd /root/agilex_ws/src && rm -rf YDLidar-SDK

# ROS2 ワークスペースの作成、リポジトリ取得、ビルド
RUN cd /root/agilex_ws/src && \
    git clone -b humble-dev https://github.com/TechShare-inc/limo_ros2.git && \
    git clone -b master https://github.com/orbbec/ros2_astra_camera.git && \
    cd /root/agilex_ws && \
    . /opt/ros/humble/setup.bash && \
    colcon build --symlink-install

# bash ログイン時に ROS とワークスペースをソースする
RUN echo "source /opt/ros/humble/setup.bash" >> /etc/bash.bashrc && \
    echo "source /root/agilex_ws/install/setup.bash" >> /etc/bash.bashrc
