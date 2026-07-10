---
title: "YOLO Distillation for Real-Time Inference on a Raspberry Pi Zero"
date: 2026-07-10T14:15:05+07:00
description: Distilling a fine-tuned YOLO into a ~280K-parameter model for real-time landing-pad detection on a Raspberry Pi Zero 2W
image: yolo_distillation/bitterlessondrone.png
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

# YOLO Distillation for Real-Time Inference on a Raspberry Pi Zero

#### Joint project with Theo Coulson, Northwestern University

We distilled a fine-tuned YOLO detector into a custom architecture small enough to run in real time on a Raspberry Pi Zero 2W, alongside a drone's flight control code, so it can spot and land on a landing platform on its own. The final model runs at about a tenth of the teacher's parameter count while keeping detection performance usable for landing.

## The Platform

{{< figure src="/yolo_distillation/bitterlessondrone.png" alt="Small Drone used in the project" width="70%" >}}

The drone system belongs to Mike Rubenstein's lab at Northwestern (built principally by Andrew Curtis) for swarm robotics research. It has three parts: a custom Optitrack interface broadcasting position over Wi-Fi, a ground station for fleet management, and the drone itself, controlled through Python scripts that read Optitrack position data and send setpoints to the flight controller. For this project we built a new drone on the standard frame and taped a Raspberry Pi camera module to the underside. We weren't able to get the model fully integrated with the flight controller in time — our unit had persistent serial connection issues — so testing was done by manually holding the drone over the landing platform.

## Downscoping

The original pitch was a miniaturized VLA running on the drone, adapted to consume Optitrack data directly. We scaled that back for three reasons: shrinking a VLA enough to fit a Pi Zero would have gutted its capability, the Optitrack API doesn't expose enough for a VLA's generality to matter, and collecting and labeling training data for that setup would have taken more time than we had (we also lost close to two weeks of team availability to illness). YOLO distillation for a single fixed object — the landing pad — was a much more tractable and testable target.

## Why the Neck Goes

A standard YOLO has three parts: a backbone extracting features at multiple scales (P3/P4/P5), a neck (FPN) fusing those scales together, and a head producing boxes and classes. The neck exists so the model can handle objects of wildly different sizes. We only ever need to find one object — a landing pad with roughly fixed dimensions — so multi-scale fusion buys us nothing. We drop the neck entirely and predict from a single scale.

## Student Architecture

The student takes a 224×224 image and outputs five numbers: a confidence score and four normalized box corners `(x1, y1, x2, y2)`.

**Depthwise separable convolution (DSConv)** — a regular convolution looks for spatial patterns and mixes channels in one expensive step. DSConv splits this in two: a depthwise conv finds spatial patterns per-channel with no mixing, then a 1×1 pointwise conv mixes channels. Same expressive shape, far fewer parameters.

**CSP block** — our lightweight stand-in for YOLO11's C3k2. Split the input in half along channels; one half runs through two DSConv layers, the other skips straight through; concatenate and mix with a 1×1 conv. The skip path keeps a clean gradient route, similar to a ResNet.

**Backbone** — a stack of strided `ConvBnAct` + `CSP` blocks that downsample 224 → 112 → 56 → 28 → 14 while increasing channel depth, the same progressive-downsampling principle as YOLO11's backbone, just with cheaper blocks.

**Spatial pooling** — the backbone output is pooled to a 4×4 grid rather than collapsed to 1×1. A single global-average vector is nearly translation-invariant, which is great for "is the pad present" but throws away *where* it is. The 4×4 grid keeps enough position information for the box regression to work.

**Head** — a two-layer fully connected network mapping the flattened 4×4×128 features to confidence + box. Box coordinates go through a sigmoid at both train and inference time so the loss and the deployed model see the same mapping. Confidence stays a raw logit during training (for `BCEWithLogitsLoss`) and gets sigmoided only at inference.

The result is 283,541 parameters against YOLO11n's 2,624,080 — about a 9× reduction — with no neck and a single-scale head.

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

The teacher was a YOLO11l fine-tuned for 50 epochs on our own dataset on an RTX 6000 Ada. Data was labeled on Roboflow, which integrates SAM2 for prompt-driven automatic annotation, then exported in YOLO format and converted into single-box regression targets: `[confidence, x1, y1, x2, y2]`, keeping the largest box per image when multiple labels exist and treating unlabeled images as negatives.

Training combines two losses:

- **Detection loss** — `BCEWithLogits` on confidence over every example, plus MSE on box corners computed only over positive examples (no point penalizing coordinates when there's no pad in frame). Box loss is scaled by 5× since raw MSE on normalized coordinates is tiny next to the BCE term and gets ignored otherwise.
- **Distillation loss** — rather than only matching final outputs, we match intermediate feature maps location-by-location, so the teacher's sense of *where* the pad is transfers, not just *whether* it's there. A learned 1×1 adapter projects the student's 128 channels up to the teacher's channel count (captured via a forward hook on the teacher backbone), the teacher's map is pooled down to the student's spatial resolution, both are L2-normalized per spatial location, and we take the MSE between them — matching direction rather than magnitude.

Total loss is `det_loss + 0.3 * kd_loss`, optimized with Adam and a cosine annealing schedule for 50 epochs. The student's backbone features are reused for the KD loss rather than run twice, and the teacher runs under `no_grad`, upscaled to 512px so its features stay on the distribution it was fine-tuned at. Distillation itself took about 45 minutes on an RTX 500 Ada.

## Results

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

Roughly 10× less memory and 100× faster inference than the teacher on the same hardware, with localization on held-out test images that held up well qualitatively. On the Pi Zero 2W itself, inference averaged around 180ms over a one-minute run — not integrated with the full flight stack due to the serial issue mentioned above, but well within the margin needed for detecting a static object at the drone's operating speed. To simulate the visual field of flight, the drone was manually held over the platform:

{{< figure src="/yolo_distillation/holdingdrone.png" alt="Integrated evaluation setup" width="70%" >}}

Examples of inference from the Pi itself:

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

Left to right: a successful detection, a rejected borderline case, and a correct non-detection (no pad in frame).

## What Degraded on the Pi

The model was noticeably less reliable and less well-localized running on the actual Pi camera than on the laptop test set, for a few compounding reasons:

- Some quality loss is expected from a reduction this aggressive.
- Delays getting the ribbon cable for the Pi camera module meant training data was shot on phones in portrait mode; at inference everything gets resized to 224×224 square, so the differing aspect ratios vertically compress the pad relative to what the model saw in training.
- Training data was collected at night under one lighting condition; the Pi tests ran during the day under a skylight, and YOLO-family models are known to be sensitive to lighting shifts.
- The Pi camera module has no IR filter, giving images a red tint that likely confused the model further.
- Few training examples showed the pad partially clipped at the frame edge — a case that matters a lot for a drone doing initial localization, and a priority to fix before flying this on the real system.

## Code

Full architecture, training scripts, and the distillation pipeline are on [GitHub](https://github.com/theoHC/yolo-berry-distillery).
