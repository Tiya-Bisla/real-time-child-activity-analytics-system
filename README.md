# real-time-child-activity-analytics-system

# Overview

This repository documents the **system architecture, dataset strategy, performance evaluation, and engineering considerations** behind a real-time child activity recognition system using **YOLOv8s**.

The system analyzes CCTV video streams in indoor kindergarten environments and classifies child activities in real time.

Due to research and institutional constraints, implementation source code is not publicly released. This repository focuses on **architectural design, experimental configuration, benchmarking results, and system-level analysis**.

---

# Problem Definition

Monitoring children’s physical engagement and activity levels presents multiple technical challenges:

- **Real-time inference requirement**
- **Activity classification from live video**
- **Indoor lighting variability**
- **Class imbalance in dataset**
- **Limited labeled data**
- **Robust detection under motion blur and occlusion**

The objective was to design a **GPU-accelerated system** capable of classifying five primary activity states:

- **Sitting**
- **Standing**
- **Running**
- **Laying**
- **Jumping**

Additionally, spatial context (_in-room / on-mat_) was included to enhance activity interpretation.

---

# System Architecture

The system follows a structured real-time processing pipeline:

CCTV Video Input  
↓  
Frame Extraction (30 FPS)  
↓  
YOLOv8s Detection + Activity Classification  
↓  
Bounding Box Generation  
↓  
Confidence Filtering  
↓  
Frame-Level Activity Logging  
↓  
Activity Duration & Vitality Analysis  

The architecture prioritizes:

- **Low inference latency**
- **Lightweight model selection**
- **Efficient GPU utilization**
- **Modular extension capability**

---

# Model Architecture

The system uses **YOLOv8s**, consisting of:

- **CSP-based backbone**
- **PAN-FPN neck structure**
- **Anchor-free detection head**
- **Decoupled classification and bounding box regression branches**
- _Binary Cross-Entropy loss_ for classification
- _CIoU_ and _Distribution Focal Loss (DFL)_ for bounding box regression

The model was selected due to its balance between **speed and accuracy**, making it suitable for real-time CCTV deployment.

---

# Dataset Description

**Dataset Name:** _KAR (Kid Activity Recognition Dataset)_

**Total Images:** 6,489  

- **Training:** 5,026 (~77%)  
- **Validation:** 1,463  

**Image Resolution:** 640 × 640  

**Annotation Tool:** Makesense.ai  

Nine activity-position classes were created by combining activity type and spatial context.

---

# Training Configuration

- **Model:** YOLOv8s  
- **Image Size:** 640 × 640  
- **Epochs:** 300  
- **Batch Size:** 16  
- **Optimizer:** SGD  
- **Hardware:** NVIDIA A100 GPU  
- **Training Duration:** ~2.5 hours  

---

# Performance Metrics

## Overall Performance

- **Average Precision:** 83.9%
- **Average Recall:** 89.5%
- **F1 Score:** 86.6%
- **Overall Accuracy:** 88.7%

---

# Real-Time Inference Performance

Video input processed at **30 FPS**.

Observed latency:

- **First frame:** ~108.1 ms  
- **Subsequent frames:** ~6–8 ms per frame  

This demonstrates suitability for **real-time GPU deployment**.

---

# Observed Limitations

- Class imbalance (e.g., limited laying samples)
- Reduced performance for running classes
- Sensitivity to motion blur
- Single-camera assumption
- No identity tracking across frames
- Manual annotation effort required
