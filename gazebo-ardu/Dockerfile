FROM carbon225/ros-gazebo:humble-fortress

RUN apt-get update


# Ardupilot (for SITL)
RUN git clone https://github.com/ArduPilot/ardupilot.git /code/ardupilot && \
    cd /code/ardupilot && \
    git submodule update --init --recursive
    # ignore root warning
RUN cd /code/ardupilot && \
    sed -i '6,9d' Tools/environment_install/install-prereqs-ubuntu.sh && \
    USER=root Tools/environment_install/install-prereqs-ubuntu.sh -y
ENV PATH=$PATH:/opt/gcc-arm-none-eabi-10-2020-q4-major/bin:/code/ardupilot/Tools/autotest:/usr/lib/ccache:/root/.local/bin:/root/bin
RUN cd /code/ardupilot && \
    ./waf configure --board sitl && \
    ./waf copter


# Ardupilot Gazebo plugin
RUN apt-get install -y rapidjson-dev libignition-gazebo6-dev
RUN git clone https://github.com/ArduPilot/ardupilot_gazebo -b ignition-fortress /code/ardupilot_gazebo
RUN cd /code/ardupilot_gazebo && \
    mkdir build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo && \
    make -j4
# make the plugin visible to gazebo
ENV IGN_GAZEBO_SYSTEM_PLUGIN_PATH=/code/ardupilot_gazebo/build:$IGN_GAZEBO_SYSTEM_PLUGIN_PATH
ENV IGN_GAZEBO_RESOURCE_PATH=/code/ardupilot_gazebo/models:/code/ardupilot_gazebo/worlds:$IGN_GAZEBO_RESOURCE_PATH


# Install VIM
RUN apt-get install -y vim

# Install mavros
RUN apt-get install -y ros-humble-mavros ros-humble-mavros-extras

# Configure scripts
COPY scripts /root/scripts
ENV PATH=$PATH:/root/scripts
RUN echo source '/opt/ros/$ROS_DISTRO/setup.bash' >> /root/.bashrc
