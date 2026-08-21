---
title: "YOLO Distillation for Real-Time Inference on a Raspberry Pi Zero"
date: 2026-06-10T14:15:05+07:00
description: Distilling a fine-tuned YOLO into a ~280K-parameter model for real-time landing-pad detection on a Raspberry Pi Zero 2W
featureimage: yolo_distillation/front_pic.png
tags:
  - Python
  - PyTorch
  - YOLO
  - Knowledge Distillation
  - Computer Vision
  - Raspberry Pi
draft: false
github: https://github.com/theoHC/yolo-berry-distillery
---

#### Joint project with Theo Coulson, Northwestern University

Distilling a fine-tuned YOLO detector into a custom architecture small enough to run in real time on a Raspberry Pi Zero 2W, so a drone can spot and land on a landing platform on its own — at about a tenth of the teacher's parameter count. Read the <a href="/yolo_distillation/report.pdf" style="text-decoration: underline;">full report</a> for the complete writeup.

## The Platform

{{< figure src="/yolo_distillation/bitterlessondrone.png" alt="Small Drone used in the project" width="50%" >}}

Built on a swarm-robotics drone platform from Mike Rubenstein's lab at Northwestern, with a Raspberry Pi camera module added to the underside. The model itself wasn't integrated with the flight controller due to a hardware serial issue, so it was evaluated by holding the drone over the landing pad by hand.

## Architecture

A standard YOLO has a backbone, a multi-scale neck (FPN), and a head — the neck exists to handle objects of wildly different sizes. Since we only ever detect one object of roughly fixed size, we drop the neck entirely and predict from a single scale, replacing YOLO11's C3k2 blocks with cheaper depthwise-separable ones:

- **DSConv** — splits a regular convolution into a per-channel spatial pass and a 1×1 channel-mixing pass, for a fraction of the compute.
- **CSP block** — splits the input in half, runs one half through two DSConvs, skips the other, concatenates and mixes — a cheap stand-in for C3k2 with a clean gradient path.
- **Backbone** — strided `ConvBnAct` + `CSP` blocks downsampling 224 → 14, same principle as YOLO11 with lighter blocks.
- **Spatial pooling to 4×4** instead of 1×1, so the head keeps a coarse sense of *where* the pad is, not just whether it's present.
- **Head** — a 2-layer FC net predicting confidence + box corners.

283,541 parameters vs. YOLO11n's 2,624,080 — about a 9× reduction.

<style>
.flowchart {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  margin: 2rem auto;
  max-width: 380px;
  font-size: 0.9rem;
}
.flowchart .fc-box {
  width: 100%;
  border: 1px solid rgba(0,0,0,0.15);
  border-radius: 10px;
  padding: 0.6rem 1rem;
  text-align: center;
  line-height: 1.3;
}
.flowchart .fc-box strong { display: block; font-weight: 600; }
.flowchart .fc-box small { color: rgba(0,0,0,0.55); }
.flowchart .fc-arrow {
  width: 2px;
  height: 1.1rem;
  background: rgba(0,0,0,0.35);
  position: relative;
}
.flowchart .fc-arrow::after {
  content: "";
  position: absolute;
  bottom: -1px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 6px solid rgba(0,0,0,0.35);
}
.flowchart .fc-input { background: #eceff1; }
.flowchart .fc-conv { background: #dbeafe; }
.flowchart .fc-csp { background: #ccfbf1; }
.flowchart .fc-pool { background: #ffedd5; }
.flowchart .fc-flatten { background: #eceff1; }
.flowchart .fc-head { background: #ede9fe; }
.flowchart .fc-out { background: #fee2e2; }
</style>

<div class="flowchart">
  <div class="fc-box fc-input"><strong>Input image</strong><small>3 × 224 × 224</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-conv"><strong>ConvBnAct ×2</strong><small>conv + batchnorm + ReLU6, downsample 224 → 56</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-csp"><strong>CSP block</strong><small>feature learning at 56</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-conv"><strong>ConvBnAct</strong><small>downsample 56 → 28</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-csp"><strong>CSP block</strong><small>feature learning at 28</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-conv"><strong>ConvBnAct</strong><small>downsample 28 → 14</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-pool"><strong>Spatial pooling</strong><small>14 → 4 × 4 grid</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-flatten"><strong>Flatten</strong></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-head"><strong>Head</strong><small>2-layer fully connected</small></div>
  <div class="fc-arrow"></div>
  <div class="fc-box fc-out"><small>confidence, x1, y1, x2, y2</small></div>
</div>
<p style="text-align:center; font-size: 0.85rem; color: rgba(0,0,0,0.55); margin-top: -1rem;">Figure — custom student model architecture.</p>

## Training

Teacher: YOLO11l, fine-tuned 50 epochs on a Roboflow-labeled dataset. Student loss combines detection loss (BCE on confidence + weighted MSE on box corners) with a feature-distillation loss — matching teacher and student feature maps location-by-location, via a learned adapter, so the teacher's sense of *where* the pad is transfers along with *whether* it's there. Distillation took about 45 minutes on an RTX 500 Ada.

## Results

On held-out test set images, the student localizes the pad well across a range of scales and distances:

<div class="side-by-side" style="display:flex; gap:1.5rem; justify-content:center; flex-wrap:wrap; margin: 1.5rem 0;">
{{< figure src="/yolo_distillation/padbig.png" alt="Detection at close range" width="65%" >}}
{{< figure src="/yolo_distillation/padsmall.png" alt="Detection at extended range" width="65%" >}}
</div>

Benchmarked on a laptop, 50 runs on a test image:

<style>
.results-table-wrap { margin: 1.5rem 0; overflow-x: auto; }
.results-table {
  width: 100%;
  max-width: 480px;
  margin: 0 auto;
  border-collapse: collapse;
  font-size: 0.92rem;
}
.results-table caption {
  caption-side: top;
  font-weight: 600;
  text-align: center;
  padding-bottom: 0.6rem;
}
.results-table th, .results-table td {
  padding: 0.5rem 1rem;
  text-align: right;
  border-bottom: 1px solid rgba(0,0,0,0.12);
}
.results-table th:first-child, .results-table td:first-child {
  text-align: left;
  font-weight: 500;
}
.results-table thead th {
  border-bottom: 2px solid rgba(0,0,0,0.4);
  font-weight: 600;
}
.results-table tbody tr:last-child td { border-bottom: none; }
.results-table td.custom-col, .results-table th.custom-col { background: #fee2e2; }
</style>

<div class="results-table-wrap">
<table class="results-table">
<caption>Performance comparison on test image (50 runs)</caption>
<thead>
<tr><th>Metric</th><th>YOLO11n</th><th class="custom-col">Custom Model</th></tr>
</thead>
<tbody>
<tr><td>Params</td><td>2,624,080</td><td class="custom-col">283,541</td></tr>
<tr><td>Size (MB)</td><td>10.01</td><td class="custom-col">1.08</td></tr>
<tr><td>Avg (ms)</td><td>105.07</td><td class="custom-col">11.4</td></tr>
<tr><td>Best (ms)</td><td>104.62</td><td class="custom-col">1.11</td></tr>
<tr><td>Worst (ms)</td><td>108.21</td><td class="custom-col">1.30</td></tr>
<tr><td>FPS</td><td>9.52</td><td class="custom-col">880.45</td></tr>
</tbody>
</table>
</div>

Roughly 10× less memory and 100× faster inference than the teacher, on the same hardware. On the Pi Zero 2W itself, inference averaged ~180ms — plenty fast for spotting a static landing pad. Testing was done by holding the drone over the platform by hand:

{{< figure src="/yolo_distillation/holdingdrone.png" alt="Integrated evaluation setup" width="50%" >}}

Examples of inference from the Pi:

<style>
.pi-examples {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  flex-wrap: wrap;
  margin: 1.5rem 0;
}
.pi-examples figure {
  flex: 1;
  min-width: 220px;
  max-width: 30%;
}
</style>

<div class="pi-examples">
{{< figure src="/yolo_distillation/frame00127_small.jpg" alt="Successful detection from the pi" >}}
{{< figure src="/yolo_distillation/frame00148_small.jpg" alt="Rejection in a borderline case" >}}
{{< figure src="/yolo_distillation/frame00060_small.jpg" alt="Successful non-detection from the pi" >}}
</div>

Left to right: a successful detection, a rejected borderline case, and a correct non-detection.

On-device performance was noticeably shakier than on the laptop test set — a mismatch between training data (phone camera, portrait, night) and the Pi's camera (no IR filter, daylight, different aspect ratio) accounts for most of the gap.
