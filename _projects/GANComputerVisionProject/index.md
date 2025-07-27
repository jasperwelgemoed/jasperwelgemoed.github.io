---
layout: post
title: Recreating Handwriting using Generative Adverserial Networks (December 2024)
description: Developed a Computer Vision model using Generative Adversarial Networks (GANs) to recreate personalized handwriting styles, enabling the generation of handwritten text from style inputs using datasets like MNIST, NIST, and custom handwriting samples. 
skills:
  - AI
  - Computer Vision
main-image: /image.png
---

<a href="https://hackmd.io/@jasperwelgemoed/Bk8wh2Sjkx">
  <img src="/reporticon.png" alt="PDF" width="16" style="vertical-align: middle; margin-right: 4px;">
  Project Report
</a>

[Project Report](https://hackmd.io/@jasperwelgemoed/Bk8wh2Sjkx)
[Data Extraction Code](https://colab.research.google.com/drive/17uTkq5Dj2RDwaOkxgT3SqELEoRYLTl47?usp=sharing)

[Character one-hot encoding training](https://colab.research.google.com/drive/162i1A9PvQPkipWaMXpI4sF0svXwUVVTT?usp=sharing#scrollTo=vnuKfWN0p0dB)

[Character + Style one-hot encoding training](https://colab.research.google.com/drive/15fV1XdnsQa_IY56wmCsjLJg7ClByYHdi#scrollTo=Gq6eUDbOqNwp)

[StyleGAN + Style extraction](https://colab.research.google.com/drive/1ZnQXRduWscCrEuTa0UI-Qaxis3ZdfsGB?usp=sharing)

## Aim of the project

For this project me and my teammates aimed to explore whether Generative Adversarial Networks can be used to recreate and manipulate individual handwriting styles. We developed methods to condition GANs on one-hot encoded identity and character vectors, and trained StyleGANs to extract and reapply handwriting style features. We also created our own dataset of handwritten characters to test personal style replication. 

---

## Method

Our project explored two approaches to generating personalized handwriting using GANs.

### 1. One-Hot Conditioned GAN

We first trained a GAN conditioned on both the character (e.g., 'A', 'B', 'C') and the writer's identity, using one-hot encoded vectors. This setup allowed the model to separate **content** (what to write) from **style** (how it looks), enabling generation of letters in different personal handwriting styles.

### 2. Style Extraction with StyleGAN

We then used **StyleGAN**, a powerful GAN architecture that generates images progressively through multiple layers. It leverages a latent style vector to control high- and low-level features such as letter shape, stroke thickness, and curvature.

<img src="https://hackmd.io/_uploads/Bkj13sMakg.png" alt="StyleGAN diagram" width="400">

Using StyleGAN, we combined the latent vectors ($w$) of two samples to mix different handwriting styles—e.g., taking the structure from one and stroke style from another.

<img src="https://hackmd.io/_uploads/H15YhghTJe.jpg" alt="Latent vector mixing" width="400">

To apply this to real handwriting, we approximated the latent vector of an input image using **MSE loss**, which proved more effective than perceptual loss for this kind of data.

This allowed us to embed and manipulate real handwriting styles for synthetic generation.

---

## Results

We successfully trained StyleGAN to generate handwriting that mimics a learned style from datasets like EMNIST. However, attempts to extract and recombine style vectors from real handwriting images yielded incoherent outputs, likely due to the complexity of the latent space and dataset limitations. Despite these challenges, the project lays strong groundwork for future improvements using higher resolution images, StyleGAN2/3, and more robust datasets.




