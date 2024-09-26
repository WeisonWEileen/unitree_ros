FROM dockerpull.com/tiryoh/ros-melodic-desktop:latest

LABEL maintainer="weisonweileen@gmail.com"

#ROOT MODE
USER root

#CHANGE Apt Mirror Source
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list && \
  sed -i s@/security.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list && \
  apt-get clean && \
  apt-get update && \
  apt-get upgrade -y 

#INSTALL Development Tools
RUN apt-get update && apt-get install -y git gdb ros-melodic-controller-interface mlocate

#Get Params from devcontainer.json
ARG USERNAME=USERNAME
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
&& useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
&& apt-get update \
&& apt-get install -y sudo \
&& echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
&& chmod 0440 /etc/sudoers.d/$USERNAME 

#User Mode
RUN apt-get -y install ros-melodic-gazebo-ros-control ros-melodic-joint-state-controller ros-melodic-effort-controllers ros-melodic-joint-trajectory-controller 

USER $USERNAME
RUN echo 'export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib' >> ~/.bashrc  && \
    echo 'source /opt/ros/melodic/setup.bash' >> ~/.bashrc && \
    echo "alias cb='cmake --build build -j$(nproc)'" >> ~/.bashrc

WORKDIR /home/ws
USER root
RUN git clone https://github.com/unitreerobotics/unitree_ros.git  && \
  mv unitree_ros/* . && \
  rm -rf unitree_ros 
  # . /opt/ros/melodic/setup.sh 
  # catkin_make


ENV SHELL /bin/bash
# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

# [Optional] Set the default user. Omit if you want to keep the default as root.
CMD ["/bin/bash"]
