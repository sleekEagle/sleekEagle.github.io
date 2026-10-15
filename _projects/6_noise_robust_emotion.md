---
layout: page
title: Noise-robust speech emotion recognition
description: CNNs and attention that keep recognising emotion from speech when the audio gets noisy. Smart Health 2021.
importance: 6
category: research
---

Most speech emotion models are trained on clean audio and fall apart in real environments. I trained CNN-based models
on clean audio and on audio mixed with ten noise types at several signal-to-noise ratios. Combining magnitude and
modified group delay spectrograms, adding synthetic noise in training, and adding attention each made the models
noticeably more robust.

Related writing on this site:

- [Adding noise to audio data]({{ '/blog/2019/mixing_noise/' | relative_url }})
- [Classifying emotions with neural networks]({{ '/blog/2019/classify_emotions/' | relative_url }})
- [Hiding emotions in plain sight]({{ '/blog/2019/hide_emotions/' | relative_url }})
