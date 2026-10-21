---
layout: page
title: Driver fatigue detection system
description: A low-cost PIC microcontroller system that spots drowsy driving from steering wheel motion. 2013.
img: assets/img/driver_fatigue/circuit.jpg
importance: 14
category: side projects
---

<img src="{{ '/assets/img/driver_fatigue/circuit.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Prototype circuit boards with the PIC18F452 microcontroller and an LCD showing 'Driver Safety System'">

<div class="caption">The prototype: the PIC18F452 board in the middle, the steering-angle potentiometer on the left, and the LCD.</div>

Falling asleep at the wheel causes a large share of road accidents, and in Sri Lanka studies put it above 30%. Commercial
drowsiness detectors exist, but they ship in new vehicles while most Sri Lankans drive older ones. As an undergraduate
mechatronics project at the University of Moratuwa (with B.S. Ariyarathna and S.D. Gunawardhana), we built a low-cost
add-on that warns the driver using only the steering wheel.

**How it detects fatigue.** An alert driver makes small, quick steering corrections around the mean angle. A drowsy
driver drifts in large, slow deviations and then snaps back. Following King et al.'s algorithm for heavy-truck drivers,
the system splits the steering angle signal into blocks, one per excursion away from the mean, and computes

$$\text{Amp}_\theta = \frac{100}{N} \sum_{j=1}^{M} A_j \, t_j$$

where $$A_j$$ is the area of block $$j$$, $$t_j$$ its length, $$N$$ the number of samples and $$M$$ the number of blocks.
When $$\text{Amp}_\theta$$ goes past the threshold (5.0 to 6.3 in the original study), the LCD shows a warning and a
buzzer sounds.

**Avoiding false alarms.** The measure only makes sense on straight roads, so the system:

- switches detection off while an indicator lamp is on, so turns and overtakes don't trigger it
- turns detection on only in a "highway mode", when the vehicle's speed stays above a set limit with little variation

**Hardware.** A PIC18F452 runs the algorithm. Its hardware multiplier and 31-level stack handle the arithmetic and
interrupts that the PIC16 family couldn't. Steering angle is sampled on a timer interrupt and smoothed by averaging.
For the demo, two potentiometers stand in for the steering angle sensor and the speedometer. A UART link through a
TTL-to-USB converter lets a laptop tune the scaling factor and block count on the fly and log fatigue data, since the
right thresholds vary from driver to driver.

- Report: [Driver Fatigue Detection System (PDF)]({{ '/assets/pdf/driver_fatigue_detection_system_2013.pdf' | relative_url }})
