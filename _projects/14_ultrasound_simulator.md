---
layout: page
title: Hand-tracked ultrasound simulator
description: Early work on a fetal ultrasound simulator that slices a 3D mesh where a Leap Motion–tracked hand points.
img: assets/img/ultrasound_sim/collage.jpg
importance: 8
category: research
---

<img src="{{ '/assets/img/ultrasound_sim/collage.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Leap Motion hand skeleton, a 3D fetus mesh in MATLAB, and a 2D cross-section of the mesh">

<div class="caption">From left: hand tracking from the Leap Motion controller, the fetus mesh in MATLAB, and a cross-section of the mesh, which is what the simulated ultrasound shows.</div>

The goal was an ultrasound simulator for training: as the user moves their hand like an ultrasound probe, the screen
shows the matching cross-section of a fetus, the way a real scan would.

- **Hand tracking.** A Leap Motion controller uses two infrared cameras and three infrared LEDs to track up to two
  hands, including fingers and joints, within about 2 ft of the device. I read it from MATLAB through the Leap Motion
  SDK (v2.3.1).
- **Anatomy.** A fetus mesh with about 79,000 points stands in for the patient, loaded from an STL file. A heart mesh
  was also used.
- **Slicing.** A plane is placed through the mesh, and the points where it crosses the mesh's triangles make up the 2D
  cross-section on screen.

The first version took about 1.5 s per update. Replacing the exact plane–triangle intersection with a simple distance
threshold (keep points close to the plane) brought that to about 1.0 s. That is still too slow for a real-time display,
so the next step in the report is a faster slicing algorithm.

- Report: [Leap Motion ultrasound simulator progress report (PDF)]({{ '/assets/pdf/leap_motion_ultrasound_simulator.pdf' | relative_url }})
