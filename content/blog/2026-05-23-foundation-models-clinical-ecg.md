+++
title = "Foundation Models for ECG: Promise, Gaps, and What Comes Next"
date = "2026-05-23"
description = "Large pretrained models have transformed NLP and vision. Can the same paradigm work for clinical time-series like ECG? The short answer: partially — and the gaps are instructive."
tags = ["ECG", "foundation models", "clinical AI", "cardiology"]
+++

Large pretrained models have reshaped NLP and computer vision. The natural question is whether the same paradigm — pretrain on huge unlabeled corpora, fine-tune on small labeled datasets — translates to clinical time-series. For ECG specifically, the stakes are high: cardiac events kill more people than any other cause, and most of the world has no cardiologist nearby.

The short answer is: partially. And the gaps are instructive.

## Why ECG is a good testbed

ECG is well-suited for this experiment for a few reasons:

1. **Scale exists.** Datasets like MIMIC-IV, PhysioNet, and hospital systems have millions of recordings. This is enough to pretrain on.
2. **The signal is structured.** Unlike free-form clinical notes, an ECG is a fixed-length, fixed-channel time-series. Tokenization choices are more constrained.
3. **Labels are expensive but finite.** Cardiology has a well-defined label set (rhythm disorders, conduction abnormalities, ischemia markers). This makes downstream evaluation tractable.

## What works

Self-supervised pretraining on large ECG corpora using masked reconstruction (analogous to BERT's masked token prediction) does generalize. Models pretrained on one hospital's data and fine-tuned on another's outperform task-specific models trained from scratch — even with 50–100x fewer labeled examples in the target domain.

The representation quality is particularly good for rhythm-based tasks: atrial fibrillation, supraventricular tachycardia, and heart block. These have clear temporal patterns that contrastive or masked objectives capture well.

## Where it breaks down

**Morphology is harder.** Subtle ST-elevation or T-wave changes that indicate ischemia are sensitive to electrode placement, patient anatomy, and recording quality. A model trained on hospital A's 12-lead acquisitions can fail badly on hospital B's if their lead placement protocol differs by even a few centimeters. Pretraining helps but doesn't fully close this gap.

**Distribution shift is brutal.** ECG quality varies enormously: motion artifact, poor electrode contact, patient demographics. Pretraining on clean hospital recordings and deploying on a wearable patch is a different problem. The model has seen the domain but not the noise distribution.

**Labels from reports are noisy.** Most large ECG datasets are labeled by mining clinical reports, not by cardiologist review of each tracing. Label noise at scale creates a ceiling on what pretraining can learn. This is not a problem unique to ECG — it's the core challenge of clinical NLP in general.

## The real bottleneck: evaluation

The hardest problem isn't modeling — it's knowing when the model is good enough to trust. Cardiac AI failures are not like ImageNet failures. A false negative on an MI-equivalent pattern isn't just a wrong prediction; it's a patient sent home who shouldn't have been.

Current evaluation frameworks borrow too directly from ML benchmarks. We need:
- Prospective validation (not just held-out test sets from the same acquisition pipeline)
- Subgroup analysis by age, sex, comorbidities, and equipment type
- Calibration as a first-class metric, not an afterthought

## What I'm watching

A few directions that look promising:

- **Multi-modal pretraining** (ECG + clinical notes + vitals) is underexplored. The signal from combining modalities should be much stronger than ECG alone.
- **Personalised adaptation** — using a patient's own historical ECGs as context — could address the morphology variability problem. This is a retrieval problem as much as a modeling problem.
- **Uncertainty quantification** at inference time. If the model knows it doesn't know, that's clinically useful. Most deployed cardiac AI systems give you a score; almost none give you a calibrated confidence.

Foundation models for ECG aren't a solved problem, but they're a real one. The next few years will be determined less by architecture choices and more by whether the research community takes validation seriously enough.

---

*I work on ECG and medical imaging AI at Tricog Health. The views here are my own.*
