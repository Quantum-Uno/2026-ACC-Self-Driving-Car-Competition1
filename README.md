# 2026-ACC-Self-Driving-Car-Competition

# Team Quantum Uno - Self-Driving Car Control Competition README

## Welcome to Our Esteemed Panel

We are excited to present our autonomous navigation system developed for the **ACC 2026 Self-Driving Car Competition**. This document provides an overview of our project, detailing the architecture, control strategies, and setup instructions.

---

## Project Overview

**Team Quantum Uno** has designed a sophisticated self-driving car system that leverages advanced algorithms for vehicle control, perception, and path planning. Our primary objectives include:

- Implementing a **Pure Pursuit Controller** for effective path tracking.
- Using a custom-trained **YOLO5** model for real-time detection of various traffic elements.
- Employing the **Breadth-First Search (BFS)** algorithm for optimal path planning in dynamic environments.

### Control Strategies

1. **Pure Pursuit Controller:** This controller allows the vehicle to follow a designated path smoothly by calculating the required steering angle based on the vehicle's position relative to the path.
2. **PID Controller with Stanley Controller:** This alternative approach was also tested to ensure robustness in various scenarios.

### Perception with YOLO5

Our perception module employs a custom-trained **YOLO5** model, which is integral to the vehicle's ability to navigate safely and efficiently. The model is designed to detect and classify various traffic elements in real-time, enhancing situational awareness. Key features include:

- **Traffic Light Detection:** Identifies the state of traffic lights (red, yellow, green) to make informed stopping decisions.
- **Sign Recognition:** Detects stop signs, yield signs, and roundabout signs, allowing the vehicle to adhere to traffic rules.
- **Obstacle Detection:** Recognizes cones and other obstacles in the environment, facilitating safe navigation.
- **Pedestrian Detection:** Identifies pedestrians in the vicinity to ensure the vehicle can react appropriately to prevent accidents.

The YOLO5 model enhances the vehicle's situational awareness and decision-making processes in real-time, contributing to a safer driving experience.

---

## Path Planning Using BFS

Our path planning module employs the **BFS algorithm**, which systematically explores all possible paths to determine the most efficient route from any starting point to a destination. The BFS approach ensures that the vehicle can navigate effectively while avoiding obstacles and adhering to traffic rules.

### BFS Implementation Steps

- **Adjacency Mapping:** Convert the map's node sequence into an adjacency list representation.
- **Queue-Based Exploration:** Begin from the initial position and explore all neighboring nodes at the current depth before advancing to the next level.
- **Backtracking for Path Reconstruction:** Once the destination is reached, backtrack using parent pointers to reconstruct the optimal path for the vehicle to follow.

---

## System Requirements

### Hardware & Software Specifications

- **Operating System:** Ubuntu 24.04
- **GPU:** NVIDIA (RTX 3060 or higher recommended)
- **Architecture:** x86_64
- **ROS 2 Version:** Humble (implemented via Docker container)
- **Python Versions:**
  - Host system: Python 3.11.4
  - Docker container: Python 3.8.10

---

## File Management in Development Environment

When you place files in the `python_dev` or `<non_ros_development>` folders, they will be accessible in the following directory once you start the Development Docker Container:

```
admin@username:/workspaces/isaac_ros-dev/<non_ros_development>
```

This structure facilitates organized development and easy access to your scripts and modules.

---

## LED Strip Configuration in ROS

To change the LED strip colors for the QCar 2 within the ROS framework, you can use the following command:

```bash
ros2 param set qcar2_hardware led_color_id <value>
```

The color values are defined as follows:

- **0:** Red
- **1:** Green
- **2:** Blue
- **3:** Yellow
- **4:** Cyan
- **5:** Magenta

This command allows you to dynamically alter the LED strip's color during operation, enhancing visual signaling for various driving conditions.

---

## Setting Up the Development Environment

### Important Note

Due to specific configurations in the LiDAR settings, it is essential to clone the entire repository to ensure compatibility with the `qcar2_nodes`.

### Installation Steps

1. **Clone the Repository and Move Files:**

   Open Terminal 1 and execute the following commands:

   ```bash
   git clone https://github.com/csie-foundation/ACC2026_Quanser_Student_Competition.git
   mv ACC2026_Quanser_Student_Competition/Setup_Real_Scenario_Interleaved.py /home/$USER/Documents/ACC_Development/docker/virtual_qcar2/python/Base_Scenarios_Python/
   rm -rf /home/$USER/Documents/ACC_Development/Development/ros2/src/*
   mv -f ACC2026_Quanser_Student_Competition/* /home/$USER/Documents/ACC_Development/Development/ros2/src/
   ```

2. **Start Docker and Install Dependencies:**

   Execute the following commands in Terminal 2:

   ```bash
   docker start isaac_ros_dev-x86_64-container
   docker exec -it isaac_ros_dev-x86_64-container bash
   cd /workspaces/isaac_ros-dev/ros2/src/quantumUno
   sudo apt update
   sudo apt install ros-humble-tf-transformations
   pip3 install -r requirements.txt
   ```

3. **Build the ROS2 Packages:**

   In the same terminal, run:

   ```bash
   cd /workspaces/isaac_ros-dev/ros2
   colcon build --symlink-install
   source install/setup.bash
   ```

---

## Running the System

To execute the navigation algorithm, open multiple terminals and follow these steps:

### Terminal 2 (for virtual_qcar2 container)

Initialize the virtual environment with obstacles:

```bash
docker start virtual-qcar2
docker exec -it virtual-qcar2 bash
python3 /home/qcar2_scripts/python/Base_Scenarios_Python/Setup_Real_Scenario_Interleaved.py --with_obstacle
```

### Terminal 1 (for isaac_ros_dev-x86_64-container)

Launch the simulation:

```bash
ros2 launch quantumUno run_sim.launch.py
```

### Terminal 3 (for detection node)

Execute the detection node:

```bash
docker exec -it isaac_ros_dev-x86_64-container bash
source /workspaces/isaac_ros-dev/ros2/install/setup.bash
ros2 run quantumUno Detection_node
```

### Terminal 4 (for control nodes)

Choose one of the following commands based on your requirements:

```bash
docker exec -it isaac_ros_dev-x86_64-container bash
source /workspaces/isaac_ros-dev/ros2/install/setup.bash
ros2 run quantumUno PurePursuit_node --with_obstacle # Runs Pure Pursuit with obstacle avoidance
ros2 run quantumUno PurePursuit_node # Runs Pure Pursuit without obstacle avoidance
ros2 run quantumUno PID_node # Runs PID without obstacle avoidance
```

---

## Conclusion

We appreciate your time in reviewing our project. If you have any questions or require further assistance, please feel free to reach out through our support channels.

---

## Additional Resources

- [YOLO5 Model for Real-Time Detection](link-to-yolo5)
- [Path Planning Using BFS](https://github.com/Quantum-Uno/2026-ACC-Self-Driving-Car-Competition1/tree/main/path_planning)
- [Pure Pursuit Controller Documentation](https://github.com/Quantum-Uno/2026-ACC-Self-Driving-Car-Competition1/tree/main/purepursuit)

Thank you for your evaluation!
