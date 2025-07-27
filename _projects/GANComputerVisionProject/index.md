---
layout: post
title: Recreating Handwriting using Generative Adverserial Networks
description: Developed a Computer Vision model using Generative Adversarial Networks (GANs) to recreate personalized handwriting styles, enabling the generation of handwritten text from style inputs using datasets like MNIST, NIST, and custom handwriting samples. 
skills:
  - AI
  - Computer Vision
main-image: /image.png
Date: 2025
---

[Project Report](https://hackmd.io/@jasperwelgemoed/Bk8wh2Sjkx)

## Aim of the project

For this project me and my teammates aimed to explore whether Generative Adversarial Networks can be used to recreate and manipulate individual handwriting styles. We developed methods to condition GANs on one-hot encoded identity and character vectors, and trained StyleGANs to extract and reapply handwriting style features. We also created our own dataset of handwritten characters to test personal style replication. 


---

## Results

We successfully trained StyleGAN to generate handwriting that mimics a learned style from datasets like EMNIST. However, attempts to extract and recombine style vectors from real handwriting images yielded incoherent outputs, likely due to the complexity of the latent space and dataset limitations. Despite these challenges, the project lays strong groundwork for future improvements using higher resolution images, StyleGAN2/3, and more robust datasets.




