---
layout: page
title: Multimodal echocardiogram classification
description: Fusing echocardiogram video with clinical records to detect Takotsubo cardiomyopathy.
importance: 3
category: research
---

At the University of Iowa I labelled a dataset and built a spatio-temporal multimodal deep learning framework that
combines echocardiogram video with structured clinical data to improve detection of Takotsubo cardiomyopathy.

The interesting part was making two very different modalities cooperate in one end-to-end model, and keeping training
honest on a highly imbalanced dataset. Training ran on an SGE-managed GPU cluster with PyTorch and CUDA.
