---
layout: post
title: Recreating Handwriting using Generative Adverserial Networks (December 2024)
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

We used the working-principle of Generative Adversarial Networks to recreate and manipulate individual handwriting styles. We developed methods to condition GANs on one-hot encoded identity and character vectors, and trained StyleGANs to extract and reapply handwriting style features. We also created our own dataset of handwritten characters to see if we could replicate our own handwriting.

### 1. One-Hot Conditioned GAN

We first trained a GAN conditioned on both the character (e.g., 'A', 'B', 'C') and the writer's identity, using one-hot encoded vectors. This setup allowed the model to separate **content** (what to write) from **style** (how it looks), enabling generation of letters in different personal handwriting styles.

### 2. Style Extraction with StyleGAN

We then used **StyleGAN**, a powerful GAN architecture that generates images progressively through multiple layers. It leverages a latent style vector to control high- and low-level features such as letter shape, stroke thickness, and curvature. Using StyleGAN, we combined the latent vectors ($w$) of two samples to mix different handwriting styles—e.g., taking the structure from one and stroke style from another.

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">
  <img src="https://hackmd.io/_uploads/Bkj13sMakg.png" alt="StyleGAN diagram" width="400">
  <img src="https://hackmd.io/_uploads/H15YhghTJe.jpg" alt="Latent vector mixing" width="400">
</div>

To apply this to real handwriting, we approximated the latent vector of an input image using **MSE loss**, which proved more effective than perceptual loss for this kind of data.

This allowed us to embed and manipulate real handwriting styles for synthetic generation.

---

## Results

We successfully trained StyleGAN to generate handwriting that mimics a learned style from datasets like EMNIST. However, attempts to extract and recombine style vectors from real handwriting images yielded incoherent outputs, likely due to the complexity of the latent space. 

![Training Example](_projects/GANComputerVisionProject/GANGeneration.gif)



