# arm

### Install Docker
https://www.docker.com/products/docker-desktop/

### Description

This Docker container is set up for running a robotic simulation environment with the following specifications:

**ROS Version:** The container is configured to run with ROS (Robot Operating System) in the "Humble" release.

**Gazebo Version:** Gazebo, a simulation tool, is included in version 11.10

**Operating System:** The base operating system is Ubuntu 22.04.3 LTS.

### Command to Install the container

From the Terminal (macOS) or PowerShell (Windows):
When you use the docker run command, you are essentially instructing Docker to download an image from a container registry (if the image is not already present locally) and then run a container based on that image.

MacOS:

```plaintext
docker run -p 6080:80 --security-opt seccomp=unconfined --shm-size=512m -v /users/<username>:/my_files --name roverchallenge tiryoh/ros2-desktop-vnc:humble
```

Windows:

```plaintext
docker run -p 6080:80 --security-opt seccomp=unconfined --shm-size=512m -v C:\Users\<username>:/my_files --name roverchallenge tiryoh/ros2-desktop-vnc:humble
```

At this point you should get as output a bunch of `RUNNING state` lines and you can proceed.

### Accessing the GUI

After running the container, you can access the graphical user interface (GUI) by opening a web browser and navigating to `http://localhost:6080`. The container exposes the GUI on port 6080, allowing you to interact with the simulation environment.

### To restart the container when closed
(you should always have the docker app running in the background)
in the computer terminal/shell write:
```plaintext
docker start roverchallenge
```


### Note

The `-v /users/<username>:/my_files` part of the Docker run command establishes a volume mount. This allows you to share data between your host machine and the Docker container. Here's a breakdown of this volume mount:

`/users/<username>`: Replace `<username>` with the actual username or path on your host machine that you want to make accessible to the Docker container.

`:/my_files`: This is the path inside the Docker container where the shared data will be available. In this case, it's mounted at `/my_files`.

### Troubleshooting

In case the volume mount does not work:  
In the Docker Preferences or Settings window, find and click on the "Resources" tab.  
File Sharing Section: Look for a section titled "File Sharing" or a similar name. This is where you specify which paths on your host machine should be accessible to Docker containers.
