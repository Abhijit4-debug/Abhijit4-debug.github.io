---
title: Liouville-Regularized Flow-Matching World Models
summary: Proposed Liouville and Jacobian Frobenius pointwise regularizers for flow-matching latent world models, cutting long-horizon latent drift by 17 orders of magnitude on DMControl Walker-Walk.
date: 2026-03-01
tags:
  - world models
  - generative modeling
  - reinforcement learning
---

- Proposed Liouville (squared divergence) and Jacobian Frobenius pointwise regularizers for flow-matching latent world models, reducing long-horizon latent drift by 17 orders of magnitude at H=128 on DMControl Walker-Walk versus an unregularized baseline.
- Formally proved cycle-consistency is structurally vacuous for rectified flows in the autonomous limit.
- Confirmed empirically that Liouville variants achieve cycle-error floors 3 orders of magnitude lower than naive cycle regularization across all solver tolerances.
