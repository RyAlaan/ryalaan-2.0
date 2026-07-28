---
resource: project
created: 2026-07-27
project: ROS-2
module: Environment & Workspace
task: Create new colcon workspace
language: bash
framework: ROS 2 Jazzy
repository: github.com/ryalaan/ros-2-jazzy/
tags:
  - project
  - documentation
  - ROS2-Jazzy
  - ROS2-Environtment
---

# Create a new colcon workspace

> **Objective**
>
> 1. Create a new colcon workspace (`ros2_ws`), source your ROS 2 Jazzy install, and verify `echo $ROS_DISTRO` shows `jazzy`.
> 2. Create a package called `my_robot_basics` using `ros2 pkg create`, once with `--build-type ament_python` and once with `--build-type ament_cmake` (separate packages), and build both with `colcon build`.

---

# 📌 Background

ROS workspace

---

# 🎯 Requirements

- [x] Create workspace ros2_ws
- [x] Sourcing ROS 2 jazzy
- [x] Create packages my_robot_basics both python and c++
- [x] Build both packages

---

# 💻 Code

```bash

$ mkdir -p ros2_ws/src # create workspace directory called ros2_ws

$ cd ros2_ws/src # navigate to created workspace

$ source /opt/ros/jazzy/setup.bash # sourcing ros environment

$ echo $ROS_DISTRO # this comment will return active ros distro
jazzy

$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 my_robot_basic_c # create python package named my_robot_basic_py using ament_python

$ ros2 pkg create --build-type ament_python --license Apache-2.0 my_robot_basic_py # create python package named my_robot_basic_py using ament_python

$ cd .. && colcon build # build all package inside current workspace

```

---

# 🧠 Why It Works

1. Why we need to source setup.bash everytime we open a new terminal ?

Source setup.bash is necessary because it will determine which environment we're gonna use like by updating critical variable like (PATH, ROS Distro, AMENT_PREFIX_PATH). Without sourcing, our terminal can't locate ROS executable command or find necessary library. [reference](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html#source-the-setup-files)

2. Command structure for create package

`ros2 pkg create --build-type <build-type> --license <license> <package-name> `

sub-commands: 
- pkg : used for various package related sub-command. there's many more ros2 sub-command like node, topic, service, ect. you can always use -h (help) if your're get lost.
- create : this command will create a new ROS 2 package. this sub-command level also have various command like executable, prefix, list, ect.

flags : 
- --build-type : this value depends what your package want to build with {cmake, ament_cmake, ament_cargo, ament_python}.
- --license : The license attached to this package; this can be an arbitrary string, but a LICENSE file will only be generated if it is one of the supported licenses

---

# 📚 References

- [[ROS 2 Environment]]

---

# 📌 Takeaways 

- setup.bash harus di source setiap terminal baru dibuka