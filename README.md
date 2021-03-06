# fv_sensor package

This is a package containing fingervision tactile sensor driver codes and processing nodes for wrench estimation, grasping force adaptation and visualization utilities (implemented fully in Python).

For full sensor development, one can refer to https://arxiv.org/abs/1810.02653, please cite it if you find this code benefitial for your projects.

- - -
System setup:

  Software:
  - Ubuntu 14.04
  - python 2.7
  - Ros kinetic
  - opencv 3.3.0 (comes with ros distribution)
  
  Hardware:
  - UR10 robot arm
  - Robotiq 2f-140 gripper

- - -
Required packages:
  - [ur_modern_driver](https://github.com/ros-industrial/ur_modern_driver/tree/kinetic-devel)
  - [robotiq meta package](https://github.com/ros-industrial/robotiq/tree/kinetic-devel)
  
- - -
Beforce launch the whole thing, one need to check
1. Connect sensors' usb to computer with an order of left to right, then check camera index with following script.
  ```bash
  ls /dev/video*
  ```
2. Connect Robotiq 2-finger gripper through UR controller box to computer under modbus protocol, then make sure you get through steps on this [site](http://wiki.ros.org/robotiq/Tutorials/Control%20of%20a%202-Finger%20Gripper%20using%20the%20Modbus%20RTU%20protocol%20%28ros%20kinetic%20and%20newer%20releases%29).

3. Make sure you setup your local IP with same router for UR robot. You can setup your ip with
  ```sh
  sudo ifconfig [your ethernet module name] [ip_adress]
  ```
  

- - -
Launching steps (on my setup, you may need carefully adapt to your own environment):
- Launch ur10 driver and visulization
  ```sh
  roslaunch ur_modern_driver ur10_bringup.launch  limited:=true robot_ip:=192.168.1.102
  roslaunch ur10_moveit_config ur10_moveit_planning_execution.launch limited:=true
  roslaunch ur10_moveit_config moveit_rviz.launch  config:=true
  ```
- Launch gripper base node and simple controller node
  ```sh
  rosrun robotiq_2f_gripper_control Robotiq2FGripperRtuNode.py /dev/ttyUSB1
  rosrun robotiq_2f_gripper_control Robotiq2FGripperStatusListener.py
  rosrun robotiq_2f_gripper_control Robotiq2FGripperSimpleController.py
  ```
- Launch fv_sensor nodes
  ```sh
  roslaunch fv_sensor fv_sensor.launch
  ```


