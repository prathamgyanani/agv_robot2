## Running the Simulation

### Pre-requisites

1. **Ubuntu 20.04.6** (Ubuntu Mate recommended)
2. **ROS 2 Foxy** must be installed

### Setup Instructions

1. **Install ROS 2 Foxy**:
   Follow the installation instructions for ROS 2 Foxy [here](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html).

2. **Source ROS Foxy**:
   Add the following line to your `.bashrc` file to source ROS Foxy automatically:
   ```bash
   echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```

3. **Clone the Repository**:
   Create a directory for your workspace, navigate into it, and clone the repository into the `src` folder:
   ```bash
   mkdir -p ~/your_workspace/src
   cd ~/your_workspace/src
   git clone <repository_url>
   ```

4. **Build the Workspace**:
   Navigate back to the workspace root and build the project using `colcon`:
   ```bash
   cd ~/your_workspace
   colcon build --symlink-install
   ```

### Running the Simulation

1. Open **five terminals** and in each terminal, source the workspace setup file:
   ```bash
   source ~/your_workspace/install/setup.bash
   ```

2. In each terminal, execute the following commands respectively:

   **Terminal 1**: Launch the simulation with the specified world file:
   ```bash
   ros2 launch agv_robot2 launch_sim.launch.py world:=./src/agv_robot2/worlds/obstacles.world
   ```

   **Terminal 2**: Run the twist multiplexer:
   ```bash
   ros2 run twist_mux twist_mux --ros-args --params-file ./src/agv_robot2/config/twist_mux.yaml -r cmd_vel_out:=diff_cont/cmd_vel_unstamped
   ```

   **Terminal 3**: Launch SLAM Toolbox:
   ```bash
   ros2 launch slam_toolbox online_async_launch.py params_file:=./src/agv_robot2/config/mapper_params_online_async.yaml use_sim_time:=true
   ```

   **Terminal 4**: Launch Navigation:
   ```bash
   ros2 launch nav2_bringup navigation_launch.py use_sim_time:=true
   ```

   **Terminal 5**: Start RViz:
   ```bash
   rviz2
   ```

### Using the Simulation

1. After the simulation has started completely, navigate to the RViz window.
2. Set the **Initial Pose** using the '2D Pose Estimate' tool.
3. Set the **Goal Pose** using the '2D Nav Goal' tool.
