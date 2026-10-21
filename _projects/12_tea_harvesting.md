---
layout: page
title: Selective tea harvesting
description: A purely mechanical attachment that lets an existing tea harvester pick mature shoots and spare the young ones. 2014.
img: assets/img/tea_harvesting/collage.jpg
importance: 10
category: research
---

<img src="{{ '/assets/img/tea_harvesting/collage.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Collage of the prototypes: CAD model of prototype 2, field tests of prototypes 2 and 3, concept of prototype 4, the attachment on the harvester in CAD, and the built attachment on the harvester">

<div class="caption">Top: prototype 2 (CAD and field test) and prototype 3 in the field. Bottom: the prototype 4 concept, the attachment modelled on the harvester, and the attachment built onto an Ochiai AM-110VB450.</div>

Sri Lanka's high-quality tea comes from plucking only "two leaves and a bud" from each shoot, and that is still done by
hand. Tea harvesting machines exist, but they cut everything at blade height. That includes the _arimbu_, young shoots
that would have become the next harvest, so the yield drops. For our final-year project at the University of Moratuwa
(with C.M. Samarakoon and K.P.P. Sanjeeva), we set out to make an existing harvester selective. Tea bushes are humid and
the operators often have little training, so we kept the selection purely mechanical: no electronics to waterproof or
repair.

**Picking a selection criterion.** On a TRI 2026 estate, we measured shoot heights and built a hand-held tester to
measure how much force it takes to bend a shoot. Statistical tests showed that harvestable shoots and arimbu differ
clearly in both height (153 mm vs. 78 mm on average) and stiffness. We also defined a selectivity index so that
different mechanisms could be compared fairly.

**Four prototypes.**

- **Prototypes 1–3** sorted shoots by stiffness. A flexible plastic strip bent out of the way for stiff, harvestable
  shoots and pushed soft arimbu aside. They worked on single shoots but failed on dense bunches, where the shoots
  stiffen each other, and short arimbu slipped underneath the strip.
- **Prototype 4** added height. A row of separators sits 74 mm above the plucking table, a height that catches 95% of
  harvestable shoots. Shorter shoots pass underneath into protective cavities. Taller shoots hit the angled aluminium
  edges and slide into the cutting zone, while tall but floppy arimbu bend and drop into the cavities. The edge angle
  came from a model of how a shoot slides along the edge, solved with a small MATLAB tool.

**Result.** We built prototype 4 as an 820 g aluminium attachment for an Ochiai AM-110VB450 harvester. In field tests it
left about 65% of the arimbu on the bush while harvesting every harvestable shoot. Its selectivity index of 44 was the
best of all our designs.

- Demo video: [selective tea harvester on YouTube](https://www.youtube.com/watch?v=ySlhCdYftiw)
- Report: [Developing a selective tea plucking method for an existing tea plucking machine (PDF)]({{ '/assets/pdf/selective_tea_plucking_report_2014.pdf' | relative_url }})
- Papers:
  - [Selection of harvestable tea shoots based on height and stiffness](https://doi.org/10.1109/MIES.2016.7780270) (MIES 2016)
  - Stiffness as a discrimination criterion to select harvestable tea shoots (International Conference on Multidisciplinary Approaches, 2014)
