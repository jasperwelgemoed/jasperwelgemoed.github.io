---
layout: post
title: Improving 3D Object Detection with Modular Enhancements to the CenterPoint Framework (2025)
description:  This project investigates improvements to a CenterPoint-based LiDAR 3D object detector on the View of Delft dataset. Key extensions include semantic fusion via PointPainting, data augmentation for both LiDAR and image modalities, and architectural changes such as a multiview fusion neck and dropout. 
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

For this project, my teammates and I aimed to explore implementations of Computer Vision to assist people with any handwriting disabilities/illnesses in expressing themselves through one of the main forms of communication and identity in life: their (lost) handwriting. Some of these people have lost the ability to write at a later age. Which is why we mainly aimed to be able to train a machine learning network on their old existing handwriting such that they can use our model such that they can continue writing using their old handwriting style.

## Method

We used the working-principle of Generative Adversarial Networks to recreate and manipulate individual handwriting styles. We developed methods to condition GANs on one-hot encoded identity and character vectors, and trained StyleGANs to extract and reapply handwriting style features. We also created our own dataset of handwritten characters to see if we could replicate our own handwriting.

### 1. One-Hot Conditioned GAN

We first trained a GAN conditioned on both the character (e.g., 'A', 'B', 'C') and the writer's identity, using one-hot encoded vectors. This setup allowed the model to separate **content** (what to write) from **style** (how it looks), enabling generation of letters in different personal handwriting styles.

### 2. Style Extraction with StyleGAN

We then used **StyleGAN**, a powerful GAN architecture that generates images progressively through multiple layers. It leverages a latent style vector to control high- and low-level features such as letter shape, stroke thickness, and curvature. Using StyleGAN, we combined the latent vectors (**W**) of two samples to mix different handwriting styles—e.g., taking the structure from one and stroke style from another.

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">
  

  <figure>
  <img src="https://hackmd.io/_uploads/Bkj13sMakg.png" alt="StyleGAN diagram" width="400">
  <figcaption>Figure 1: Model Pipeline Overview</figcaption>
  </figure>
  
  <figure>
  <img src="https://hackmd.io/_uploads/H15YhghTJe.jpg" alt="Latent vector mixing" width="400">
  <figcaption>Figure 2: Style generation</figcaption>
  </figure>
  
  
</div>
<br>

To apply this to real handwriting, we approximated the latent vector of an input image using **MSE loss**. This allowed us to embed and manipulate real handwriting styles for synthetic generation.

## Results

We successfully trained a StyleGAN to generate handwriting that mimics a learned style from datasets like EMNIST.

<figure>
  <img src="/_projects/GANComputerVisionProject/GANGeneration.gif" alt="Training Example" width="400">
  <figcaption>Figure 3: GAN Handwriting Style Generation</figcaption>
</figure>
<br>
