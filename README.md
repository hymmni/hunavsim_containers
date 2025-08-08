# hunavsim_containers

**This is a work in progress version**

This is a package with the files and tools required to build and run Docker containers with the HuNavSim v1.0 and different Robotics simulators under ROS 2. Te available options are:

1. HuNavSim + Gazebo Classic 11 + ROS 2 Humble + PAL PMB2 robot
2. HuNavSim + Gazebo Fortress   + ROS 2 Humble  (**NO ROBOT FOR THE MOMENT!**)
3. HuNavSim + Webots            + ROS 2 Humble + PAL TIAGo robot

Option 1 Gazebo Classic includes the [PMB2 Robot (ROS 2)](https://github.com/pal-robotics/pmb2_simulation/tree/humble-devel) robot of PAL robotics and the ROS 2 navigation system up.
Option 2 Gazebo Fortress does not include any robot for the moment. We will work to include it. 
Option 3 Webots includes the TIAGo robot of PAL Robotics ([TIAGO Lite](https://github.com/cyberbotics/webots_ros2/wiki/Example-TIAGo)). 

The containers contains all the required packages to run different simulations of the HuNavSim in the chosen Robotics Simulator. Moreover, the HuNavSim software is installed in a shared directory with the host system, so the user can modify or create new simulations and store them.   


# Dependencies

| Requirement | Notes / Links |
|-------------|---------------|
| **Docker**  | Install Docker Engine following the official guide → <https://docs.docker.com/desktop/setup/install/linux/> |
| **Git**     | Needed to clone the HuNavSim and wrappers repositories |



# Installation

Once you have installed docker, you can install the system by executing the bash script *install.sh*.
First give execution permission to the script from a terminal:

```sh
chmod +x install.sh
```

Then you can execute it:

```sh
./install.sh
```

This script will ask you about the option that you want to install, and will show the installation progress. 
**The HuNavSim software and the indicated wrapper, will be installed in a shared workspace shared with the docker container, located inside the indicated simulator directory, so you can modify and create your own environments with persistance.**   


# Execution

After system installation, the installation program will show on the screen the name of the script that is required to run the system. Before execute it, we must check the execution permissions (only required the first time).

```sh
chmod +x given_script_name.bash
```

Execute the indicated script:

```sh
./given_script_name.bash
```

## Execution

The previous script will run the docker container and will compile the current workspace. Moreover it will show a menu with the possible options. It will show the stored scenarios that can be executed (.yaml files in the *scenarios* directory of the running wrapper) along with other options. For example:

```sh
========= HuNavSim Docker Menu =========
  1) Run environment agents_cafe.yaml
  2) Run environment agents_house.yaml
  3) Run environment agents_warehouse.yaml
  4) Create a new environment with RViz
  5) Open a bash shell
  6) Exit
========================================
Select an option (number): 
```

Option 4 allows to create new scenarios over existing environments. The program will show the available environments (ROS maps in the *maps* folder that must match the simulator environments under the *worlds* folder). RViz with the HuNavSim panel will be opened so the user can create new scenarios: define the agents and their positions and goals on the map.

The user can also create/modify the simulations throught the shared workspace. 

NOTE FOR GAZEBO CLASSIC: SOMETIMES, GAZEBO TAKES A LONG TIME TO LAUNCH THE FIRST TIME LEADING TO ERRORS IN THE SYSTEM. IN THAT CASE, STOP THE SYSTEM (CRTL+C), THE MENU WILL SHOW UP AGAIN, AND RE-RUN THE ENVIRONMENT AGAIN. IT SHOULD WORK THE SECOND TIME.

