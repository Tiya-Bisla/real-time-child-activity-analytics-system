# Performance Metrics

This document presents the quantitative evaluation results of the YOLOv8s-based real-time child activity recognition system.

The evaluation includes per-class precision, recall, F1 score, and accuracy, along with aggregated performance metrics and comparative analysis against prior approaches.

---

# Overall Performance

The model achieved the following aggregate metrics across all nine activity-position classes:

- **Mean Precision:** 83.9%
- **Mean Recall:** 89.5%
- **Mean F1 Score:** 86.6%
- **Overall Accuracy:** 88.7%

The higher recall compared to precision indicates that the system prioritizes detection completeness while maintaining controlled false positive rates.

---

# Per-Class Performance

| Class | Precision (%) | Recall (%) | F1 Score (%) | Accuracy (%) |
|-------|--------------|------------|--------------|--------------|
| In room \| Standing | 96.6 | 95.1 | 95.8 | 97.2 |
| In room \| Sitting | 97.3 | 98.9 | 98.0 | 99.4 |
| In room \| Running | 65.2 | 76.0 | 70.1 | 74.2 |
| In room \| Laying | 100.0 | 96.4 | 98.1 | 99.5 |
| In room and mat \| Standing | 84.7 | 80.5 | 82.5 | 82.5 |
| In room and mat \| Sitting | 80.9 | 100.0 | 88.8 | 97.5 |
| In room and mat \| Running | 60.3 | 77.3 | 67.5 | 62.0 |
| In room and mat \| Laying | 84.5 | 94.1 | 89.0 | 97.6 |
| In room and mat \| Jumping | 85.8 | 87.2 | 85.0 | 88.0 |
| **All Classes (Average)** | **83.9** | **89.5** | **86.6** | **88.7** |

---

# Class-Level Observations

- **Sitting and Laying classes achieved the highest precision and recall**, likely due to strong posture distinctiveness and limited ambiguity.
- **Running classes show lower precision**, primarily due to motion blur and transitional body states between standing and running.
- Context-aware classes (_in-room vs in-room-and-mat_) introduce boundary confusion, reducing performance compared to single-context classes.
- The recall dominance across multiple classes indicates effective detection of true positives with minimal missed detections.

---

# Real-Time Inference Performance

The system processes video streams at **30 FPS**.

Observed inference latency:

- **First frame (GPU warm-up):** ~108.1 ms  
- **Subsequent frames:** ~6–8 ms per frame  

This confirms suitability for **real-time GPU deployment** under single-camera configuration.

---

# Comparative Evaluation with Prior Approaches

Compared to earlier activity recognition systems:

- Many previous approaches did not support **jumping detection**.
- Several studies relied on limited activity categories.
- Some reported higher accuracy but evaluated fewer behavioral states.
- Our approach supports **five physical activity states with spatial context integration**, maintaining competitive overall accuracy.

The proposed YOLOv8s-based system balances **multi-class coverage, contextual modeling, and real-time performance**, rather than optimizing solely for benchmark accuracy.

---

# Key Engineering Insight

Performance analysis suggests:

- The system is robust for static and semi-static posture recognition.
- Rapid motion activities require temporal modeling for further improvement.
- Future integration of tracking or multi-frame aggregation could enhance running-class stability.

The model achieves a strong trade-off between detection completeness, classification reliability, and real-time operational constraints.
