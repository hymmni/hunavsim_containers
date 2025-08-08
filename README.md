# hunavsim_containers

**This is a work in progress version**

This is a package with the files and tools required to build and run Docker containers with the HuNavSim and different Robotics simulators under ROS 2. Te available options are:

1. HuNavSim + Gazebo Classic 11 + ROS 2 Humble + PAL PMB2 robot
2. HuNavSim + Gazebo Fortress   + ROS 2 Humble  (**NO ROBOT FOR THE MOMENT!**)
3. HuNavSim + Isaac Sim         + ROS 2 Humble + different robots
4. HuNavSim + Webots            + ROS 2 Humble + PAL TIAGo robot"

Option 1 Gazebo Classic includes the [PMB2 Robot (ROS 2)](https://github.com/pal-robotics/pmb2_simulation/tree/humble-devel) robot of PAL robotics and the ROS 2 navigation system up.
Option 2 Gazebo Fortress does not include any robot for the moment. We will work to include it. 
Option 3 Isaac Sim allows to select between different robots.
Option 4 Webots includes the TIAGo robot of PAL Robotics ([TIAGO Lite](https://github.com/cyberbotics/webots_ros2/wiki/Example-TIAGo)). 

The containers contains all the required packages to run different simulations of the HuNavSim in the chosen Robotics Simulator. Moreover, the HuNavSim software is installed in a shared directory with the host system, so the user can modify or create new simulations and store them.   


# Dependencies

| Requirement | Notes / Links |
|-------------|---------------|
| **Docker**  | Install Docker Engine following the official guide → <https://docs.docker.com/desktop/setup/install/linux/> |
| **Git**     | Needed to clone the HuNavSim and wrappers repositories |
| **Nvidia container toolkit** | https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html |
| **Isaac Sim container prerequisites** <br>(**only for option 3 – Isaac Sim**) | Before running the *Isaac Sim* container you **must** complete the “Container Setup” steps in the official Isaac Sim docs (NVIDIA driver and Container Toolkit).<br>Guide → <https://docs.isaacsim.omniverse.nvidia.com/latest/installation/install_container.html#container-setup> |



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

## Execution Option 1, 2 and 4 (Gazebo Classic, Gazebo Fortress and Webots)

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

With option 4, the RViz with the HuNavSim panel will be opened so the user can create new scenarios (**ONGOING WORK**).

The user can create/modify the simulations throught the shared workspace. 

NOTE FOR GAZEBO CLASSIC: SOMETIMES, GAZEBO TAKES A LONG TIME TO LAUNCH THE FIRST TIME LEADING TO ERRORS IN THE SYSTEM. IN THAT CASE, STOP THE SYSTEM (CRTL+C), THE MENU WILL SHOW UP AGAIN, AND RE-RUN THE ENVIRONMENT AGAIN. IT SHOULD WORK THE SECOND TIME.

## Execution Option 3 (Isaac Sim)

### Running the Container

To start the container and automatically configure the simulation environment, run:

```bash
./run-hunav_isaac.bash
```

This script will:

- Prompt you for **Omniverse credentials** (only on the first run).
- Automatically build and source the **HuNav Isaac workspace**.
- Launch the Isaac Sim container.

### Omniverse Login (Required)

To connect to the **Nucleus server**, which allows **Isaac Sim** to download and cache asset packs locally, you must log in with your **Omniverse username and password**.

If you don’t have an account, you can register here:\
   [https://developer.nvidia.com/login](https://developer.nvidia.com/login)

Your credentials are securely stored using your system’s **keyring** (`libsecret` via `secret-tool`) after first use.\
You won’t be prompted again in future runs unless you manually reset them.

#### Resetting Credentials (Optional)

If you entered incorrect credentials or want to re-authenticate, you can run:

```bash
./run-hunav_isaac.bash --reset-credentials
```

This will:

- Clear the stored credentials from the system keyring.
- Prompt you again for your username and password.
- Immediately launch the container afterward.

### Inside the Container

Launch the simulation using one of the following options:

```bash
# Interactive launcher (recommended)
hunav_isaac

# Launch with custom configuration
hunav_isaac --config warehouse_agents.yaml --robot carter_ROS

# or use ROS2 command with same arguments (hunav_isaac is an alias for this)
ros2 run hunav_isaac_wrapper hunav_isaac_launcher
```

**Available robots**: `jetbot`, `create3`, `carter`, `carter_ROS`\
**Available worlds**: `warehouse`, `hospital`, `office`

### ⓘ Additional Notes

- The first launch may take **several minutes** while Isaac Sim populates its caches. Subsequent launches are significantly faster thanks to persistent volumes.
- The `Carter_ROS` robot is compatible with the **ROS 2 Nav2** stack for navigation. To control **Carter** robot via **ROS 2 Nav2**:

```bash
# Open a new terminal 
terminator &

# In the new terminal, execute:
cd /workspace/hunav_isaac_ws/src/Hunav_isaac_wrapper/src
ros2 launch carter_navigation carter_navigation.launch.py \
  params_file:="config/navigation_params/carter_navigation_params.yaml" \
  map:="scenarios/occupancy_maps/warehouse.yaml"
```
