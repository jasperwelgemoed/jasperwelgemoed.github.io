---
layout: post
title: Recreating Handwriting using Generative Adverserial Networks (2025)
description: Developed a Computer Vision model using Generative Adversarial Networks (GANs) to recreate personalized handwriting styles, enabling the generation of handwritten text from style inputs using datasets like MNIST, NIST, and custom handwriting samples. 
skills:
  - AI
  - Computer Vision
main-image: /image.png
---
## Project Documentation
<div style="display: flex; flex-wrap: wrap; gap: 12px; margin-bottom: 20px;">

  <a href="https://hackmd.io/@jasperwelgemoed/Bk8wh2Sjx" target="_blank" style="
    background-color: #007bff;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    📄 Project Report
  </a>

  <a href="https://colab.research.google.com/drive/17uTkq5Dj2RDwaOkxgT3SqELEoRYLTl47?usp=sharing" target="_blank" style="
    background-color: #20c997;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    💻 Data Extraction Code
  </a>

  <a href="https://colab.research.google.com/drive/162i1A9PvQPk1pWAMXpI4sF0svXwUVVTT?usp=sharing" target="_blank" style="
    background-color: #20c997;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    💻 Char One-hot Training
  </a>

  <a href="https://colab.research.google.com/drive/15fV1xdnsQa_IY56wmCsjLJg7CLByYHdi#scrollTo=Gq6eUDbDqNwp" target="_blank" style="
    background-color: #20c997;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    💻 Char+Style One-hot Training
  </a>

  <a href="https://colab.research.google.com/drive/17nQXRduWsscCrEuTa0UI-Qaxis3ZdfsGB?usp=sharing" target="_blank" style="
    background-color: #20c997;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    💻 StyleGAN + Extraction
  </a>

</div>

---

## Aim of the project

For this project, my teammates and I aimed to explore implementations of Computer Vision to assist people with any handwriting disabilities/illnesses in expressing themselves through one of the main forms of communication and identity in life: their (lost) handwriting. Some of these people have lost the ability to write at a later age. Which is why we mainly aimed to be able to train a machine learning network on their old existing handwriting such that they can use our model such that they can continue writing using their old handwriting style.

## Method

## Methodology Summary

The project focused on enhancing the **CenterPoint 3D object detector** for the **View of Delft** dataset using practical improvements tailored to computational constraints. The enhancements were made in four main areas:

---

### 1. Sensor Fusion
- **Technique:** Early fusion via **PointPainting**: each LiDAR point is enriched with semantic class scores from RGB image segmentation using DeepLabV3 (MobileNetV2 backbone).
- **Goal:** Improve classification and localization, especially for small or occluded objects.
- **Projection:** LiDAR points are projected into the image plane and assigned class probabilities.
- **Rejected Alternative:** BEVFusion was considered but not used due to high computational cost.

---

### 2. Data Augmentation
- **Image augmentations:** 50% of images were randomly flipped, color jittered, or converted to grayscale. LiDAR and ground truth were mirrored accordingly.
- **LiDAR augmentations:** Applied on the 7D fused representation using random rotation, scaling, and translation to introduce spatial diversity.

---

### 3. Backbone
- **Change:** Introduced **ResNet** as a deeper backbone to better capture fine-grained features for detecting small objects (e.g., cyclists, pedestrians).
- **Rationale:** Residual connections in ResNet mitigate vanishing gradients and preserve spatial detail.

---

### 4. Neck
- **Baseline:** Used **SECONDFPN** to generate a high-resolution BEV map from backbone features.
- **Experiment 1: Gated MultiViewFusion**
  - Combined voxel-level detail and high-level BEV context using a **learned gating mechanism**.
- **Experiment 2: Multi-Scale Gated Fusion (BiFPN-like)**
  - Integrated bidirectional flow across scales but didn’t outperform simpler fusion.
- **Regularization:** Dropout (0.2) was added to improve generalization and reduce overfitting.

---

### 5. Head
- **Experimented** with intermediate fusion using projected image features and LiDAR features in BEV space (BEVFusion-based), but it exceeded compute limits.
- **Tuning the CenterPoint head** alone yielded subpar performance.

---

## Key Result
The best-performing configuration was:  
**PointPainting + Data Augmentation + Gated Fusion**,  
which achieved an **overall mAP of 81.9%**, with particularly strong results for:
- **Cars:** 90.17%  
- **Pedestrians:** 73.95%  
- **Cyclists:** 81.59%


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

## Results

We successfully trained a StyleGAN to generate handwriting that mimics a learned style from datasets like EMNIST.

<figure>
  <img src="/_projects/GANComputerVisionProject/GANGeneration.gif" alt="Training Example" width="400">
  <figcaption>Figure 3: GAN Handwriting Style Generation</figcaption>
</figure>
<br>


   
