FROM osrf/ros:noetic-desktop-full

ENV DEBIAN_FRONTEND noninteractive
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV ROS_DISTRO=noetic

###################################### user #####################################

ENV SHELL=/bin/bash \
    USER=arg \
    UID=1000 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8

ENV HOME=/home/${USER}

RUN adduser --disabled-password \
    --gecos "Default user" \
    --uid ${UID} \
    ${USER} 

RUN echo "root:root" | chpasswd
RUN echo "${USER}:111111" | chpasswd

###################################### basic tools #####################################
RUN apt-get -o Acquire::ForceIPv4=true update && apt-get -yq dist-upgrade \
    && apt-get -o Acquire::ForceIPv4=true install -yq --no-install-recommends \
    software-properties-common \
    && add-apt-repository ppa:deadsnakes/ppa -y \
    && apt-get -o Acquire::ForceIPv4=true install -yq --no-install-recommends \
    python3.8 \
    && update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1

RUN apt-get -o Acquire::ForceIPv4=true update && apt-get -yq dist-upgrade \
    && apt-get -o Acquire::ForceIPv4=true install -yq --no-install-recommends \
    file \
    locales \
    cmake \
    unzip \
    make \
    git \
    vim \
    gedit \
    wget \
    sudo \
    lsb-release \
    build-essential \
    net-tools \
    dirmngr \
    gnupg2 \
    mercurial \
    libcanberra-gtk-module \
    libcanberra-gtk3-module \
    libpcap-dev \
    python3-pip \
    python3-setuptools \
    python3-opencv \
    python3-numpy \
    python3-empy  \
    python3-dev \
    gfortran \ 
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


##################################### PIP3 ######################################

RUN pip3 install --upgrade pip setuptools

RUN pip3 install \
    jupyter \
    jupyter_http_over_ws

RUN jupyter server extension enable --py jupyter_http_over_ws

RUN pip3 install \
    digi-xbee==1.3.0 \
    pyyaml \
    cython \
    catkin-tools \
    matplotlib \
    pandas \
    requests \
    scikit-image \
    scikit-learn \
    gdown \
    opencv-python \
    xarray==2022.3.0

RUN pip3 install \
    torch==1.5.0+cu101 \
    torchvision==0.6.0+cu101 \
    -f https://download.pytorch.org/whl/torch_stable.html


###### install ROS ############
# RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

# RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key "C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654"

# RUN apt update

# RUN apt-get -o Acquire::ForceIPv4=true update && apt-get -o Acquire::ForceIPv4=true install --no-install-recommends -y \
#     ros-melodic-desktop-full \
#     ros-melodic-mavros \
#     ros-melodic-mavros-extras \
#     ros-melodic-octomap-* \
#     ros-melodic-serial \
#     ros-melodic-soem \
#     ros-melodic-openslam-gmapping \
#     ros-melodic-geodesy \
#     ros-melodic-cartographer-* \
#     ros-melodic-cartographer-ros \
#     ros-melodic-cartographer-rviz \
#     ros-melodic-ddynamic-reconfigure \
#     ros-melodic-perception \
#     ros-melodic-navigation \
#     ros-melodic-rgbd-launch \
#     ros-melodic-joystick-drivers \
#     ros-melodic-pointcloud-to-laserscan \
#     ros-melodic-robot-localization \
#     ros-melodic-spacenav-node \
#     ros-melodic-tf2-sensor-msgs \
#     ros-melodic-controller-manager \
#     ros-melodic-twist-mux \
#     ros-melodic-velodyne-simulator \
#     ros-melodic-gazebo-ros \
#     ros-melodic-move-base-msgs \
#     ros-melodic-hector-gazebo \
#     ros-melodic-hector-trajectory-server \
#     ros-melodic-lms1xx \
#     ros-melodic-rviz-imu-plugin \
#     ros-melodic-interactive-marker-twist-server \
#     ros-melodic-teleop-twist-joy \
#     ros-melodic-rosserial-arduino \
#     ros-melodic-rosserial \
#     ros-melodic-cv-bridge \
#     python-rosdep \
#     python-rosinstall \
#     python-rosinstall-generator \
#     python-wstool \
#     ros-melodic-roslint \
#     ros-melodic-robot \
#     ros-melodic-robot-state-publisher \
#     ros-melodic-geographic-msgs \
#     ros-melodic-rqt \
#     ros-melodic-rqt-common-plugins \
#     ros-melodic-web-video-server \
#     ros-melodic-xacro \
#     ros-melodic-gps-common \
#     ros-melodic-ecl-threads \
#     ros-melodic-rosbridge-suite \
#     && apt-get clean \
#     && rm -rf /var/lib/apt/lists/*

RUN pip3 install rospkg \
    netifaces \
    defusedxml

# bootstrap rosdep
# RUN rosdep init 

# RUN rosdep fix-permissions
    
# RUN rosdep update
#### catkin_make=================
RUN apt-get update -y
RUN echo "root ALL=(ALL)  ALL" >> /etc/sudoers \
    && echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ${HOME}/.bashrc

##################################### setting #####################################

#### =================================thermal installation==========================
RUN apt-get update -y
# RUN cd spinnaker-3.0.0.118-amd64
COPY ./spinnaker-3.0.0.118-amd64/ /home/${USER}/thermal_sdk
RUN apt-get update -y
# RUN apt upgrade -y
RUN apt-get install -yq libusb-1.0-0 \
                        libavcodec58 \
                        libavformat58 \
                        libswscale5 \
                        libswresample3 \
                        libavutil56 \
                        qt5-default

# RUN sh /home/${USER}/thermal_sdk/install_spinnaker.sh
RUN apt-get update
# RUN dpkg -i /home/${USER}/thermal_sdk/libgentl_3.0.0.118_amd64.deb
# RUN dpkg -i libspinnaker_*.deb
# RUN dpkg -i libspinnaker-dev_*.deb
# RUN dpkg -i libspinnaker-c_*.deb
# RUN dpkg -i libspinnaker-c-dev_*.deb
# RUN dpkg -i libspinvideo_*.deb
# RUN dpkg -i libspinvideo-dev_*.deb
# RUN dpkg -i libspinvideo-c_*.deb
# RUN dpkg -i libspinvideo-c-dev_*.deb
# RUN apt-get install -y spinview-qt_*.deb
# RUN dpkg -i spinview-qt-dev_*.deb
# RUN dpkg -i spinupdate_*.deb
# RUN dpkg -i spinupdate-dev_*.deb
# RUN dpkg -i spinnaker_*.deb
# RUN dpkg -i spinnaker-doc_*.deb

#### ============download thermal camera python api
COPY ./spinnaker_python-3.0.0.118-cp38-cp38-linux_x86_64/ /home/${USER}/thermal_api
RUN apt-get update -y
RUN python3.8 -m pip install /home/${USER}/thermal_api/spinnaker_python-3.0.0.118-cp38-cp38-linux_x86_64.whl

#### ===============================================================================

RUN chown -R ${USER}:${USER} ${HOME}/
RUN echo "${USER} ALL=(ALL)  NOPASSWD: ALL" > /etc/sudoers
RUN echo "export PATH=/home/${USER}/.codon/bin:$PATH" > /home/${USER}/.bashrc
# Support of nvidia-docker 2.0
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=all

USER ${USER}

WORKDIR ${HOME}