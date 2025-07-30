---
layout: post
title: Student Team Delft Aerospace Rocket Engineering (2022)
description: "Performed the role of Chief Systems Engineer at Delft Aerospace Rocket Engineering (DARE) for the new flagship rocket: Stratos V, a fully reusable bi-propellant liquid fuel engine built on a 3D-printed rocket engine to be launched to space in 2027."
skills: 
- Project Management
- Systems Engineering
- Mission Design
- System Integration
- Rocket Propulsion
main-image: /StratosVFront.png
---

## Project Documentation
<div style="display: flex; flex-wrap: wrap; gap: 12px; margin-bottom: 20px;">

  <a href="/assets/Stratos_V_Preliminary_Project_Planning_Report.pdf" target="_blank" style="
    background-color: #007bff;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    📄 Project Report
  </a>

  <a href="/assets/StratosVSymposium.pdf" target="_blank" style="
    background-color: #007bff;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    📄 Project Report
  </a>
</div>

---

## Aim of the project

The aim of the Stratos V project is to design, build, test, launch to space, recover, and re-launch a refurbishable, liquid bi-propellant rocket to demonstrate the feasibility of reusable and sustainable launch vehicles in student and amateur rocketry. The 2022/23 team specifically focuses on preliminary and detailed design as well as testing selected subsystems, laying the foundation for future teams to achieve full reusability and break DARE’s internal altitude record of 12.5km.

## Responsibilities
Documentation Management
Providing self made systems engineering tools to the engineering
Setting up meetings
Quality management
Planning of the entire project

## Method

To realize the mission of developing a reusable launch vehicle, I set up a structured systems-engineering-driven approach. The planning and development process is based on the **NASA Systems Engineering Handbook**, which divides the project into **seven phases**: from Pre-Phase A (concept studies) to Phase F (closeout).

My task as chief systems engineer was to ensure the engineers could do what they were hired for, their own specific engineering task. My task was to be the glue between the different departments and engineers to ensure the functional requirements for the project would be validated and verified and that the team kept their eyes on the same end-goal. I ensured this by setting up multiple methods for the team to use.

- **Team Organization**: First I setup the team structure using a **matrix management structure**, dividing members into departments and sections with shared responsibilities. This enabled clear communication paths and division of tasks across all subsystems. 
<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">


  <figure>
  <img src="/_projects/MDP/Teamstructure.png" alt="Team Structure" width="700">
  <figcaption>Figure 1: Team Structure  </figcaption>
  </figure>
  
  
</div>
 
- **Phased Development**: Then I broke up the project into logical phases (Pre-Phase A to F), each with clear goals, decision points, and deliverables. Early phases focus on feasibility, requirements, and design, while later ones cover fabrication, testing, operations, and decommissioning. I setup daily meetings to keep track of the department their progress during each phase. 
  
- **Requirement-Driven Design**: The team defined clear **Mission** and **Stakeholder Requirements** to guide all activities. These include technical goals (e.g., reusability, altitude targets), documentation, safety compliance, budget constraints, and stakeholder communication.
  
- **Functional Decomposition**: The mission is analyzed using a **Functional Flow Diagram (FFD)** and **Functional Breakdown Structure (FBS)**, which identify what the rocket must do from production to recovery. These are used to derive and structure design requirements.

- **Work Planning Tools**: A set of **Work Flow Diagrams (WFDs)** and **Work Breakdown Structures (WBSs)** were created for each project phase, allowing for top-down planning. This is supplemented by **Gantt charts** that show timelines, task durations, and milestones.



- **Knowledge Transfer**: The 2022/23 team focuses on cold flow testing and preliminary subsystem design, ensuring that thorough documentation is produced to allow the next year’s team to continue development seamlessly.

- **Risk and Resource Management**: A **SWOT and organizational risk analysis** was conducted to identify internal and external challenges. Preliminary budgets were drafted and are subject to DARE board approval, while the team actively seeks sponsorships to offset costs.

In summary, the Stratos V method combines professional aerospace engineering principles with educational goals, ensuring technical progress, team growth, and alignment with industry trends in sustainable and reusable spaceflight.

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">


  <figure>
  <img src="/_projects/MDP/FFDMDP.png" alt="Functional Flow Diagram MDP" width="700">
  <figcaption>Figure 1: Functional Flow Diagram MDP  </figcaption>
  </figure>
  
  
</div>


  
## Results Summary

A total of **12 system-level tests (T1–T12)** were conducted to validate the robot’s functionality against the defined requirements. Out of these, **10 tests passed successfully**, demonstrating strong performance in perception, manipulation, and communication. Two tests (T3 and T10) related to navigation did not pass due to incomplete integration of the `nav2` stack with the MIRTE robot.

- **Startup and System Readiness:**  
  The robot initialized correctly, handled state transitions reliably, and maintained clear communication with the farmer via a responsive GUI.

  {% include youtube-video.html id="cQ7CS-sukfs" autoplay= "false"%}

- **Localization and State Awareness:**  
  The robot accurately localized itself with <5 cm error using a Monte Carlo particle filter and successfully detected basket positions.
  {% include youtube-video.html id="Mrpe3mMtA3s" autoplay= "false"%}


- **Camera Perception and Object Detection:**  
  Apple detection reached 90% accuracy within 2 meters, and relative position estimates had average errors of 1.5–2 cm.
  <div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">
  
  
    <figure>
    <img src="/_projects/MDP/Appledetection.jpg" alt="Apple Detection" width="400">
    <figcaption>Figure 1: Apple Detection  </figcaption>
    </figure>
    
    
  </div>

- **Manipulation:**  
  Apple picking and placement achieved success rates above 85%, with reliable arm retraction behavior.
  {% include youtube-video.html id="IvshLTwPhNs" autoplay= "false"%}


#### Unsuccessful Tests

- **Autonomous Navigation (T3):**  
  Although effective in simulation, the robot could not perform goal-directed navigation in real-world conditions due to incomplete `nav2` integration.  

- **Dynamic Obstacle Avoidance (T10):**  
  While the robot successfully detected nearby obstacles and triggered alerts, it could not replan or resume navigation paths autonomously.  

---
