![diagram-export-19-2-2024-15_55_34](https://github.com/RoverTech-team/arm/assets/49610092/1f6bd142-8c0c-4def-ab68-5a0c6c00acd4)



# ROS2 development setup

This is a guide to make the setup process easier for everyone, some may find the steps trivial but it's to get everybody on board before we start the development.

### Install Docker
[﻿https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) 

**Possible errors**

- you have to enable hardware assisted virtualization on windows [look here](https://forums.docker.com/t/hardware-assisted-virtualization-and-data-execution-protection-must-be-enabled-in-the-bios/109073) 
- if you have an older version of operating system you may want to install an older version of docker [﻿look here](https://docs.docker.com/desktop/release-notes/) 
### Description
This Docker container is set up for running a robotic simulation environment with the following specifications:

**ROS Version:** The container is configured to run with ROS (Robot Operating System) in the "Humble" release.

**Gazebo Version:** Gazebo, a simulation tool, is included in version 11.10

**Operating System:** The base operating system is Ubuntu 22.04.3 LTS.

---

# Set up(github)

[downlad github desktop here](https://desktop.github.com/)

Download github desktop and clone this repository (arm). By doing open in finder/file explorer you will be able to locate the folder GitHub on your computer. **Save the absolute path of this folder**. (it's needed in the next steps)

---

# Create your first branch(github)

From the githb desktop app go to current branch (which is going to be set on main) and click New Branch. Name it with your username. (during development each branch is going to represent a feature) 

From github desktop you can open the whole arm repository on vscode. (from the top bar: repository -> open in visual studio code)

---

# Set up(docker)

- **important note**: in docker before executing the following commands you have to set in preferences -> resources -> file sharing the path to the github folder 

MacOS:

```
docker run -p 6080:80 --security-opt seccomp=unconfined --shm-size=512m -v /users/<username>/<path_to_github>:/github --name roverchallenge vossgit/ros-roverchallenge:stable
```
Windows:

```
docker run -p 6080:80 --security-opt seccomp=unconfined --shm-size=512m -v C:\<absolute_path_to_github>:/github --name roverchallenge vossgit/ros-roverchallenge:stable
```
At this point you should get as output a bunch of `RUNNING state` lines and you can proceed.



- note: replace for example the whole string like <absolute_path_to_github> -> /user/doc/github
- **very important note**: do not remove or replace :/github because it's the path for the folder inside of the virtual machine



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
The `-v /users/<username>/<path_to_github>:/github` part of the Docker run command establishes a volume mount. This allows you to share data between your host machine and the Docker container. Here's a breakdown of this volume mount:


`/users/<username>/<path_to_github>`: with the actual path on your host machine that corresponds to the github folder, that way it will be accessible from the Docker container.

`:/github`: This is the path inside the Docker container where the shared data will be available. In this case, it's mounted at `/github`.

---


# Set up(host machine)
the following commands are going to be executed from a terminal inside your host machine:

After having entered the container with:

```
docker exec -it roverchallenge /bin/sh
```
We have to set the command line to use bash by writing

```
bash
```
let's source ROS 2
```
. /opt/ros/humble/local_setup.bash 
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
colcon build --cmake-clean-cache
```
[﻿source and more commands](https://answers.ros.org/question/333534/when-to-use-cmake-cleanconfigure/)

---

# VNC + GUI (ubuntu)
To visualize rviz and gazebo go to 

[﻿localhost:6080](http://localhost:6080) 

you should see a vnc viewer screen. Connect.

the following commands are going to be executed inside the ubuntu gui from the vnc viewer on localhost:6080


to source ROS 2
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
**if gazebo does not start correctly / does not load the model**

It might not start and give "waiting for serviece /spawn_entity" logs and in the end it could retourn "process has died". To fix that start this **before** running gazebo.launch.py in a different terminal 


```
gazebo -s libgazebo_ros_init.so -s libgazebo_ros_factory.so myworld.world
```
then by keeping this process going, open another terminal (dont forget the recurring instructions(look at the last paragraph of this readme))
```
ros2 launch rover_model gazebo.launch.py
```
if it still does not start restarting the whole docker container often helps

---



# Recurring instructions to set up a terminal. When to use colcon and when install 

In a terminal on the HOST MACHINE
```
echo "source /opt/ros/humble/local_setup.bash" >> ~/.bashrc
echo "source /github/arm/setup/rovertest/install/setup.bash" >> ~/.bashrc
source ~/.bashrc

sudo printf "source /github/arm/setup/rovertest/install/setup.bash\n" >> /home/ubuntu/.bashrc
source /home/ubuntu/.bashrc
```

Now you can open a new terminal in ubuntu or your host machine and there will be no need to do 
```
. /opt/ros/humble/local_setup.bash 
. install/setup.bash
```


**note**
every time we open a new terminal at the moment we have to source ROS 2 for both **UBUNTU** and **HOST MACHINE**
```
. /opt/ros/humble/local_setup.bash 
```

**second commands if you are on vnc UBUNTU**
```
cd ../../
```
```
cd github/arm/setup/rovertest
```

**second commands if you are on the HOST MACHINE**
```
bash
```
```
cd github/arm/setup/rovertest
```



**colcon**

```
colcon build 
```
generally used in **HOST MACHINE** where we have our code editor. It's made to build the packages, we use it each time we edit the files inside rover_model (same for any other ros package). It updates the files inside of rovertest/build/rover_model. 

Therefore we always want to execute this command from 

```
cd github/arm/setup/rovertest
```



****

