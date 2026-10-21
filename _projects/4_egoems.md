---
layout: page
title: EgoEMS
description: A high-fidelity multimodal egocentric dataset for cognitive assistance in emergency medical services. AAAI 2026.
importance: 5
category: research
---

EgoEMS is a multimodal egocentric dataset of simulated emergency medical scenarios, built to support the development of
AI assistants for first responders. My contributions:

- **Ground-truth CPR sensing.** I built an Arduino-based time-of-flight depth sensor into a CPR manikin to capture
  ground-truth compression depth and rate.
- **Data collection.** I recruited participants and collected first- and third-person video, depth video, audio,
  smartwatch motion data and ground-truth measurements.
- **CPR quality from a smartwatch.** I developed LSTM models that estimate CPR quality from smartwatch motion data.
- **CPR quality from video.** I developed a vision-based method that estimates compression depth and rate using Google
  MediaPipe hand tracking and careful camera calibration.

The study found that combining video and smartwatch data gave the best overall CPR feedback: smartwatch data alone
estimated compression rate most accurately, while the combination estimated depth most accurately.

- Project website: [EgoEMS](https://uva-dsa.github.io/EgoEMS/)
- Paper: [EgoEMS: A High-Fidelity Multimodal Egocentric Dataset for Cognitive Assistance in Emergency Medical Services](https://doi.org/10.1609/aaai.v40i46.41293) (AAAI 2026, [arXiv](https://arxiv.org/abs/2511.09894))
