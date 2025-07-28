---
layout: post
title: Improving 3D Object Detection with Modular Enhancements to the CenterPoint Framework (2025)
description: This project investigates improvements to a CenterPoint-based LiDAR 3D object detector on the View of Delft dataset. Key extensions include semantic fusion via PointPainting, data augmentation for both LiDAR and image modalities, and architectural changes such as a multiview fusion neck and dropout. 
skills: 
- AI
- Advanced Machine Perception
- GitHub
- DelftBlue Supercomputer
main-image: /PointPainting.jpg
---

## Project Documentation
<div style="display: flex; flex-wrap: wrap; gap: 12px; margin-bottom: 20px;">

  <a href="/assets/AMP_Final_Assignment___My_title.pdf" target="_blank" style="
    background-color: #007bff;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    📄 Project Report
  </a>

  <a href="/assets/AMP_Group15_Code_Submission.zip" download style="
    background-color: #20c997;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    ⬇️ Download Project ZIP
  </a>
    <a href="/assets/AMP_Group15_Wandb_logs_Final_Detection_Pipeline_PoitPainting_DataAugmentation" download style="
    background-color: #20c997;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    ⬇️ Download Weights & Biases ZIP
  </a>



</div>

---

## Aim of the project
The project focused on enhancing the **CenterPoint 3D object detector** code base for the **View of Delft** dataset using practical improvements tailored to computational constraints set by the DelftBlue Supercomputer of the TU Delft. Specifically, we aimed to improve the mean average precision of the baseline performance for this network. 

### **Baseline Performance**

| Class       | mAP (%) |
|-------------|---------|
| Cars        | 89.65   |
| Pedestrians | 58.38   |
| Cyclists    | 52.32   |
| **Overall** | **66.78** |


## Method
After analysing the existing code base, we concluded that the network was performing great at detecting larger objects like cars, but lacked the ability to detect smaller objects like bicycles and pedestrians. So we decided to make the following adjustments to the existing base code:

### 1. Sensor Fusion
**Technique:** Early fusion via PointPainting each LiDAR point is enriched with semantic class scores from RGB image segmentation using DeepLabV3 (MobileNetV2 backbone). <br>
**Goal:** Improve classification and localization, especially for small or occluded objects. <br>
**Projection:** LiDAR points are projected into the image plane and assigned class probabilities. <br>
**Rejected Alternative:** BEVFusion was considered but not used due to high computational cost. <br>

### 2. Data Augmentation
**Image augmentations:** 50% of images were randomly flipped, color jittered, or converted to grayscale. LiDAR and ground truth were mirrored accordingly. <br>
**LiDAR augmentations:** Applied on the 7D fused representation using random rotation, scaling, and translation to introduce spatial diversity. <br>

### 3. Backbone
**Change:** Introduced **ResNet** as a deeper backbone to better capture fine-grained features for detecting small objects (e.g., cyclists, pedestrians). <br>
**Rationale:** Residual connections in ResNet mitigate vanishing gradients and preserve spatial detail. <br>

### 4. Neck
**Baseline:** Used **SECONDFPN** to generate a high-resolution BEV map from backbone features. <br>
**Experiment 1: Gated MultiViewFusion** <br>
Combined voxel-level detail and high-level BEV context using a **learned gating mechanism**. <br>
**Experiment 2: Multi-Scale Gated Fusion (BiFPN-like)** <br>
Integrated bidirectional flow across scales but didn’t outperform simpler fusion. <br>
**Regularization:** Dropout (0.2) was added to improve generalization and reduce overfitting <br>

### 5. Head
**Experimented** with intermediate fusion using projected image features and LiDAR features in BEV space (BEVFusion-based), but it exceeded compute limits. <br>
**Tuning the CenterPoint head** alone yielded subpar performance. <br>

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
