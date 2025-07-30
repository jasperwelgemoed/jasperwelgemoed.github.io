---
layout: post
title: Multidisciplinary Project
description: Multidisciplinary Software Development Project for an Autonomous Apple Picking Robot
skills: 
- Project Management
- Systems Engineering
- ROS2
- Python
- C++
main-image: /MDPFront.png
---

## Project Documentation
<div style="display: flex; flex-wrap: wrap; gap: 12px; margin-bottom: 20px;">

  <a href="/assets/MDP_Report.pdf" target="_blank" style="
    background-color: #007bff;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    📄 Project Report
  </a>

  <a href="https://github.com/jasperwelgemoed/MultidisciplinaryProject" target="_blank" style="
    background-color: #20c997;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;
    display: inline-block;">
    🔗 View GitHub Repository
  </a>

</div>

---

## Aim of the project

The aim of the project was to develop an autonomous apple-picking robot to support the UN’s Sustainable Development Goal of Zero Hunger. The robot was designed to assist UNity Orchard—an ecological apple farm facing labor shortages and harsh working conditions—by autonomously navigating the orchard, detecting and picking ripe apples, and communicating its status to the farmer, all while minimizing disruption and ensuring safe operation in a dynamic environment.

## Method
### Method Summary

The method consisted of designing, implementing, and integrating a ROS 2-based software system for an autonomous apple-picking robot, guided by a structured functional architecture.

A **functional hierarchy** was created with six core parent functions:

1. Perceive environment  
2. Navigate around orchard  
3. Detect apples  
4. Manage gripper  
5. Communicate with farmer  
6. Manage exceptions  

These were decomposed into child functions and visualized using a **SysML block diagram**, an **N² chart**, and a **functional flow diagram** with swimlanes to show node responsibilities.

Each function was implemented via dedicated or shared ROS 2 nodes:

- **SLAM Toolbox** was used to map the environment and localize the robot on a static map.
- A custom **EmergencyBrakeCheck** node read sonar data to perform emergency braking when dynamic obstacles (e.g. workers, animals) were detected within 20 cm.
- The **NavPlanner** node used the `nav2` stack with a SMAC lattice planner, restricted to spin-and-forward motion for simpler obstacle avoidance.
- The **AppleDetection** node used **YOLOv8** (via Ultralytics) and **OpenCV** to detect ripe apples using RGB images from both base and gripper-mounted cameras.
- The **GripperPlanner** node calculated inverse kinematics-based trajectories for the robotic arm and controlled the gripper’s grasping and placing behavior.
- A **Tkinter-based GUI** was built in the **FarmerInterface** node, allowing the farmer to issue commands (start, stop, return) and monitor robot status.
- A **Finite State Machine (FSM)** node coordinated node activation and transitions, managing operational flow and top-level exceptions.

The robot’s operational loop was:

1. Navigate to a suspected tree  
2. Detect apples  
3. Pick apple  
4. Navigate to basket  
5. Place apple  
6. Update farmer and repeat  

Fallback logic was implemented for failed detections, dropped apples, and low battery scenarios. This modular and hierarchical approach ensured robust autonomous operation in a dynamic orchard environment, aligned with the system’s functional requirements and sustainable agriculture goals.

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">


  <figure>
  <img src="/_projects/MDP/FFDMDP.png" alt="Functional Flow Diagram MDP" width="700">
  <figcaption>Figure 1: Network Pipeline Overview  </figcaption>
  </figure>
  
  
</div>


  
## Key Result
We performed training, validation and testing on these techniques separately and later combined multiple of the adjustments into one model to see if they together yield better performance. After performing training, validation and testing on these different combined models, we yielded the following results. 
### **3D Object Detection mAP Comparison**

| Method                                        | Car mAP (%) | Pedestrian mAP (%) | Cyclist mAP (%) | Overall mAP (%) |
|----------------------------------------------|-------------|---------------------|------------------|------------------|
| Baseline (Standard FPN)                      | 89.65       | 58.38               | 52.32            | 66.78            |
| ResNet                                       | 70.00       | 54.00               | 73.00            | 66.00            |
| Single-Level Gated Fusion                    | 66.23       | 54.98               | 81.76            | 67.66            |
| Multi-Scale Gated Fusion (BiFPN-like)        | 68.91       | 50.52               | 76.89            | 65.44            |
| Gated Fusion + Dropout (0.2)                 | 80.32       | 44.61               | 76.47            | 67.13            |
| PointPainting (only)                         | 70.65       | 66.88               | 85.59            | 74.37            |
| PointPainting + Data Augmentation            | 70.47       | 72.52               | 85.55            | **76.18**        |
| Tuned CenterPoint Head                       | 72.47       | 46.05               | 71.94            | 63.68            |

For the best performing model (PointPainting + Data Augementation, we saw a significant increase in the pedestrian and cyclist class, but also a significant drop in the Car mAP. So we decided to add gated fusion plus a dropout during training as here the performance drop on the car was minimal. This turned out to work great and yielded the following results. <br> <br>
The best-performing configuration was:  
**PointPainting + Data Augmentation + Gated Fusion**,  
which achieved an **overall mAP of 81.90%** which is a **performance increase of 15.12%** compared to the baseline model.<br>

| Class       | mAP (%) |
|-------------|---------|
| Cars        | 90.17   |
| Pedestrians | 73.95   |
| Cyclists    | 81.59   |
| **Overall** | **81.90** |


---

