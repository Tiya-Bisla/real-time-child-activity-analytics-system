# System Design: Real-Time Child Activity Analytics System

## 1. Overview

This system detects and analyzes child activities in real-time from classroom video feeds. It is designed for **operational deployment**, emphasizing low latency, scalability, and modularity rather than purely academic evaluation.  

Key design priorities:

- **Real-time performance** – inference per frame <10 ms after warmup
- **Modularity** – detection, tracking, activity analysis decoupled
- **Scalability** – designed to accommodate multiple cameras and high frame-rate feeds
- **Interpretability** – outputs structured activity summaries, visualizations, and performance metrics

---

## 2. High-Level Architecture

Video Feed → Preprocessing → YOLOv8s Activity Detection → Tracking & Activity Aggregation → Metrics & Visualization

| Step                     | Description                               |
|--------------------------|-------------------------------------------|
| Video Feed               | Capture raw camera frames                  |
| Preprocessing            | Resize, normalize, optional masking       |
| YOLOv8s Detection        | Detect activity-position classes          |
| Tracking & Aggregation   | Link detections across frames             |
| Metrics & Visualization  | Generate charts & summaries               |

**Components:**

1. **Video Feed Input**
   - Supports high-frame-rate cameras (30 FPS)
   - Preprocessing handles resizing, normalization, and optional background masking
2. **YOLOv8s Detection Module**
   - Single-stage object detection for child activity + position
   - Produces bounding boxes and class probabilities
3. **Tracking & Aggregation**
   - Associates detections across frames
   - Aggregates activity counts, duration, and engagement statistics
4. **Metrics & Visualization**
   - Generates charts for active vs inactive ratios, per-activity duration
   - Stores performance metrics for post-analysis

---

## 3. Design Decisions and Trade-offs

| Decision | Rationale | Trade-offs |
|----------|-----------|------------|
| **YOLOv8s** | Lightweight, real-time optimized, high detection accuracy | Slightly lower precision vs larger YOLO variants |
| **Single-camera first** | Simplifies implementation and latency measurement | Cross-camera consistency requires future work |
| **SGD Optimizer** | Stable convergence, predictable learning | Slightly longer training than adaptive optimizers |
| **Batch size = 16** | Balances GPU memory with throughput | Larger batches might speed up training but require more memory |
| **640x640 input** | Optimal compromise between detection accuracy and inference speed | May lose small object fidelity |
| **Results-only release** | Protects sensitive child data and model weights | Limits reproducibility for external developers |

---

## 4. Performance & Scalability Considerations

- **Inference speed:** 6–8 ms per frame on NVIDIA A100 after warmup → supports ~120–160 FPS if batched efficiently
- **Memory footprint:** ~350 MB GPU memory per model; multi-camera scaling feasible with multiple A100s or batch scheduling
- **Throughput vs latency:** Priority is low latency for real-time alerts; throughput optimizations (e.g., batch inference) are secondary
- **Extensibility:** Modular architecture allows swapping detection models, adding tracking algorithms, or integrating edge devices

---

## 5. Limitations

- **Single-camera bias:** Current metrics assume a single view; occlusion may reduce accuracy
- **Activity granularity:** Only 5 primary activities tracked; complex actions require future expansion
- **Dataset coverage:** 6,489 images may not capture all classroom scenarios
- **Deployment constraints:** Real-time inference requires GPU acceleration; CPU-only deployment will significantly increase latency

---

## 6. Next Steps / Deployment Roadmap

1. **Multi-camera integration:** Merge activity counts across cameras with cross-camera identity tracking
2. **Edge deployment:** Evaluate model on embedded GPUs or inference accelerators
3. **API/Dashboard:** Real-time reporting of engagement metrics for teachers and parents
4. **Activity extension:** Add composite activities or fine-grained postures
5. **Automated evaluation pipeline:** Continuous monitoring for accuracy drop over time
