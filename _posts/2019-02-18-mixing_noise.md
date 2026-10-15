---
layout: post
title: Adding noise to audio data
date: 2019-02-18
description: Mixing white and real-world noise into audio clips at a chosen SNR, with examples you can listen to.
tags: [audio, machine-learning]
categories: [articles]
---

Some times we need to add noise to audio data. One examples is when we ealuate machine learning models 
which classify audio data. Performance of these models could degrade in noisy conditions. So we can add 
generated of other recorded noise to these original sounds and see how the models perform under these noisy conditions.

An article I wrote about this: 
[Medium article](https://medium.com/@lnw8px/adding-noise-to-audio-clips-5d8cee24ccb8)

Here I talk about adding two types of noises.
1. White noise
2. real world noises

The following are the signal and the real world noise I used to mix with it.

**signal ("leave my dog alone!")**

{% include audio.liquid path="assets/audio/mixing_noise/signal.wav" controls=true %}
\
**noise ("running water")**

{% include audio.liquid path="assets/audio/mixing_noise/noise.wav" controls=true %}


# Signal mixed with noise at various SNR
**SNR=30**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=30.wav" controls=true %}
\
**SNR=20**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=20.wav" controls=true %}
\
**SNR=15**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=15.wav" controls=true %}
\
**SNR=10**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=10.wav" controls=true %}
\
**SNR=5**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=5.wav" controls=true %}
\
**SNR=0**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=0.wav" controls=true %}
\
**SNR=-5**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=-5.wav" controls=true %}
\
**SNR=-10**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=-10.wav" controls=true %}
\
**SNR=-20**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=-20.wav" controls=true %}
\
**SNR=-30**

{% include audio.liquid path="assets/audio/mixing_noise/signal_noise_SNR=-30.wav" controls=true %}



We can see how the quality of the signal degrades with decreasing SNR.
