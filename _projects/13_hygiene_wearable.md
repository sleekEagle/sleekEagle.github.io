---
layout: page
title: Smartwatch hygiene monitoring
description: Recognising hand washing and tooth brushing from a smartwatch to support people with developmental disabilities. IEEE Wireless Health 2016.
img: assets/img/hygiene_wearable/collage.jpg
importance: 8
category: research
---

<img src="{{ '/assets/img/hygiene_wearable/collage.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" alt="The nine WHO hand-washing motions, the smartwatch with its sensor axes, the 18 mouth regions used for brushing, and the data-collection app">

<div class="caption">From left: the hand-washing motions (W01–W09), the smartwatch and its sensor axes, the 18 mouth regions (B01–B18), and the phone app used to collect data.</div>

Many people with developmental disabilities struggle to keep up personal hygiene. Poor memory or attention makes it
hard to wash hands or brush teeth properly, and poor hygiene leads to infections. Carers mostly keep track by watching
and logging, which is expensive and hard to scale. With Benny Lo, I built a framework that uses an ordinary smartwatch
(an LG G Watch W100) to recognise these activities and measure how well they're done, so users and carers can get
feedback.

**How it works.** The watch records its accelerometer and gyroscope at about 150 Hz and audio at 8 kHz. Classification
runs in stages, so the watch only does heavy work when it has to:

1. **Is it hygiene?** Six simple accelerometer features (per-axis mean and standard deviation) and a Naïve Bayes
   classifier separate hand washing and tooth brushing from everything else, with **98.1%** accuracy. This stage runs
   all the time, so it was kept cheap enough for a battery-powered watch.
2. **Washing or brushing?** Audio features such as spectral flux, zero crossings and MFCCs tell the two apart from
   other activities with **95.4%** accuracy. Audio also catches someone miming hand washing without any water.
3. **Which motion or region?** Motion features, including per-axis energy and the correlations between sensor axes,
   identify which hand-washing motion is being done (following the WHO guidelines) or which part of the mouth is being
   brushed. Accuracy was 96% for 3 groups of washing motions and 72% for 2 groups of mouth regions. At the finest
   level it was 84.0 ± 10.6% for individual washing motions and 67.9 ± 20.4% for brushing regions.

This was a preliminary study with 7 healthy adult volunteers in a controlled setting. Next steps were more participants
(including people with developmental disabilities), checking left-handed users, and turning the detections into
feedback, such as a reminder to brush the lower right teeth more.

- Demo video: [smartwatch hygiene monitoring on YouTube](https://www.youtube.com/watch?v=v-B4TnfMe4s)
- Paper: [A wearable sensing framework for improving personal and oral hygiene for people with developmental disabilities](https://doi.org/10.1109/WH.2016.7764550) (IEEE Wireless Health 2016), also listed on the [publications page]({{ '/publications/' | relative_url }})
