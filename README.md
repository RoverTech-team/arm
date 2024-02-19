![diagram-export-19-2-2024-15_55_34](https://github.com/RoverTech-team/arm/assets/49610092/1f6bd142-8c0c-4def-ab68-5a0c6c00acd4)


# (STILL NOT FINISHED DO NOT USE THIS)
# ROS2 development setup

This is a guide to make the setup process easier for everyone, some may find the steps trivial but it's to get everybody on board before we start the development.

### Install Docker
[﻿https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) 

**Possible errors**

- you have to enable hardware assisted virtualization on windows [﻿forums.docker.com/t/hardware-assisted-virtualization-and-data-execution-protection-must-be-enabled-in-the-bios/109073](https://forums.docker.com/t/hardware-assisted-virtualization-and-data-execution-protection-must-be-enabled-in-the-bios/109073) 
- if you have an older version of operating system you may want to install an older version of docker [﻿docs.docker.com/desktop/release-notes/](https://docs.docker.com/desktop/release-notes/) 
### Description
This Docker container is set up for running a robotic simulation environment with the following specifications:

**ROS Version:** The container is configured to run with ROS (Robot Operating System) in the "Humble" release.

**Gazebo Version:** Gazebo, a simulation tool, is included in version 11.10

**Operating System:** The base operating system is Ubuntu 22.04.3 LTS.



MacOS:

```
docker run -p 6080:80 --security-opt seccomp=unconfined --shm-size=512m -v /users/<username>/<path_to_github>:/github --name roverchallenge vossgit/ros-roverchallenge:2
```
Windows:

```
docker run -p 6080:80 --security-opt seccomp=unconfined --shm-size=512m -v C:\Users\<username>\<path_to_github>:/github --name roverchallenge vossgit/ros-roverchallenge:2
```
At this point you should get as output a bunch of `RUNNING state` lines and you can proceed.



note: replace the whole string like <username> -> gabrielvoss same with <path_to_github>



### Accessing the GUI
After running the container, you can access the graphical user interface (GUI) by opening a web browser and navigating to `http://localhost:6080`. The container exposes the GUI on port 6080, allowing you to interact with the simulation environment.

To restart the container when closed(you should always have the docker app running in the background) in the computer terminal/shell write:docker start roverchallenge



### To restart the container when closed
(you should always have the docker app running in the background) in the computer terminal/shell write:

```
docker start roverchallenge
```
```
docker exec -it roverchallenge /bin/sh
```
### Note
The `-v /users/<username>:/github` part of the Docker run command establishes a volume mount. This allows you to share data between your host machine and the Docker container. Here's a breakdown of this volume mount:

`/users/<username>`: Replace `<username>` with the actual username or path on your host machine that you want to make accessible to the Docker container.

`:/github`: This is the path inside the Docker container where the shared data will be available. In this case, it's mounted at `/github`.

---

# Set up(host machine)
After having entered the container with:

```
docker exec -it roverchallenge /bin/sh
```
We have to set the command line to use bash by writing

```
bash
```
navigate into

```
cd github/arm/setup/rovertest
```
where we are going to build the cmake files and install the packages

```
colcon build
```
```
. install/setup.bash
```
**Possible errors**

if the colcon build does not instantly work try to 

```
(toadd)
```
---

# VNC + GUI (ubuntu)
To visualize rviz and gazebo go to 

[﻿localhost:6080](http://localhost:6080) 

you should see a vnc viewer screen. Connect.


at the moment we have to initialize ros like this (work is in the process to remove this)
```
. /opt/ros/humble/local_setup.bash 
```
Now open a terminal with the terminator app 

now we can navigate to the system folder of ubuntu (which is simply /) with

```
cd ../../
```
navigate into

```
cd github/arm/setup/rovertest
```
then let's setup the packages with

```
. install/setup.bash
```
**Start rviz**

```
ros2 launch rover_model display.launch.py
```
**Start gazebo**

```
ros2 launch rover_model gazebo.launch.py
```
**Possible errors with gazebo**

It might not start and give "segment not loaded" errors or the model might not load. To fix that start this **before** running gazebo.launch.py in a different terminal 

```
gazebo -s libgazebo_ros_init.so -s libgazebo_ros_factory.so myworld.world
```






# When to use colcon and when setup


**note**
every time we open a new terminal at the moment we have to initialize ros like this (work is in the process to remove this)
```
. /opt/ros/humble/local_setup.bash 
```

**colcon**

```
colcon build 
```
it's made to build the env, we use it each time we edit the files inside rover_model (same for any other ros package). It updates the files inside of rovertest/build/rover_model. 

Therefore we always want to execute this command from 

```
cd github/arm/setup/rovertest
```
**setup**

```
. install/setup.bash
```
this command is used for installing all the packages in our current session of the terminal, therefore we need to execute it **all the times** that we open a **new terminal**, both when we are on the **HOST MACHINE** and when we are on the **localhost:6080** page accessing the ubuntu gui

we execute it from  

```
cd github/arm/setup/rovertest
```



****

