---
layout: post
title: Development of a Small Scale Tool Switching Mechanism for Agricultural Robotic Manipulators
description: Designed, developed and tested a reliable, low-cost, lighteweight toolswitching mechanism to work on a mobile robot with a manipulator; an open source educational mobile robot platform developed by the TU Delft.
skills: 
- Project Management
- CAD (Solidworks)
- ROS
main-image: /MMDesignB.png
---

## Project Documentation
<div style="display: flex; flex-wrap: wrap; gap: 12px; margin-bottom: 20px;">

  <a href="/assets/Robotics_Bachelor_Thesis_BEP_Group_9.pdf" target="_blank" style="
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
The aim of this project is to develop a small-scale, low-budget agricultural tool-switching mechanism compatible with the Mirte Master robot, specifically optimized for greenhouse conditions. The goal is to enable the robot to switch between different tools autonomously, addressing challenges such as precision limitations, moisture and dirt resistance, and cost-efficiency, ultimately improving the robot’s functionality for owners of smaller greenhouses.


## Method
The methodology of this project follows a structured NASA systems engineering and project management approach, broken down into seven distinct phases, each with specific objectives and deliverables to ensure systematic progress and integration. These phases include:

| **Phase**                                         | **Description**                                                                                   |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------|
| **Pre-Phase A – Mission Study**                  | Identifying stakeholders, refining the project scope, and formulating a clear mission need statement. |
| **Phase A – Concept Development**                | Analyzing the previous design, defining system-level requirements, and generating multiple design concepts. |
| **Phase B – Preliminary Design**                 | Selecting the most promising concept through trade-offs and testing early prototypes using 3D printing. |
| **Phase C – Detailed Design**                    | Finalizing the design for manufacturing, including CAD models and connection definitions.         |
| **Phase D/E – Production, Testing, and Final Iteration** | Manufacturing the system, conducting integration with the parallel group, and performing functional and performance tests. |
| **Phase F – Verification and Documentation**     | Verifying requirement compliance and compiling the final report.                                  |


This phased methodology ensures a well-documented, collaborative, and test-driven development process for creating a functional tool-switching mechanism.

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">


  <figure>
  <img src="/_projects/CenterpointProject/Pipeline.png" alt="Network Pipeline Overview" width="700">
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
