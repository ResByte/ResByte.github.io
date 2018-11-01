---
title: 'Notes: DeepLab Segmentation'
date: 2017-02-15
permalink: /posts/2017/02/deeplab-segment/
tags:
  - Deep Learning
  - Segmentation
  - CRF
  - Atrous conv
---

This post is a summary of Segmentation paper by Chen et al. 2016. They combine CRFs to generate a more accurate segmentation results. 

Paper : [Chen et al.](https://arxiv.org/abs/1606.00915)


## Challenges
There are 3 challenges in segmentation using Deep CNNs
- reduced feature resolutions due to presence of multiple max-pooling and striding(downsampling). Feature maps resulting from this have reduced feature resolutions. [Solution]: instead of downsampling, use Upsampling to increase resolution in last few layers. (Atrous Convolution + bilinear interpolation) to recover original image resolution.  
- multiple scales objects present in image. [Solution]: Using Atrous spatial pyramid pooling, multiple filters are used to collect complementary field of view as a result capture multiple scales, including context and object.
- presence of DCNN invariance reduces localization accuracy. [Solution]: use of fully connected CRF to assimilate fine details.

## Advantages
Finally, some advantages of DeepLab
- high speed of 8 FPS.
- state-of-art accuracy.
- simple structure.     

## Related Works
DCNN based segmentation have following major categories
- cascade of bottom-up segmentation with DCNN based region segmentation[ [7](https://arxiv.org/abs/1311.2524), [49](https://arxiv.org/abs/1407.1808), [50](https://arxiv.org/abs/1412.0774)]
- Coupling a convolutionally computed DCNN for image labelling and an independent segmentation. [[39](http://yann.lecun.com/exdb/publis/pdf/farabet-pami-13.pdf), [21](https://arxiv.org/abs/1411.5752), [51](https://arxiv.org/abs/1412.1283)]
- segmentation free approach by computing dense category-level pixel labels.[[14](https://arxiv.org/abs/1605.06211), [52](https://arxiv.org/abs/1411.4734)]

Works using CRF with DCNN
- [Efficient inference in fully connected crfs with gaussian edge potentials](https://papers.nips.cc/paper/4296-efficient-inference-in-fully-connected-crfs-with-gaussian-edge-potentials.pdf)
- [Combining the best of graphical models and convnets for semantic segmentation](https://arxiv.org/abs/1412.4313)
- [Learning hierarchical features for scene labeling](http://yann.lecun.com/exdb/publis/pdf/farabet-pami-13.pdf)
- [Material recognition in the wild with the materials in context database](https://arxiv.org/abs/1412.0623)

## Advances in Segmentation
End-to-End training for structured prediction
- [[40](https://arxiv.org/abs/1504.01013), [59](https://arxiv.org/abs/1502.03240), [62](https://arxiv.org/abs/1509.02634), [64](https://arxiv.org/abs/1407.2538), [65](https://arxiv.org/abs/1503.02351)] proposed methods for joint learning of DCNN and CRFs.

Weaker Supervision based methods
- [[58](https://arxiv.org/abs/1502.02734), [69](https://arxiv.org/abs/1411.6228), [70](https://arxiv.org/abs/1506.03648), [71](https://arxiv.org/abs/1506.04924)] relaxes assumption of availability of pixel-level semantic annotations for training whole set.

## Model Description
![Model Description](/images/deeplab_img1.png)
