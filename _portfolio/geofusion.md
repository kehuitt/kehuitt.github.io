---
layout: research
title: Multi-View Depth Estimation
permalink: /research/geofusion/
author_profile: false
collection: portfolio
excerpt: Combines geometric consensus-aware cost aggregation with geometrically anchored
  feature rectification to improve multi-view depth estimation.
display_order: 1
publication_slug: geofusion
comments: false
share: false
---

Research project associated with **IEEE Robotics and Automation Letters (RA-L)**.

## Core approach

Combines geometric consensus-aware cost aggregation with geometrically anchored feature rectification to improve multi-view depth estimation.

GC-CA suppresses depth hypotheses that disagree with multi-view geometric consensus. GAFR-Net uses the multi-view cost volume as a metric anchor to rectify monocular features. Evaluation covers KITTI, 7-Scenes, and zero-shot transfer to TUM-RGBD.

## Associated publication

[GeoFusion: Geometric Consensus Learning and Anchored Feature Fusion for Multi-View Depth Estimation]({{ "/publication/geofusion/" | relative_url }})
