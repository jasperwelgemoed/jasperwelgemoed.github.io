---
layout: post
title: Improving 3D Object Detection with Modular Enhancements to the CenterPoint Framework (2025)
description: This project investigates improvements to a CenterPoint-based LiDAR 3D object detector on the View of Delft dataset. Key extensions include semantic fusion via PointPainting, data augmentation for both LiDAR and image modalities, and architectural changes such as a multiview fusion neck and dropout. 
skills: 
- AI
- Advanced Machine Perception
- GitHub
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
The project focused on enhancing the **CenterPoint 3D object detector** code base for the **View of Delft** dataset using practical improvements tailored to computational constraints. Specifically, we aimed to improve the mean average precision of the baseline performance for this network. 

### Baseline Performance
**Cars:** 89.65%  
**Pedestrians:** 58.38%  
**Cyclists:** 52.32%
**Mean Average Precision:** 66.78%


## Method
After analysing the existing code base, we concluded that the network was performing great at detecting larger objects like cars, but lacked the ability to detect smaller objects like bicycles and pedestrians. SO we decided to make the following adjustments to the existing base code:

### 1. Sensor Fusion
- **Technique:** Early fusion via **PointPainting**: each LiDAR point is enriched with semantic class scores from RGB image segmentation using DeepLabV3 (MobileNetV2 backbone).
- **Goal:** Improve classification and localization, especially for small or occluded objects.
- **Projection:** LiDAR points are projected into the image plane and assigned class probabilities.
- **Rejected Alternative:** BEVFusion was considered but not used due to high computational cost.

### 2. Data Augmentation
- **Image augmentations:** 50% of images were randomly flipped, color jittered, or converted to grayscale. LiDAR and ground truth were mirrored accordingly.
- **LiDAR augmentations:** Applied on the 7D fused representation using random rotation, scaling, and translation to introduce spatial diversity.

### 3. Backbone
- **Change:** Introduced **ResNet** as a deeper backbone to better capture fine-grained features for detecting small objects (e.g., cyclists, pedestrians).
- **Rationale:** Residual connections in ResNet mitigate vanishing gradients and preserve spatial detail.

### 4. Neck
- **Baseline:** Used **SECONDFPN** to generate a high-resolution BEV map from backbone features.
- **Experiment 1: Gated MultiViewFusion**
  - Combined voxel-level detail and high-level BEV context using a **learned gating mechanism**.
- **Experiment 2: Multi-Scale Gated Fusion (BiFPN-like)**
  - Integrated bidirectional flow across scales but didn’t outperform simpler fusion.
- **Regularization:** Dropout (0.2) was added to improve generalization and reduce overfitting

### 5. Head
- **Experimented** with intermediate fusion using projected image features and LiDAR features in BEV space (BEVFusion-based), but it exceeded compute limits.
- **Tuning the CenterPoint head** alone yielded subpar performance.



<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">
  

  <figure>
  <img src="/_projects/CenterpointProject/AMPResults.png" alt="StyleGAN diagram" width="400">
  <figcaption>Figure 1: Model Pipeline Overview</figcaption>
  </figure>
  
  <figure>
  <img src="/_projects/CenterpointProject/Pipeline.png" alt="Latent vector mixing" width="400">
  <figcaption>Figure 2: Style generation</figcaption>
  </figure>
  
  
</div>

## Key Result
The best-performing configuration was:  
**PointPainting + Data Augmentation + Gated Fusion**,  
which achieved an **overall mAP of 81.9%**, with particularly strong results for:
- **Cars:** 90.17%  
- **Pedestrians:** 73.95%  
- **Cyclists:** 81.59%
- **Mean Average Precision:** 81.90%

