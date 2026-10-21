---
layout: page
title: 3DOF transtibial robotic prosthetic limb
description: A below-knee robotic prosthesis with a three-degree-of-freedom ankle, spring-based energy recovery and EMG control. ICIAfS 2014.
img: assets/img/prosthetic_limb/prototype.jpg
importance: 10
category: research
---

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/prosthetic_limb/prototype.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Photo of the 3DOF prosthetic limb prototype fitted with a shoe">
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/prosthetic_limb/cad_model.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Labelled solid model of the final design showing motor, belt drive, ball screw and springs">
  </div>
</div>
<div class="caption">Left: the fabricated prototype. Right: the solid model of the final design.</div>

Transtibial (below-knee) amputations are the most common lower-limb amputations, and in Sri Lanka many are caused by
land mines and accidents. Most transtibial prostheses actuate the ankle in only one direction, up and down. A real
ankle moves in three, and missing the other two leads to an uneven gait. At the University of Moratuwa, our team
designed and built a prosthesis with a **3-degree-of-freedom ankle**.

**Design.** Two of the three degrees of freedom are driven by DC motors and the third is passive:

- **Dorsiflexion/plantarflexion** (foot up/down): a motor turns a ball screw through a toothed belt
- **Abduction/adduction** (foot rotating in/out): a second motor rotates the frame relative to the stump socket, also
  through a belt drive
- **Inversion/eversion** (foot tilting side to side): passive, with two springs that return the foot to neutral

We went through three designs. The first used a worm-and-wheel drive, which was slow. The second used spur gears and a
linear actuator, which made the limb heavy and power-hungry. The final design uses belt drives to save weight and
trims the aluminium frames based on finite element analysis (minimum factor of safety 3.65). The prototype weighs
3.8 kg.

**Energy recovery.** The springs compress during plantarflexion and give that energy back during dorsiflexion. We
modelled this with a robotic tendon approach and swept the spring stiffness. At 30 kN/m the motor's peak power for a
70 kg user drops to 83 W, well below the power a natural ankle produces.

**Control.** We tested two kinds of control:

- **Following a healthy leg:** a potentiometer worn on a healthy leg gave the target angle, and a PD controller on an
  Arduino tracked it. This was tested with three subjects.
- **EMG control:** surface EMG from the tibialis anterior and gastrocnemius, sampled at 2 kHz through a NI DAQ card
  and LabVIEW, drove the up/down motion from the difference of the two muscles' RMS activity.

In both experiments the prosthesis followed the natural limb. Abduction/adduction and inversion/eversion reached close
to natural ranges. Dorsiflexion/plantarflexion reached 12°/32° against the natural 20°/50°, which is still enough for
daily activities.

- Demo video: [3DOF prosthetic limb on YouTube](https://www.youtube.com/watch?v=jit0trzElGw)

**Papers**

- [A 3DOF transtibial robotic prosthetic limb](https://ieeexplore.ieee.org/abstract/document/7069617) (ICIAfS 2014) ([PDF]({{ '/assets/pdf/3dof_transtibial_prosthetic_limb.pdf' | relative_url }}))
- [A review on hybrid myoelectric control systems for upper limb prosthesis](https://ieeexplore.ieee.org/abstract/document/7112334) (MERCon 2015)
