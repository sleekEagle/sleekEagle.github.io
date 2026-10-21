---
layout: page
title: Depth from defocus on real camera blur
description: Taking camera-independent depth from defocus from synthetic blur to real lens blur on the MODEST dataset, and what makes it hard. 2026.
img: assets/img/modest_dfd/blur_maps.jpg
importance: 2
category: research
github: https://github.com/sleekEagle/RealCameraIndependent
---

<img src="{{ '/assets/img/modest_dfd/blur_maps.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Two MODEST images at F9 and F2.8, their label blur maps, and the blur maps predicted by the network">

<div class="caption">Real MODEST images at F9 (top) and F2.8 (bottom). Middle: blur computed from stereo depth. Right: the blur the network reads from the image alone, after training on whole images.</div>

My [earlier depth-from-defocus work]({% link _projects/1_depth_from_defocus.md %}) made a model camera-independent:
it predicts depth from blur, divides out how much blur the camera settings produce, and so works on cameras it was not
trained on. That work, like most depth from defocus, used **synthetically blurred** images. Here I tried the same idea
on **real lens blur**, using MODEST, a dataset of real photos of 9 indoor scenes taken at 10 focal lengths (28–70 mm)
and 5 F-numbers (F2.8–F22). To my knowledge, this is the first attempt at camera-independent depth from defocus on real
optical blur.

**Setup.** The network (Camind) has two stages. Stage 1 reads the blur at each pixel. The camera correction removes the
camera's base blur and divides by how much blur the current focal length and F-number produce, which gives a
camera-free quantity. Stage 2 turns that into depth. I compared it with the same network without camera information,
and with large pretrained models (Depth Anything 3 and UniDepth), zero-shot and fine-tuned on MODEST. Each test split
held out one F-number and two focal lengths.

### Getting the network to see real blur at all

At first, stage 1 learned nothing: its blur output was nearly constant. Working out why took most of the project.

- **A dropout bug.** Dropout placed right before max pooling made training and evaluation disagree, inflating the depth
  scale by 24% at eval time. It was a real bug, but not the main problem. (I wrote it up separately:
  [Dropout right before max pooling]({% post_url 2026-10-15-dropout_maxpool %}).)
- **Crops with one blur level.** Real blur is mostly small. At F16–F22, about 90% of pixels have less blur than the
  camera's own base blur. Worse, within a 518 px training crop the largest blur was typically only 1.1–1.2× the
  smallest, so the network never saw sharp and blurred parts of the same scene side by side. A synthetic test confirmed
  this is fatal: with one blur level per crop, learning failed in 3 of 3 seeds.

<img src="{{ '/assets/img/modest_dfd/fig_synth.png' | relative_url }}" class="img-fluid" alt="Synthetic blur diagnostic: learning works with a wide blur range, survives camera processing, fails with one blur per crop, and is unreliable with only small blurs">

**The fix was to train on whole images** resized to 1036 px. Each image then contains near and far parts with very
different blur. Stage 1 learned to read blur well (rank correlation 0.92 with the label blur), including on a scene it
never saw in training (0.88). So MODEST does contain enough blur to learn from. The problem was how the images were cut.

### Results

<img src="{{ '/assets/img/modest_dfd/fig_main.png' | relative_url }}" class="img-fluid" alt="Test AbsRel for each model and group of seen and unseen camera settings, on two splits">

- **Whole images cut the error substantially**, from 0.32–0.35 AbsRel to 0.18–0.20.
- **The camera correction helps where it should.** On unseen focal lengths, error dropped by 13–17% (0.250 → 0.208 and
  0.238 → 0.207). At wide apertures, where blur carries the most depth information, it dropped by up to 21% (F2.8) and
  16% (F5).
- **It does not transfer to unseen F-numbers.** With F22 held out it was 16% worse than the uncorrected model. With F2.8
  held out it only tied, holding up at 2–4 m but failing close up and far away.
- **At 2–4 m and wide apertures**, the corrected model roughly halved the error of zero-shot Depth Anything 3 (0.10–0.14
  vs. 0.23–0.26).
- **It stays far behind the fine-tuned foundation models**, which reach 0.05–0.06 AbsRel overall.

<img src="{{ '/assets/img/modest_dfd/fig_by_fnumber.png' | relative_url }}" class="img-fluid" alt="Error per F-number: the camera correction helps at wide apertures and hurts at narrow ones, especially on the held-out F-number">

### Conclusion

**Building a depth-from-defocus model on real camera blur turned out to be hard,** much harder than with synthetic blur.
The reasons are mostly in the physics of real photos, not the network:

1. **Real blur is a weak signal.** At narrow apertures, defocus blur is about as small as the camera's base blur and
   its motion blur, so there is little depth information to read. There is also none near the focus plane or on
   textureless surfaces, and blur alone cannot tell in front of the focus plane from behind it.
2. **How the data is presented matters as much as the data.** Standard random crops removed the contrast between sharp
   and blurred regions that the network needs. The same data, shown as whole images, worked.
3. **The camera correction only transfers within the blur range seen in training.** Focal lengths are fine. A new
   F-number produces blur sizes stage 1 has never seen, and the simple blur model drifts per F-number (diffraction,
   motion blur, the lens's real aperture). That breaks the "divide out the camera" assumption.
4. **A small network trained from scratch on 9 scenes cannot match foundation models** that already know scene shape.
   On MODEST, blur is better seen as an extra cue than as a replacement for them.

The most promising next step is to give the camera-corrected blur map to a pretrained model as extra input channels,
so that blur refines a model that already understands scenes. Other next steps are finishing the four fixes for
unseen F-numbers (stopped partway through training), checking the blur model separately for each F-number, and running
more splits and seeds. Every main result here comes from a single training run.

- Code: [sleekEagle/RealCameraIndependent](https://github.com/sleekEagle/RealCameraIndependent)
