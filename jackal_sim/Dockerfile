#FROM nvidia/opengl:1.2-glvnd-devel-ubuntu16.04
FROM ros:melodic as simul_base
ENV NVIDIA_VISIBLE_DEVICES \
    ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES \
    ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics
RUN apt-get update \
    && apt-get install -y --no-install-recommends -qq git \
        ros-melodic-jackal-simulator \
        ros-melodic-jackal-desktop \
        ros-melodic-jackal-navigation \
        ros-melodic-uuv-gazebo-worlds \
        ros-melodic-ros-control \
        ros-melodic-gazebo-ros-control \
        ros-melodic-realsense2-camera \
        ros-melodic-realsense2-description \
        ros-melodic-joint-state-publisher-gui \
        ros-melodic-compressed-image-transport \
        ros-melodic-gazebo-plugins

FROM simul_base as jackal_base

WORKDIR /root/catkin_ws/src
RUN git clone https://github.com/clearpathrobotics/cpr_gazebo.git
RUN git clone https://github.com/aws-robotics/aws-robomaker-small-warehouse-world.git
WORKDIR /root/catkin_ws
RUN /bin/bash -c 'source /opt/ros/melodic/setup.bash && catkin_make'

FROM jackal_base 
COPY ./urdfs /root/.urdfs
ENV JACKAL_URDF_EXTRAS=/root/.urdfs/realsense.urdf.xacro
WORKDIR /root/.urdfs
