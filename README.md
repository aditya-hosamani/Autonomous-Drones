<h1> Autonomous Drones </h1>
A PX4-firmware based drone control written in Python

<h2>Environment setup instructions</h2>

The tutorial uses Ubuntu 18.04 LTS and ROS melodc

<h3> 1. Install ROS-Melodic </h3>

* Allow package download from ROS
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

* Setup your keys
```
curl -sSL 'http://keyserver.ubuntu.com/pks/lookup?op=get&search=0xC1CF6E31E6BADE8868B172B4F42ED6FBAB17C654' | sudo apt-key add -
```

* Installation: ROS-Desktop-Full
```
sudo apt install ros-melodic-desktop-full
```

* Environment setup
Save your ROS environment variables to the bashrc file
```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

* Install dependencies for building ROS packages
```
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool python-rosinstall-generator python-catkin-tools build-essential
```

* Install and initialize rosdep
```
sudo apt install python-rosdep
sudo rosdep init
rosdep update
```

<h3> 2. Setup Catkin workspace </h3>

* Initialize catkin worskpace
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin init
```

<h3> 3. Install PX4 and Mavros </h3>

* PX4 Firmware
```
wstool init ~/catkin_ws/src
cd ~/catkin_ws/src
git clone https://github.com/PX4/Firmware.git --recursive
cd Firmware
git checkout v1.11.1
bash ./Tools/setup/ubuntu.sh
```

* MAVLINK and MAVROS
```
wget https://raw.githubusercontent.com/PX4/Devguide/master/build_scripts/ubuntu_sim_ros_melodic.sh
bash ubuntu_sim_ros_melodic.sh
```

* Build the packages
```
cd ~/catkin_ws
catkin build
```

<h3> 4. Finalizing installation </h3>

The installation will be completed by updating the bashrc file with the recently installed ROS package locations.
<br>Add the following lines to the bottom of the bashrc file.</br>
```
source ~/catkin_ws/devel/setup.bash
source ~/catkin_ws/src/Firmware/Tools/setup_gazebo.bash ~/catkin_ws/src/Firmware/ ~/catkin_ws/src/Firmware/build/posix_sitl_default
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/catkin_ws/src/Firmware
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/catkin_ws/src/Firmware/Tools/sitl_gazebo
```

<h3> 5. Test-flight </h3>

The successful setup of ROS, PX4 and simulation environment can be verified by running the either comamnds

* PX4 SITL in JMAVSIM
```
cd ~/catkin_ws/src/Firmware
make px4_sitl_default jmavsim
```

* PX4 SITL in Gazebo
```
cd ~/catkin_ws/src/Firmware
roslaunch px4 mavros_posix_sitl.launch
```


