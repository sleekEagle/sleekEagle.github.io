---
layout: page
title: Camera-independent depth from defocus
description: Single-image depth estimation from defocus blur that keeps working when you change the camera. WACV 2024.
importance: 1
category: research
github: https://github.com/sleekEagle/defocus_camind
---

Monocular depth-from-defocus models learn how blur relates to depth, but that relationship depends on the camera
(focal length, aperture, sensor). A model trained on one camera quietly breaks on another.

Using the optics of defocus blur, I showed how camera parameters change the blur, and built a correction that lets a
trained model generalize to new cameras **without retraining**. I also built a synthetic dataset to test camera
independence, and evaluated on it as well as DDFF12 and NYU Depth V2 captured with different cameras.

- Paper: [Camera-Independent Single Image Depth Estimation from Defocus Blur (WACV 2024)](https://openaccess.thecvf.com/content/WACV2024/html/Wijayasingha_Camera-Independent_Single_Image_Depth_Estimation_From_Defocus_Blur_WACV_2024_paper.html)
- Code: [sleekEagle/defocus_camind](https://github.com/sleekEagle/defocus_camind)
