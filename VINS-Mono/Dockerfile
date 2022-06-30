FROM ros:kinetic-perception

ENV CERES_VERSION="1.12.0"
ENV CATKIN_WS=/root/catkin_ws
SHELL ["/bin/bash", "-c"] 

# set up thread number for building if you only want to use half the machine
#RUN   USE_PROC=$(( ($(nproc)+(2-1))/2 )) && \
# https://stackoverflow.com/questions/2395284/round-a-divided-number-in-bash
# Build and install Ceres
RUN   USE_PROC="$(nproc)" && \
      apt-get update && apt-get install -y \
          cmake \
          libatlas-base-dev \
          libeigen3-dev \
          libgoogle-glog-dev \
          libsuitesparse-dev \
          python-catkin-tools \
          "ros-${ROS_DISTRO}-cv-bridge" \
          "ros-${ROS_DISTRO}-image-transport" \
          "ros-${ROS_DISTRO}-message-filters" \
          "ros-${ROS_DISTRO}-tf" && \
      rm -rf /var/lib/apt/lists/* && \
      git clone https://ceres-solver.googlesource.com/ceres-solver && \
      cd ceres-solver && \
      git checkout "tags/${CERES_VERSION}" && \
      mkdir build && cd build && \
      cmake .. && \
      make "-j$(USE_PROC)" install && \
      rm -rf ../../ceres-solver && \
      mkdir -p "$CATKIN_WS/src/VINS-Mono/"

# Copy VINS-Mono (deprecated) assumes you have copied in the HKUST repo
#COPY ./ $CATKIN_WS/src/VINS-Mono/
# use the following line if you only have this dockerfile
RUN git clone https://github.com/HKUST-Aerial-Robotics/VINS-Mono.git

# Build VINS-Mono
WORKDIR $CATKIN_WS
ENV TERM xterm
ENV PYTHONIOENCODING UTF-8
RUN catkin config \
      --extend "/opt/ros/$ROS_DISTRO" \
      --cmake-args \
        -DCMAKE_BUILD_TYPE=Release && \
    catkin build && \
    sed -i '/exec "$@"/i \
            source "/root/catkin_ws/devel/setup.bash"' /ros_entrypoint.sh
