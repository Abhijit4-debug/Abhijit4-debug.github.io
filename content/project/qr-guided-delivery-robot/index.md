---
title: Autonomous QR-Guided Delivery Robot
summary: Architected and deployed a full ROS autonomy stack on real hardware (Triton robot) for end-to-end QR-guided delivery.
date: 2026-04-01
tags:
  - robotics
  - ros
  - autonomy
---

- Architected and deployed a full ROS autonomy stack on real hardware (Triton robot): Gmapping SLAM for map building, AMCL localization, move_base global/local planning, RPLidar sensing, and a 4-state FSM (WAIT → EXPLORE → QR_TRACK → ARRIVED) for end-to-end delivery.
- Designed and implemented a visibility-scored waypoint generation algorithm from occupancy maps and a depth-camera QR perception stack, enabling robust autonomous delivery with partial localization recovery.
- Validated the full pipeline on physical hardware, achieving successful autonomous delivery runs.
