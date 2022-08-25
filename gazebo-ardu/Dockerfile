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
