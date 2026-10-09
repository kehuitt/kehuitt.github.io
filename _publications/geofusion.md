---
layout: research
title: 'GeoFusion: Geometric Consensus Learning and Anchored Feature Fusion for Multi-View
  Depth Estimation'
collection: publications
permalink: /publication/geofusion/
date: '2026-09-01'
date_precision: month
year: 2026
status: published
venue: IEEE Robotics and Automation Letters (RA-L)
authors:
- Ying Zhu
- Kehui Tan
- Hong Liu
authorship: Co-first author
selected: true
display_order: 1
paperurl: https://doi.org/10.1109/LRA.2026.3713729
doi: 10.1109/LRA.2026.3713729
codeurl: https://github.com/kehuitt/GeoFusion
excerpt: Combines geometric consensus-aware cost aggregation with geometrically anchored
  feature rectification to improve multi-view depth estimation.
tags:
- Multi-view depth
- Geometric consistency
- Feature fusion
comments: false
share: false
author_profile: false
teaser: /images/publications/geofusion-overview.webp
teaser_alt: Geometric consensus-aware aggregation and geometrically anchored feature
  rectification.
teaser_caption: Geometric consensus-aware aggregation and geometrically anchored feature
  rectification.
short_venue: RA-L
scholarurl: https://scholar.google.com/citations?view_op=view_citation&hl=en&user=ZvQiaNAAAAAJ&citation_for_view=ZvQiaNAAAAAJ:Y0pCki6q_DkC
---

{% include publication-meta.html paper=page %}

<nav class="paper-outline" aria-label="Paper sections"><a href="#design">Method</a><a href="#results">Results</a><a href="#ablations">Ablations</a></nav>

## The problem

Multi-view depth estimation combines images taken from different viewpoints to recover metric scene geometry. Matching becomes unreliable when views disagree, while strong monocular features do not necessarily preserve the geometry needed for depth reconstruction.

## How GeoFusion works

- **Geometric Consensus-aware Cost Aggregation (GC-CA)** checks whether depth hypotheses agree across views. It suppresses inconsistent evidence before cost aggregation, helping the model avoid misleading matches.
- **Geometrically Anchored Feature Rectification (GAFR-Net)** uses the multi-view cost volume as a metric anchor to rectify monocular features. This brings learned visual cues into closer agreement with the geometric reconstruction.
- The two components connect **reliable multi-view matching** with **geometry-aware feature fusion**, rather than treating matching and feature quality as independent problems.


<h2 id="design">Inside the architecture</h2>

### From camera views to a consensus-aware cost volume

GeoFusion starts from a target image and reference views with camera calibration and relative poses. Reference features are warped into the target camera frustum over inverse-depth hypotheses. These pairwise matches provide the evidence used to estimate depth.

Photometric confidence alone can be misleading in textureless areas, shadows, or occluded regions. GC-CA therefore measures how far each view's local depth hypothesis deviates from the global consensus. A soft geometric weight suppresses outliers without imposing a hard accept/reject boundary. Multiplying the photometric and geometric weights gives the final view weight used for cost aggregation.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/geofusion-figure-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-figure-3.webp' | relative_url }}" alt="Figure 3. Intermediate depth, photometric confidence, geometric suppression, and final aggregation weights in GC-CA." width="1560" height="570" loading="lazy" decoding="async"></a><figcaption>Figure 3. Intermediate depth, photometric confidence, geometric suppression, and final aggregation weights in GC-CA. <a href="{{ '/images/publications/details/geofusion-figure-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The intermediate maps distinguish photometric confidence from geometric agreement. The suppression map highlights locations where an apparently confident image match lacks support from the other views; the final weight combines both sources of evidence.

### Why the fusion has a direction

The monocular branch predicts an initial depth map, which is encoded over the same depth hypotheses as the multi-view cost volume. Both representations are embedded before fusion. In geometric-anchored cross-attention (GCA), **monocular features form the queries, while multi-view features form the keys and values**. The metric information from calibrated views guides the monocular representation.

Hierarchical global aggregation (HGA) then uses self-attention to capture longer-range spatial relationships. A learnable residual connection adds the refined representation back to the initial features, preserving local detail. The model supervises both initial monocular depth and the final depth estimate.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/geofusion-figure-4.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-figure-4.webp' | relative_url }}" alt="Figure 4. GAFR-Net: geometric-anchored cross-attention, hierarchical global aggregation, and residual fusion." width="1575" height="600" loading="lazy" decoding="async"></a><figcaption>Figure 4. GAFR-Net: geometric-anchored cross-attention, hierarchical global aggregation, and residual fusion. <a href="{{ '/images/publications/details/geofusion-figure-4.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

<h2 id="results">Benchmark results and efficiency</h2>

**Reading the metrics.** Abs Rel, Sq Rel, RMSE, and logarithmic RMSE measure depth error, so lower is better. The threshold accuracy columns measure the fraction of predictions within a specified ratio of ground-truth depth, so higher is better. Table 1 preserves each method's backbone and supervision setting; comparisons should be read within that context.

<figure class="paper-evidence table-evidence" style="--table-width:980px"><a href="{{ '/images/publications/details/geofusion-table-1.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-table-1.webp' | relative_url }}" alt="Table 1. KITTI and 7-Scenes depth-estimation comparison across backbones and supervision settings." width="1572" height="585" loading="lazy" decoding="async"></a><figcaption>Table 1. KITTI and 7-Scenes depth-estimation comparison across backbones and supervision settings. <a href="{{ '/images/publications/details/geofusion-table-1.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The main comparison covers outdoor KITTI Odometry and indoor 7-Scenes. With ResNet-18, GeoFusion reports KITTI Abs Rel of **0.034**, compared with **0.043** for DynamicMVS, and 7-Scenes Abs Rel of **0.093**, compared with **0.096**. The paper also evaluates the larger EfficientNet-B5 backbone.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/geofusion-table-2.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-table-2.webp' | relative_url }}" alt="Table 2. Model complexity and inference speed on an RTX 4090." width="717" height="408" loading="lazy" decoding="async"></a><figcaption>Table 2. Model complexity and inference speed on an RTX 4090. <a href="{{ '/images/publications/details/geofusion-table-2.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

Table 2 reports the cost of the complete models, including parameter counts and speed at 256 × 512 on an RTX 4090. With ResNet-18, GeoFusion runs at **15.75 FPS** versus DynamicMVS at **14.99 FPS**, with both using 20.67 million parameters. These measurements belong to the stated hardware and input resolution.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/geofusion-table-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-table-3.webp' | relative_url }}" alt="Table 3. Zero-shot transfer from KITTI to TUM-RGBD." width="744" height="324" loading="lazy" decoding="async"></a><figcaption>Table 3. Zero-shot transfer from KITTI to TUM-RGBD. <a href="{{ '/images/publications/details/geofusion-table-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

For zero-shot transfer, KITTI-trained models are evaluated directly on 11 TUM-RGBD reconstruction sequences. GeoFusion has the lowest RMSE in this comparison (**1.507**), while MonoRec has the lowest Abs Rel (**0.526**, versus GeoFusion's 0.536). The table makes the metric-dependent comparison visible.

<h2 id="ablations">What the ablations test</h2>

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/geofusion-table-4.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-table-4.webp' | relative_url }}" alt="Table 4. Ablation of GC-CA and GAFR-Net on KITTI." width="732" height="411" loading="lazy" decoding="async"></a><figcaption>Table 4. Ablation of GC-CA and GAFR-Net on KITTI. <a href="{{ '/images/publications/details/geofusion-table-4.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

Table 4 separates cost aggregation from feature rectification at two reference-frame counts. Both contribute to the full model, and the combined configuration improves over either individual component.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/geofusion-table-5.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-table-5.webp' | relative_url }}" alt="Table 5. Ablation of geometric cross-attention and hierarchical global aggregation." width="741" height="321" loading="lazy" decoding="async"></a><figcaption>Table 5. Ablation of geometric cross-attention and hierarchical global aggregation. <a href="{{ '/images/publications/details/geofusion-table-5.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

GCA and HGA have different roles. In Table 5, GCA alone improves Abs Rel from **0.038 to 0.036**. HGA alone gives **0.040**, while combining GCA and HGA gives **0.034**. Global aggregation works best after the features have been geometrically rectified.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/geofusion-table-6.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-table-6.webp' | relative_url }}" alt="Table 6. Comparison of feature-fusion directionality." width="738" height="288" loading="lazy" decoding="async"></a><figcaption>Table 6. Comparison of feature-fusion directionality. <a href="{{ '/images/publications/details/geofusion-table-6.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

Table 6 directly tests fusion direction: multi-view-to-monocular rectification gives Abs Rel **0.034**, versus **0.035** for bidirectional fusion and **0.035** for monocular-to-multi-view rectification. The same comparison also reports the remaining error and accuracy metrics.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/geofusion-table-7.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-table-7.webp' | relative_url }}" alt="Table 7. Sensitivity to consensus coefficient and number of input frames." width="744" height="453" loading="lazy" decoding="async"></a><figcaption>Table 7. Sensitivity to consensus coefficient and number of input frames. <a href="{{ '/images/publications/details/geofusion-table-7.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/geofusion-figure-5.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-figure-5.webp' | relative_url }}" alt="Figure 5. Effect of input-frame count on depth errors, with and without geometric consensus." width="1506" height="315" loading="lazy" decoding="async"></a><figcaption>Figure 5. Effect of input-frame count on depth errors, with and without geometric consensus. <a href="{{ '/images/publications/details/geofusion-figure-5.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The sensitivity study varies the consensus coefficient and the number of reference frames. Without geometric consensus, adding views can also add inconsistent matching evidence. The selected coefficient **β = 0.5** reduces error as the view count increases in the reported experiment.

<h2 id="examples">Reconstruction and qualitative examples</h2>

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/geofusion-figure-1.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-figure-1.webp' | relative_url }}" alt="Figure 1. Point-cloud reconstruction examples." width="750" height="723" loading="lazy" decoding="async"></a><figcaption>Figure 1. Point-cloud reconstruction examples. <a href="{{ '/images/publications/details/geofusion-figure-1.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The reconstruction examples connect the predicted depth maps to recovered 3D structure. The comparison below pairs depth predictions with error maps; warmer colors indicate larger errors, and marked regions highlight fine structures and boundaries.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/geofusion-figure-6.webp' | relative_url }}"><img src="{{ '/images/publications/details/geofusion-figure-6.webp' | relative_url }}" alt="Figure 6. Depth predictions and error maps on KITTI." width="1440" height="717" loading="lazy" decoding="async"></a><figcaption>Figure 6. Depth predictions and error maps on KITTI. <a href="{{ '/images/publications/details/geofusion-figure-6.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

[All publications]({{ "/publications/" | relative_url }})
