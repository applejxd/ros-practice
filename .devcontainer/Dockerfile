# cf. https://qiita.com/hinamimi/items/b3dd159f956628cebdbb
FROM tiryoh/ros-desktop-vnc:noetic AS ros-practice_base
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

RUN apt-get update && \
  apt-get install -y zsh time tzdata tree git curl

#--------#
# catkin #
#--------#

ENV CATKIN_WS=/root/catkin_ws
RUN mkdir -p ${CATKIN_WS}/src
WORKDIR ${CATKIN_WS}
RUN catkin init

RUN git clone https://github.com/HKUST-Aerial-Robotics/A-LOAM.git ${CATKIN_WS}/src/A-LOAM
RUN catkin build

#-----#
# zsh #
#-----#

RUN apt-get install -y fzf

#---------#
# cleanup #
#---------#

RUN apt-get clean && \ 
  rm -rf /var/lib/apt/lists/*

RUN chsh -s /bin/zsh

RUN mkdir /workspace
WORKDIR /workspace

#------#
# User #
#------#

FROM ros-practice_base AS ros-practice_user

ARG USERNAME=user
ARG GROUPNAME=user
ARG UID=1000
ARG GID=1000

# RUN if ! grep -q $GID /etc/group; then groupadd -g $GID $GROUPNAME; fi
# RUN if ! grep -q $UID /etc/group; then useradd -m -s /bin/zsh -u $UID -g $GID $USERNAME; fi
RUN groupadd -g $GID $GROUPNAME
RUN useradd -m -s /bin/zsh -u $UID -g $GID $USERNAME
USER $USERNAME
