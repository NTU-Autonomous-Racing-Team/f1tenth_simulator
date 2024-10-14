# [DEPRECATED] F1tenth Simulator

The f1tenth simulator from `f1tenth_gym_ros` is containerised to avoid dependency issues and to shortcut setup.

## System
Ubuntu 20.04

## Setup

1. Clone repository and submodules
```
git clone --recursive https://github.com/NTU-Autonomous-Racing-Team/f1tenth_simulator.git
```

2. Build Dockerfile

Builds an image from the `f1tenth_gym_ros.Dockerfile` and tag it as `f1tenth:gym_ros_foxy`  

```
sudo docker build -f f1tenth_gym_ros.Dockerfile -t f1tenth:gym_ros_foxy .
```

3. Install dependencies (WIP i.e. SKIP THIS STEP)

```
sudo apt install -y python3-pip ros-foxy-ackermann-msgs
git clone https://github.com/f1tenth/f1tenth_gym
cd f1tenth_gym && pip3 install -e .
sudo apt install ros-foxy-navigation2
sudo apt install ros-foxy-nav2-bringup
sudo apt install ros-foxy-xacro
sudo apt install ros-foxy-joint-state-publisher
sudo rosdep init
rosdep update
rosdep install -i --from-path src --rosdistro foxy -y
```

4. Start the container to colcon build (only run once when repo is first cloned)

```
./run_gym_ros.sh
```

5. Start container to start the Rviz simulator

```
./run_gym_ros.sh
```

## Change maps

### f1tenth_racetracks

1. Open the `./simulator/sim_ws/src/config/sim.yaml` file in a text editor e.g. vim, nano, vscode, ...
2. Change the `map_path` under `map_parameters` to the path of the desired map. 

### custom ractracks
1. Copy the `map.png` and `map.yaml` into `./simulator/sim_ws/src/maps`
2. Change the directory in `./simulator/sim_ws/src/config/sim.yaml`

```
# map parameters
map_path: '/sim_ws/src/f1tenth_gym_ros/maps/<new_map>'
```

## Troubleshoot

1. Test if topic are publishing
    1. Test: `ros2 topic list`
    2. Expected:
        ```
        root@f1tenth_gym_ros:/# ros2 topic list
        /clicked_point
        /clock
        /cmd_vel
        /drive
        /ego_racecar/odom
        /ego_robot_description
        /goal_pose
        /initialpose
        /joint_states
        /map
        /map_server/transition_event
        /map_updates
        /parameter_events
        /rosout
        /scan
        /tf
        /tf_static
        ```
2. If you face a cmake cache issue, remove the generated files in the `f1tenth_ws`
    1. ```
       rm -rf build install log
       ```
    2. ```
       colcon build --packages-select <package name>
       ```

## Resources

-   [f1tenth_gym_ros](https://github.com/f1tenth/f1tenth_gym_ros.git)
-   [f1tenth_racetracks](https://github.com/f1tenth/f1tenth_racetracks.git)
-   [ROS Tutorial/Using GUIs with Docker](http://wiki.ros.org/docker/Tutorials/GUI)
