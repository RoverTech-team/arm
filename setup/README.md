# DEPRECATED, LOOK AT THE README INSIDE THE PARENT FOLDER ARM

This guide provides step-by-step instructions for setting up the Rover Challenge environment and working with ROS2 packages.

In this guide we assume that the folder rovertest is located directly in the my_files forlder of the ubuntu docker image.

## Initial Terminal Setup
(opening a terminal from the host machine)
When opening the terminal for the first time, execute the following commands:

```bash
docker exec -it roverchallenge /bin/sh
```

```bash
bash
```

```bash
sudo apt update
```

```bash
. /opt/ros/humble/local_setup.bash 
```

### Explanation:
- `docker exec -it roverchallenge /bin/sh`: Enters the Docker container named "roverchallenge."
- `sudo apt update`: Updates the package list.
- `. /opt/ros/humble/local_setup.bash`: Sets up the ROS environment.

## Workspace Setup

Navigate to the working directory and set up the workspace:

```bash
cd /my_files
mkdir -p rovertest/src
cd rovertest
colcon build
```

### Explanation:
- `cd /my_files`: Changes the directory to "/my_files."
- `mkdir -p rovertest/src`: Creates necessary directories for the ROS workspace.
- `cd rovertest`: Enters the ROS workspace.
- `colcon build`: Builds the workspace.

## ROS Package Creation

Create a ROS package named "rover_model":

```bash
cd src
ros2 pkg create --build-type ament_cmake rover_model
cd ..
colcon build
```

### Explanation:
- `cd src`: Changes to the source directory of the workspace.
- `ros2 pkg create --build-type ament_cmake rover_model`: Creates a ROS package with the specified build type.
- `colcon build`: Builds the workspace.

## Installing the Package


install the package

```bash
. install/setup.bash
```

Verify the installation of the package:

```bash
ros2 pkg list
```

### Explanation:
- `ros2 pkg list`: Lists installed ROS packages.
- `. install/setup.bash`: Sets up the environment for the installed packages.


## Files setup
Now you can copy the content from this repo located at (rovertest/src/rover_model) inside the package forlder in ubuntu located at my_files/rovertest/src/rover_model

## Starting a New Terminal

On the host computer, open a new terminal and execute the following commands:

```bash
docker exec -it roverchallenge /bin/sh
```

Inside the Docker container, run:

```bash
bash
```

```bash
. /opt/ros/humble/local_setup.bash 
```

```bash
cd /my_files/rovertest
. install/setup.bash
```

If a command needs to be executed from Ubuntu, run:

```bash
cd /my_files/rovertest
. install/setup.bash
```

## Building a Program

From the directory "/my_files/rovertest," typically build a written program using:

```bash
colcon build
```

### Explanation:
- `colcon build`: Builds the ROS program in the specified directory.

## URDF Setup

The urdf references the stl files inside of the meshes folder.
The urdf file is contained inside of rovertest/src/rover_model/urdf
Open a terminal for VS Code builds from the main folder "/my_files/rovertest":

```bash
colcon build
```


```bash
cd /my_files/rovertest
. install/setup.bash
```


## Running RViz

### Without launch.py

Execute the following commands from the From the Ubuntu terminal within the VNC Viewer GUI:

```bash
cd /my_files/rovertest
. install/setup.bash
```

```bash
ros2 run robot_state_publisher robot_state_publisher --ros-args -p robot_description:="$(xacro /my_files/rovertest/src/rover_model/urdf/arm.urdf.xacro)"
ros2 run joint_state_publisher_gui joint_state_publisher_gui
ros2 run rviz2 rviz2
```

Ensure the fixed frame is set to "world," add "tf" and "robot model" with the description topic "/robot_description." Save the configuration in "src/rover_model/rviz."

### With launch.py
(launch.py is located in rovertest/src/rover_model/launch)

After creating the "launch.py" file, use the following command to launch RViz:

(from an ubuntu terminal in the the vnc gui window)
```bash
ros2 launch rover_model display.launch.py
```


## Running Gazebo
(gazebo.launch.py is located in rovertest/src/rover_model/launch)

From "/my_files/rovertest," launch Gazebo:

(from an ubuntu terminal in the the vnc gui window)
```bash
ros2 launch rover_model gazebo.launch.py
```
