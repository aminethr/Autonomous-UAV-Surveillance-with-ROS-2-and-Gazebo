## 🛰️ Autonomous UAV Surveillance using ROS 2 and Gazebo

This project demonstrates a basic autonomous drone surveillance mission using ROS 2 and Gazebo. A drone patrols a predefined area and uses YOLOv8 object detection to identify people in the environment. If a person is detected, their GPS coordinates are published, which can be consumed by alert systems or other drones for coordinated response.

    ✅ This project is designed for a single UAV, but the world supports multi-drone (4 total) surveillance with minimal code modifications.
    

## 🧠 Key Features

    Uses YOLOv8n to detect people in real-time.

    Publishes alerts with GPS coordinates when a person is detected.

    Simulated GPS, depth camera, and image feed are used for geolocation.

    Custom patrol logic that:

        Moves forward a fixed distance.

        Turns 180° and returns.

        Loops continuously to simulate area surveillance.

## 🎮 Simulation Environment

    Gazebo world contains:

        A central office building to be monitored.

        Four drones, each in a corner surrounded by walls.

        Only one drone is active by default.

    To extend this to a drone swarm, simply replicate the ROS 2 logic for drones 2–4 and adjust topic names accordingly.

## 🚁 ROS 2 Node Overview

The core logic lives in drone_surveillance_node.py:

    Subscribed topics (may need to adjust names based on your setup):

        /drone0/local_position/pose

        /drone1_camera1_sensor/image_raw

        /drone1_camera1_sensor/depth/image_raw

        /drone1/global_position/global

        /drone1/global_position/compass_hdg

    Published topics:

        /drone1/setpoint_velocity/cmd_vel_unstamped: for movement commands

        /drone1/person_detected: publishes alerts like

        Person detected at GPS: lat=36.123456, lon=2.123456

    Detection logic:

        Converts image using cv_bridge.

        Runs YOLOv8 on RGB feed.

        Calculates bearing based on object location and camera field of view.

        Combines with GPS and depth to estimate real-world location of the detected person.

    📌 You may need to adjust topic names based on your own drone's namespace or Gazebo setup.

## 🛠️ Requirements

    ROS 2 (e.g., Humble)

    Gazebo

    Ultralytics YOLOv8

    cv_bridge, sensor_msgs, geometry_msgs, etc.

    Python dependencies: opencv-python, numpy, ultralytics, tf_transformations

## 📷 Example Output

Here’s an example of the system in action:

    The drone begins in one corner and moves forward.

    Then it turns 180° and returns when arriving to end of the wall .

    On detecting a person:

        GPS of the detected person is published to /drone1/person_detected.

## ✅ Detection Example
![Execution Example](https://github.com/user-attachments/assets/ed48c73c-e8b3-43de-a394-23b4f4fbb27e)
## 🧩 Extending to Multi-Drone

The current setup is for drone1. To support 4 drones:

    Update the node logic to handle multiple drone namespaces:

        /drone2/..., /drone3/..., etc.

    You can run multiple instances of the node, each configured for a different drone.

    Each drone can patrol a different area or segment of the building.


## 🔔 Applications

    Perimeter surveillance of critical infrastructure

    Multi-drone patrol coordination

    Real-time alert system for intrusions
