---
layout: post
title: Autonomous Apple-picking Robot Multidisciplinary Project (2025)
description: Multidisciplinary software development project for an autonomous apple-picking robot, capable of navigating an orchard, detecting and classifying ripe apples, picking them without damage, and communicating its status to a farmer with minimal human supervision.
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

The aim of the project was to develop software packages for an autonomous apple-picking robot to support the UN’s Sustainable Development Goal of Zero Hunger based on the Mirte Master platform-an educational robot designed by the TU Delft. The robot was designed to assist UNity Orchard—an ecological apple farm facing labor shortages and harsh working conditions—by autonomously navigating the orchard, detecting and picking ripe apples, and communicating its status to the farmer, all while minimizing disruption and ensuring safe operation in a dynamic environment. 

## Method
In a group totalling five students we divided tasks based on each student's expertise (Planning & Decision/Machine Perception/Human Robot Interaction/Manipulation). I had the role of systems engineer. My main responsibility was to integrate all created ROS-nodes and to ensure that the software package was capable of meeting our main mission and functional requirements.

I created a **functional hierarchy** with six core parent functions:

1. Perceive environment  
2. Navigate around orchard  
3. Detect apples  
4. Manage gripper  
5. Communicate with farmer  
6. Manage exceptions  

These were decomposed into child functions and visualized using a **SysML block diagram**, an **N² chart**, and a **functional flow diagram** with swimlanes to show node responsibilities.

I integrated each function via dedicated or shared ROS 2 nodes:

- **SLAM Toolbox** was used to map the environment and localize the robot on a static map.
- A custom **EmergencyBrakeCheck** node read sonar data to perform emergency braking when dynamic obstacles (e.g. workers, animals) were detected within a restricted range.
- The **NavPlanner** node used the `nav2` stack with a SMAC lattice planner, restricted to spin-and-forward motion for simpler obstacle avoidance.
- The **AppleDetection** node used **YOLOv8** (via Ultralytics) and **OpenCV** to detect ripe apples using RGB images from both base and gripper-mounted cameras.
- The **GripperPlanner** node calculated inverse kinematics-based trajectories for the robotic arm and controlled the gripper’s grasping and placing behavior.
- A **Tkinter-based GUI** was built in the **FarmerInterface** node, allowing the farmer to issue commands (start, stop, return) and monitor robot status.
- A **Finite State Machine (FSM)** node coordinated node activation and transitions, managing operational flow and top-level exceptions.

The robot’s operational loop was:

1. Navigate to a detected tree  
2. Detect apples  
3. Pick apple  
4. Navigate to basket  
5. Place apple  
6. Update farmer and repeat  

Fallback logic was implemented for failed detections, dropped apples, and low battery scenarios. This modular and hierarchical approach ensured robust autonomous operation in a dynamic orchard environment, aligned with the system’s functional requirements and sustainable agriculture goals.

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">


  <figure>
  <img src="/_projects/02_MDP/FFDMDP.png" alt="Functional Flow Diagram MDP" width="700">
  <figcaption>Figure 1: Functional Flow Diagram MDP  </figcaption>
  </figure>
  
  
</div>

  
## Results Summary

A total of **12 system-level tests** were conducted to validate the robot’s functionality against the defined requirements. Out of these, **10 tests passed successfully**, demonstrating strong performance in perception, manipulation, and communication. Two tests related to navigation did not pass due to incomplete integration of the `nav2` stack with the MIRTE robot.

<div style="display: flex; gap: 20px; flex-wrap: wrap; justify-content: space-between; align-items: flex-start;">

  <div style="flex: 1; min-width: 300px;">
    <p><strong>Startup and System Readiness:</strong><br>
    The robot initialized correctly, handled state transitions reliably, and maintained clear communication with the farmer via a responsive GUI.</p>
    {% include youtube-video.html id="cQ7CS-sukfs" autoplay="false" %}
  </div>

  <div style="flex: 1; min-width: 300px;">
    <p><strong>Localization and State Awareness:</strong><br>
    The robot accurately localized itself with &lt;5 cm error using a Monte Carlo particle filter and successfully detected basket positions.</p>
    {% include youtube-video.html id="Mrpe3mMtA3s" autoplay="false" %}
  </div>

</div>

**Camera Perception and Object Detection** & **Manipulation:**  
  Apple detection reached 90% accuracy within 2 meters, and relative position estimates had average errors of 1.5–2 cm.  
  Apple picking and placement achieved success rates above 85%, with reliable arm retraction behavior.

<div style="display: flex; gap: 20px; flex-wrap: wrap; justify-content: center; align-items: flex-start;">

  <figure style="flex: 1; min-width: 300px;">
    <img src="/_projects/02_MDP/Appledetection.jpg" alt="Apple Detection" width="100%">
    <figcaption>Figure 1: Apple Detection</figcaption>
  </figure>

  <div style="flex: 1; min-width: 300px;">
    {% include youtube-video.html id="IvshLTwPhNs" autoplay="false" %}
  </div>

</div>
---

