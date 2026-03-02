# System Design – Real-Time Child Activity Analytics System

## 1. System Objective

Design a real-time vision system capable of:

- Detecting children
- Classifying posture/activity states
- Running at real-time speed (≥30 FPS)
- Generating structured analytics (counts, duration, engagement metrics)

The system is built for classroom/kindergarten environments where continuous monitoring and long-duration stability are required.

This is not a research prototype. The design assumes deployment constraints.

---

## 2. High-Level Architecture

### Data Flow

Video Stream (30 FPS)
        ↓
Frame Extraction
        ↓
YOLOv8s Inference (Activity-Position Detection)
        ↓
Post-processing (Confidence Filtering, NMS)
        ↓
Tracking Layer (ID Assignment - Future Extension)
        ↓
Temporal Aggregation Engine
        ↓
Analytics Layer (Counts, Duration, Active vs Inactive)
        ↓
Storage / Visualization

---

## 3. Core Design Decisions

### 3.1 Why YOLOv8s (and not larger models)?

Trade-offs considered:

| Model | Accuracy | Speed | Deployment Feasibility |
|-------|----------|-------|------------------------|
| YOLOv8n | Faster | Lower | Good for edge |
| YOLOv8s | Balanced | Real-time | Selected |
| YOLOv8m/l | Higher | Slower | Heavy for scaling |

Decision:
- YOLOv8s gives strong accuracy (F1 = 86.6%) while maintaining 6–8 ms inference.
- Enables real-time inference even with multiple camera scaling (with GPU batching).

This was a system-level decision, not just a metric-based one.

---

## 4. Activity Modeling Strategy

Instead of separating:
- Detection
- Posture classification
- Context classification

The system uses **joint activity-position classes (9 total)**.

Example:
- Sitting_in_room
- Sitting_in_room_and_mat
- Running_in_room
- etc.

### Why?

Pros:
- Simpler inference pipeline
- Single-stage prediction
- Lower latency
- No cascade model errors

Cons:
- Increased class complexity
- Slightly higher data requirements

The trade-off favors latency and architectural simplicity.

---

## 5. Real-Time Performance Design

### Measured Inference
- 6–8 ms per frame (post-warmup)
- ~125–160 FPS theoretical throughput on NVIDIA A100

Even accounting for:
- I/O
- Post-processing
- Logging

The system comfortably supports real-time 30 FPS streams.

### Scalability Considerations

If scaled to 8 cameras:

Option A: Single GPU batching
Option B: Multi-GPU parallel processing
Option C: Edge + central analytics split

This repository focuses on single-camera validated architecture, but design supports horizontal scaling.

---

## 6. Temporal Aggregation Engine

Raw detections are frame-level.
Analytics require time-level reasoning.

Aggregation Logic:

- Maintain per-activity counters
- Accumulate duration using frame timestamps
- Define active states (Running, Jumping)
- Define inactive states (Sitting, Laying)

This enables:
- Energy estimation
- Engagement modeling
- Session-level summaries

Design principle:
Keep analytics decoupled from detection logic.

---

## 7. Dataset-Aware Engineering Decisions

Constraints:
- 6,489 labeled frames
- Extracted from 30 FPS video
- Risk of temporal correlation

Mitigations:
- Stratified train/validation split
- Class balance monitoring
- Precision-recall trade-off analysis

System acknowledges:
Accuracy alone is insufficient.
Class-specific recall matters more in safety-critical monitoring.

---

## 8. Failure Modes and Limitations

Known challenges:

1. Occlusion during group play
2. Motion blur during fast running
3. Similar posture confusion (standing vs slow walking)
4. Camera-angle dependency

Production Mitigations:
- Multi-view setup (future extension)
- Temporal smoothing
- Confidence threshold tuning
- Tracking integration

---

## 9. Deployment Awareness

### Runtime Environment
- GPU-accelerated inference
- Docker-compatible architecture (planned)
- Model exportable to ONNX/TensorRT

### Edge Deployment Possibility
With quantization and TensorRT:
- Potential Jetson-class deployment
- Requires retraining with calibration

---

## 10. Why This Is a System and Not Just a Model

This repository separates:

- Dataset design
- Training configuration
- Model reasoning
- Performance evaluation
- Analytics logic
- Scalability considerations

The focus is not just detection accuracy,
but operational reliability and architectural clarity.

---

## 11. Future Extensions

- Multi-camera identity tracking
- Real-time dashboard visualization
- Teacher alerting system
- Cross-session longitudinal analysis
- Lightweight edge deployment version

---

# Summary

This system is designed around:
- Real-time constraints
- Deployment practicality
- Analytical usefulness
- Scalable architecture

The goal is not maximum benchmark accuracy.
The goal is reliable, interpretable, deployable activity analytics.
