---
layout: post
title: Recreating Handwriting using Generative Adverserial Networks
description: Developed a Computer Vision model using Generative Adversarial Networks (GANs) to recreate personalized handwriting styles, enabling the generation of handwritten text from style inputs using datasets like MNIST, NIST, and custom handwriting samples.
skills:
  - AI
  - Computer Vision
main-image: /image.png
---
Recreating Handwriting using Generative Adverserial Networks
===
By: Freek Müller, Jasper Welgemoed and Kasper Vaessen

Course: Computer Vision (DSAIT4125)

Date: 8th of April 2025

Group: 22

## Table of Contents

[TOC]

## Introduction
Handwriting is unique to every individual, it includes countless variations in style, stroke thickness, letter spacing, and curvature. For many, handwriting is not only used as a method of communication but also as a form of personal expression of identity. However, individuals with certain disabilities or motor impairments often face challenges in physically writing, making it more difficult or even impossible to express them selves the way they want.

Generative Adversarial Networks (GANs) offer a promising approach to overcoming these challenges by artificially recreating realistic handwriting styles. Unlike traditional methods that primarily focus on recognizing handwriting or digitizing physical strokes, GANs can generate entirely new handwritten characters and words based on learned styles. This allows individuals to produce handwriting that represent their own unique style or create their own style when handwriting has always been a problem for them. This would potentially bridge communication gaps; think about writing in different languages in your own handwritingstyle, and enhance personal connectivity.

This project investigates the capabilities of extracting handwriting styles through different methods. By directly learning the style by adding a one-hot encoding for each style or through GAN architectures, specifically the StyleGAN variant. StyleGAN is usefull for decyphering the stylistic attributes of handwriting from the content at different feature levels, enabling the generation of new text in a learned style. Our goal is to train a model capable of accurately reproducing handwritten characters, mimicking specific styles from diverse datasets including NIST, MNIST, and our custom-created dataset featuring our own handwriting.

## Background Research
Over time, recognizing handwriting and analyzing its style have improved greatly. At first, simple methods like template matching were used (Gader et al., 1991), but these methods weren't flexible enough for the many different styles people have. Later, handwriting could be digitized into images, which made it possible to work directly with static pictures of handwritten text (Plamondon & Srihari, 2000). This opened up new ways to use handwriting recognition for things like scanning documents and digital archives.

Later, statistical machine learning techniques such as Hidden Markov Models (HMMs) and Gaussian Mixture Models (GMMs) were introduced. These methods treated handwriting as sequences or patterns, significantly boosting recognition accuracy (Slimane et al., 2014).

The biggest improvement came with deep learning, especially Convolutional Neural Networks (CNNs). CNNs could automatically learn important features from images, leading to very accurate handwriting recognition. These methods performed extremely well on common benchmarks like the MNIST dataset (Chen et al., 2018). Recurrent Neural Networks (RNNs), particularly Long Short-Term Memory (LSTM) networks, further improved these systems by recognizing patterns over time, which made recognizing and predicting handwriting styles more effective (Paul et al., 2019).

Even with these advances, creating new handwriting samples rather than just recognizing existing ones remained difficult. This changed with Generative Adversarial Networks (GANs). GANs use two neural networks, a generator and a discriminator, which train together in a competitive manner. This made it possible to produce handwriting samples that closely resemble real handwriting (Davis et al., 2020) like in the figure below.
![image](https://hackmd.io/_uploads/B1zflf3akl.png)

However, standard GANs still lacked the ability to directly control style, making it hard to create handwriting that matches specific personal styles. This lack of control led to the creation of StyleGAN, which is designed to capture and control handwriting styles independently from the actual text. Our project builds on this, using StyleGAN to recreate and manipulate different handwriting styles from datasets like NIST, MNIST. The goal of our project is to re-create a StyleGAN like in the most recent paper of Davis et al. (2020), that can replicate various handwriting styles, based on an extracted input style. 

## Dataset
In this blogpost 3 datasets are used. A selection of [handwritten characters](https://www.kaggle.com/datasets/sachinpatel21/az-handwritten-alphabets-in-csv-format) in csv format taken from the NIST dataset ([National Institute of Standards and Technology, 2001](https://www.nist.gov/srd/nist-special-database-19)), the MNIST dataset ([Li Deng, 2012](https://ieeexplore.ieee.org/document/6296535)) and a self created dataset of our own handwriting.

### Characters from NIST
A dataset found on [Kaggle](https://www.kaggle.com/datasets/sachinpatel21/az-handwritten-alphabets-in-csv-format) was used as our initial choice to train on. It consists of characters extracted from the NIST dataset, put into a CSV format.

This dataset was chosen mainly because it contains seperate images of characters. This is much simpler to estimate than whole words/sentences, since connectivity between letters does not have to be considered. The dataset also contains labels of the letters, which allows a conditional GAN to be used.

### MNIST
MNIST was used during the the training of the StyleGAN because of the ease of use and ease of comparing to other methods, since the data is really similar (also extracted from NIST), this is expected to perform the same when letters are used.

### Own handwriting
To test whether it is possible for a GAN to directly learn a specific handwriting, as described in [method 1](#1.-Direct-GAN-Handwriting-Learning), a sufficiently size character separated dataset was needed with labels of the author. Since NIST, only contains one letter of the alpabet per author, this was not an option. This meant we had to create our own dataset. 

We did this by each writing a the alphabet down 18 to 36 times in a grid and scanning, splitting and binarizing the handwritten characters.

![Data Generation](https://hackmd.io/_uploads/BkO8JJXTJe.png)





## Method

### 1. Direct GAN Handwriting Learning 
As a first try to let a model learn specific handwriting styles, we wanted to see if we could directly train the model as to insert the features of a certain style in the form of a one-hot encoded vector appended to the current feature vector.

To help the model learn handwriting styles, we created a one-hot encoding system. Each image was labeled not only with the character it represented (like ‘A’, ‘B’, ‘C’...) but also with the identity of the writer. We concatenated these two one-hot encoded features onto the already existing 28x28 feature vector as the input of the generator. This way, the network could learn to associate certain styles with certain writers and characters.

The idea behind this is that if a GAN is conditioned on both the letter and the person’s identity, it can learn to generate the same letter in a specific style. The one-hot style label acts like a switch. This approach could work well because it clearly separates content (what to write) from style (how it looks), which is exactly what we want to control when recreating handwriting.

### 2. Style extraction
#### Style GAN
The architecture used to extract handwriting styles and generate new letters in a specific style is StyleGAN. This architecture can separate style from content when generating an image, making it ideal for handwriting synthesis. StyleGAN is widely used in the literature for generating handwriting with a specific style.

A StyleGAN is a type of Generative Adversarial Network (GAN), where a generator and a discriminator are trained together. The generator creates images, while the discriminator evaluates them, allowing both models to improve iteratively.

What sets StyleGAN apart is its layered training approach and its ability to control the style of generated images using a latent style vector. Unlike traditional GANs, StyleGAN generates an image progressively:

- The smallest layer first generates a low-resolution image.
- Each subsequent layer refines this image, adding more detail.
- The final resolution is determined by the last convolutional neural network (CNN) layer.

This hierarchical process ensures that fundamental image structures are generated first (e.g., overall shape and letter formation), followed by finer details such as stroke thickness and texture.

The style vector, which controls how an image is generated, is processed through a deep neural network. This network decodes elements that influence specific style aspects from the random noise vector "z", allowing precise control over different features in the generated handwriting.

![image](https://hackmd.io/_uploads/Bkj13sMakg.png)


#### Extracting $\mathcal{W}$-Space from unseen data
With the StyleGAN, styles can be combined by using the $w$ vector of one generated image for the first couple of layers and another $w$ vector for the other layers. Which means The high level features (character type, size, cursiveness) will be taken from one image, while the low level features (local curvature, stroke thickness, variations in pressure, etc) will be taken from another image. This way we could extract the style from an image and apply it to other letters. 

An example:

![image_combination](https://hackmd.io/_uploads/H15YhghTJe.jpg)

However, it is not trivial to extract the $w$ vector from a non-generated image, since the network is not backwards traversable and it is not guaranteed that a given can be generated exactly by the StyleGAN. This means we have to approximate the $\mathcal{W}$-space. Luckily, this has previously been done by [R.Abdal et. al](https://arxiv.org/abs/1904.03189).

They propose to extend the $\mathcal{W}$ space such that each layer of the synthesis network has its own $w$ vector. They call this collection of vectors the $\mathcal{W}^+$ space. They initialize a $w \in \mathcal{W}^+$ and optimize it using gradient descent and a combination of perceptual loss and mean squared error (MSE) loss. 

While perceptual loss is great to prevent weird unnatural artifacts for people, animals, or real life objects, it is not great for handwriting. This is because of two reasons:

- Perceptual loss models are often not trained on text image data.
- Small disturbances in human faces are quickly interpeted as 'eerie' by humans [(M. Masahiro)](https://ieeexplore.ieee.org/document/6213238). This is not so much a problem for handwritten text.

Because of these reasons it was chosen to only use MSE-loss for our method.



## Experiments and Results
To test our methods, we performed multiple experiments. These are described below. They are mostly visually evaluated, since objective metrics on style transfer often fail to capture subjective aspects such as aesthetic quality, coherence, or artistic intent. We therefore rely on qualitative comparisons and human judgment to assess how well the transferred styles align with the desired outcomes.

### Direct GAN Handwriting Learning 
We started testing this idea using the MNIST dataset. In this first test, we used a GAN conditioned only on a one-hot encoded vector representing the digit (0–9). After training, the generator was able to produce distinct digits based on the input vector. The results were promising, showing that the model learned the connection between the one-hot label and the digit it had to generate. The code can be found in [Character one-hot encoding training](https://colab.research.google.com/drive/162i1A9PvQPkipWaMXpI4sF0svXwUVVTT?usp=sharing#scrollTo=vnuKfWN0p0dB).

![image](https://hackmd.io/_uploads/HkvgFGhTJl.png)

Next, we extended this idea by also adding a one-hot encoded style vector to the input, trying to teach the model to generate digits not only by class but also in different handwriting styles. We trained the model on our small dataset using a pre-trained generator from the MNIST model. However, this time the results were less successful. Even though we could recognize some letters in different styles, we couldn't clearly recognize individual writing styles. Most generated samples looked very similar, likely because the model was still heavily influenced by the large MNIST dataset it was pre-trained on. Our own data was too small in comparison, so it had little impact—this likely caused the model to overfit to the pre-training data.

![image](https://hackmd.io/_uploads/SkfKFGn6yl.png)

Finally, we tried training a model from scratch using only our own handwritten dataset. Unfortunately, the results were poor. The generated letters were noisy as can be seen in the figure below, inconsistent, and often not recognizable. We believe this happened due to the small size of our dataset. GANs usually require a large number of training samples to generalize well, and with limited data per character and style, the model simply didn’t have enough information to learn effectively. This code can be found in [Character + Style one-hot encoding training](https://colab.research.google.com/drive/15fV1XdnsQa_IY56wmCsjLJg7ClByYHdi#scrollTo=Gq6eUDbOqNwp).
![image](https://hackmd.io/_uploads/rkbssfha1x.png)

### Style extraction
The initial experiment was performed with an image size of 16x16, using the $\mathcal{W^+}$, with $\mathcal{W^+} = \mathbb{R}^{6 \times 256}$ (vector of size 256 for 6 total layers). Because of the architecture of the model, the amount of layers scales with the resolution of the image, so this is not a parameter which can be adjusted.

This was able to almost identically reconstruct the original image. 


Original             |  reconstructed
:-------------------------:|:-------------------------:
![image](https://hackmd.io/_uploads/HklDlz26kx.png =70%x) | ![image](https://hackmd.io/_uploads/SyR5gMnpJe.png =70%x)

However, when trying to combine two estimated $w$'s and using this to generate a new image, the output is non-sensical for all crossover points:

![image](https://hackmd.io/_uploads/SJQTZz36Jg.png =40%x)

Most likely, this is because the $\mathcal{W^+}$ space is too big compared to the image size. I.e. the model learns to 'copy' the image instead of encoding higher level style features of the image.

Some experiments were performed to combat this and to get the best understanding of how the network behaves. We tested the impact of the used image resolution, the difference between estimating a $w$ from the $\mathcal{W^+}$ versus the $\mathcal{W}$ space and the impact of the size of the $\mathcal{W}/\mathcal{W^+}$ space.

#### Image size
A way to combat the big difference between the embedding space and the image size, is to increase the image size.

We performed the same experiment, but then with a image size of 32x32, which is the max size for the dataset. The method is still able to replicate the original image with a low loss (0.0001 MSE). However, the output when combining two estimated $w$'s is still non sensical:

![image](https://hackmd.io/_uploads/B1rrqdn6yg.png =40%x)

This tells us that a bigger image size is not enough to solve the problem.

#### $\mathcal{W}$ vs $\mathcal{W^+}$ space
The second suggested way is to estimate a $w \in \mathcal{W}$ instead of $w \in \mathcal{W}^+$. This reduces the embedding space size quite a bit, potentially forcing the optimization to learn more distinct features from the letters.

The optimization was again able to closely estimate the original image. 

Original             |  reconstructed
:-------------------------:|:-------------------------:
![image](https://hackmd.io/_uploads/ByE64b6p1l.png =70%x)| ![image](https://hackmd.io/_uploads/BkzqNbTTyx.png =70%x)


However, the output when combining two estimated $w$'s is still is a similar nonsensical image.

#### Varying $\mathcal{W}/\mathcal{W^+}$ size
The last experiment we perform is changing the size of the $\mathcal{W}$ space and with that also the size of the $\mathcal{W}^+$ space. We tried sizes of 256 (original), 128 and 64.

Below the reconstructions for different combinations of embedding size and $\mathcal{W}$/$\mathcal{W}^+$ space can be found.



|                 | 256  | 128  | 64   |
| --------------- | ---- | ---- | ---- |
| $\mathcal{W}^+$ | ![image](https://hackmd.io/_uploads/BJBnMXp6Jg.png) | ![image](https://hackmd.io/_uploads/B1n_eQap1e.png) | ![image](https://hackmd.io/_uploads/H1KLez66kg.png) |
| $\mathcal{W}$   | ![image](https://hackmd.io/_uploads/Byr0-mTp1g.png) | ![image](https://hackmd.io/_uploads/By4oyXT6Jg.png) |![image](https://hackmd.io/_uploads/HyPvJzpayl.png)|

For most of the combination the imaghe could be reconstructed almost identically. However recombining the $w$'s again results in a nonsensical image.

None of the experiments show improvements to the result. The difference between our results and the paper by [R.Abdal et. al](https://arxiv.org/abs/1904.03189) on which this method was based, could be explain by two things. 

The original paper uses StyleGAN2 instead of StyleGAN. This architecture is slightly better at extracting styles than the original styleGAN used by us. 
It could also be that the size of our data, specifically the image size, is too small to learn any low level details.
Also a reason for the difference in performance could be that the $w$ extraction from real images estimates a $w$ that should not be able to be created because the mapping network will never return this $w$. This could cause combinations of two of these $w$'s to return nonsensical results.
Lastly, it could also be that there is a high proximity between the different letter encodings and style encodings in latent space. Which makes the estimation really sensitive to small pertubations.


## Conclusion
In this post we have trained a StyleGAN to generate handwriting and decoded the styles of the handwriting using the StyleGAN so that new handwriting in a synthesised style can be created. The trained StyleGAN was able to generate letters consistent with the EMNIST dataset style, but when extracting the style from real images of letters and generating new letters with that style the method failed. We have various causes we think could be the reason for the failure of the generation. Such as the possible high proximity to the different letter encodings and style encodings in latent space, the possible estimation of infeasible $w$'s, the high embedding space and the lack usage of StyleGAN 1 instead of StyleGAN 2. Further experiments will have to be performed to verify these theories.

## Further Research
While we have shown that it is theoretically possible to copy a handwriting using a (style)GAN, there are still several limitations with our proposed method.

A big oppertunity still lies in combining extracted styles vectors to create images of letters in the style of the real image. There are a couple of research possibilities that show promise:

- Using [StyleGAN2](https://arxiv.org/abs/1912.04958v2) or [StyleGAN3](https://nvlabs.github.io/stylegan3/)
- Experimenting with bigger images resolutions.
- Tune more parameters of the model.

Besides improvements to the StyleGAN, the conditional GAN can also be improved. The most obvious improvements are:

- Creating and using a bigger dataset.
Both in terms of images per handwriting and the total amount of handwritings.
- Improving the image extraction from the paper scans.
This can include thickening the lines and dynamically adjusting the threshold for binarization.

Lastly, it would also be interesting to see if our method would work on words or sentences instead of characters. This could increase the realism when used 

## Links to our code
[Data Extraction Code](https://colab.research.google.com/drive/17uTkq5Dj2RDwaOkxgT3SqELEoRYLTl47?usp=sharing)

[Character one-hot encoding training](https://colab.research.google.com/drive/162i1A9PvQPkipWaMXpI4sF0svXwUVVTT?usp=sharing#scrollTo=vnuKfWN0p0dB)

[Character + Style one-hot encoding training](https://colab.research.google.com/drive/15fV1XdnsQa_IY56wmCsjLJg7ClByYHdi#scrollTo=Gq6eUDbOqNwp)

[StyleGAN + Style extraction](https://colab.research.google.com/drive/1ZnQXRduWscCrEuTa0UI-Qaxis3ZdfsGB?usp=sharing)

## References
Davis, B., Tensmeyer, C., Price, B., Wigington, C., Morse, B., & Jain, R. (2020). Text and Style Conditioned GAN for Generation of Offline Handwriting Lines. arXiv (Cornell University). https://doi.org/10.48550/arxiv.2009.00678

Flanagan, P. A. (2016). NIST Handprinted Forms and Characters - NIST Special Database 19 [Data set]. http://doi.org/10.18434/T4H01C

Deng, L. (2012). The mnist database of handwritten digit images for machine learning research. IEEE Signal Processing Magazine, 29(6), 141–142.

Abdal, Rameen, e.a. Image2StyleGAN: How to Embed Images Into the StyleGAN Latent Space? arXiv:1904.03189, arXiv, 3 september 2019. arXiv.org, https://doi.org/10.48550/arXiv.1904.03189.

Mori, Masahiro & MacDorman, Karl & Kageki, Norri. (2012). The Uncanny Valley [From the Field]. IEEE Robotics & Automation Magazine. 19. 98-100. 10.1109/MRA.2012.2192811. 

Gader, P., Forester, B., Ganzberger, M., Gillies, A., Mitchell, B., Whalen, M., & Yocum, T. (1991). Recognition of handwritten digits using template and model matching. Pattern Recognition, 24(5), 421–431. https://doi.org/10.1016/0031-3203(91)90055-a

Plamondon, R., & Srihari, S. (2000). Online and off-line handwriting recognition: a comprehensive survey. IEEE Transactions On Pattern Analysis And Machine Intelligence, 22(1), 63–84. https://doi.org/10.1109/34.824821

Slimane, F., Schaßan, T., & Märgner, V. (2014). GMM-based Handwriting Style Identification System for Historical Documents. infoscience.epfl.ch. https://infoscience.epfl.ch/handle/20.500.14299/105873

Chen, F., Chen, N., Mao, H., & Hu, H. (2018). Assessing four Neural Networks on Handwritten Digit Recognition Dataset (MNIST). arXiv (Cornell University). https://doi.org/10.48550/arxiv.1811.08278

Paul, I. J. L., Sasirekha, S., Vishnu, D. R., & Surya, K. (2019). Recognition of handwritten text using long short term memory (LSTM) recurrent neural network (RNN). AIP Conference Proceedings, 2096, 030011. https://doi.org/10.1063/1.5097522

Davis, B., Tensmeyer, C., Price, B., Wigington, C., Morse, B., & Jain, R. (2020). Text and Style Conditioned GAN for Generation of Offline Handwriting Lines. arXiv (Cornell University). https://doi.org/10.48550/arxiv.2009.00678

Karras, T., Laine, S., Aittala, M., Hellsten, J., Lehtinen, J., Aila, T. (2019). Analyzing and Improving the Image Quality of StyleGAN. arXiv preprint arXiv:1912.04958.

Karras, T., Aittala, M., Laine, S., Härkönen, E., Hellsten, J., Lehtinen, J., & Aila, T. (2021). Alias-Free Generative Adversarial Networks. Proc. NeurIPS.


