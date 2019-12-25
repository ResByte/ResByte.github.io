---
title: 'Do Imagenet Models transfer well across datasets?'
date: 2019-12-25
permalink: /posts/2019/12/Simon-imagenet-transfer/
tags:
  - Deep Learning
  - Transfer Learning
  - Computer Vision
---

arxiv url : http://arxiv.org/abs/1805.08974 

A lot of work in computer vision today depends on general understanding that if the model is pre-trained on a large dataset like ImageNet, it will perform better on vision tasks like image classification,  object detection, segmentation etc. 

This work from Simon et al. 2018 focuses on solving this puzzle of verifying and evaluating if transfer learning from imagenet works well for different dataset. 

## Pre-trained Model Usability

Pre-trained models can be used in following two settings:

- Fixed-feature extractor: Model is initialized with parameters from model trained over imagenet dataset. Most of the layers of model are frozen and final few layers are trained on the new dataset. 
- Fine-tuned: Similar to previous, model parameters are initilized by parameters values trained over imagenet. Instead of freezing a layer, all parameters are again updated for new dataset. 

Another way to initialize model parameter is to choose the values randomly. Then update the parameter values according to new dataset. 

## Dataset and Model Architectures Used:

Following are the various datasets used to test transfer learning : 

- Food101
- Cifar-10
- Cifar-100
- Birdsnap
- SUN397
- Stanford Cars 
- FGVC Aircraft 
- Pascal VOC 2007 Cls.
- Describable Textures (DTD) 
- Oxford-IIIT Pets 
- Caltech-101
- Oxford 102 Flowers

The variability of dataset is in terms of number of sample per category, object types, image size etc. 

Network Architectures used are : 

- Inception
- BN-Inception
- Resnet (50, 101, 152)
- Densenet (121, 169, 201)
- Mobilenet v1,v2
- NASNet-A mobile 
- NASNet-A Large

## Task Settings

For each of the network and dataset, these 3 tasks were tested : 

- learn a `logistic regression` model based on penultimate layer features using imagenet pretrained model 
- initialize layer weights from Imagenet pretrained model and `fine-tune` all.
- From `random initialization` of model weights, train it over new dataset.

## Conclusion 

Final conclusions from Simon et al. 

- Performance of models on imagenet dataset can provide better features in final layers that leads to better classifiers if these features are used. 
- In case of Fixed-feature extractor, the use of regularizor on models during imagenet training will have relatively poor performance when trained on 
- For large datasets using model with random initialization, the performance on new dataset is similar to model's performance on Imagenet. Even though pre-trained weights are not used. 