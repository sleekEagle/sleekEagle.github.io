---
layout: post
title: Adding noise to audio data
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

[signal ("leave my dog alone!")](/audio/mixing_noise/signal.wav)\\
[noise ("running water")](/audio/mixing_noise/noise.wav)

# Signal mized with noise at various SNR
[SNR=15](/audio/mixing_noise/signal_noise_SNR=30.wav)\\
[SNR=10](/audio/mixing_noise/signal_noise_SNR=20.wav)\\
[SNR=7.5](/audio/mixing_noise/signal_noise_SNR=15.wav)\\
[SNR=5](/audio/mixing_noise/signal_noise_SNR=10.wav)\\
[SNR=2.5](/audio/mixing_noise/signal_noise_SNR=5.wav)\\
[SNR=0](/audio/mixing_noise/signal_noise_SNR=0.wav)\\
[SNR=-2.5](/audio/mixing_noise/signal_noise_SNR=-5.wav)\\
[SNR=-5](/audio/mixing_noise/signal_noise_SNR=-10.wav)\\
[SNR=-10](/audio/mixing_noise/signal_noise_SNR=-20.wav)\\
[SNR=-15](/audio/mixing_noise/signal_noise_SNR=-30.wav)


We can see how the quality of the signal degrades with decreasing SNR.
ppp




