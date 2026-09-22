FROM ros:jazzy

ARG USERNAME=ros_user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Workspace sources are bind-mounted at runtime, so package-specific
# dependencies are installed by rosdep in postCreateCommand, not here.
RUN apt-get update && apt-get full-upgrade -y && apt-get install -y \
  sudo \
  ros-jazzy-ros2-control \
  ros-jazzy-ros2-controllers \
  ros-jazzy-controller-manager \
  ros-jazzy-moveit \
  ros-jazzy-moveit-py \
  ros-jazzy-rmw-cyclonedds-cpp \
  && ldconfig \
  && rm -rf /var/lib/apt/lists/*

# Ubuntu 24.04 images ship an 'ubuntu' user at UID 1000; replace it with ours.
RUN if id -u ubuntu >/dev/null 2>&1; then userdel -r ubuntu; fi \
  && groupadd --gid $USER_GID $USERNAME \
  && useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME \
  && echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
  && chmod 0440 /etc/sudoers.d/$USERNAME \
  && echo "source /opt/ros/jazzy/setup.bash" >> /home/$USERNAME/.bashrc \
  && echo "[ -f /home/ros2_ws/install/setup.bash ] && source /home/ros2_ws/install/setup.bash" >> /home/$USERNAME/.bashrc

USER $USERNAME
