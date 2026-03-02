# Activity Analysis Summary

This document summarizes the behavioral analytics derived from the real-time activity recognition system.  
Beyond classification accuracy, the system extracts structured insights regarding activity frequency, temporal duration, and engagement patterns.

---

# Activity Count Distribution

The system logs frame-wise detections at 30 FPS and aggregates them to compute total activity counts per class.

Detected activity categories:

- In room | Standing
- In room | Sitting
- In room | Running
- In room | Laying
- In room and mat | Standing
- In room and mat | Sitting
- In room and mat | Running
- In room and mat | Laying
- In room and mat | Jumping

## Observations

- Sitting and standing represent the majority of detections, consistent with expected classroom behavior.
- Running and jumping occur less frequently but represent high-motion engagement states.
- Laying is detected in fewer instances and is typically associated with rest periods.
- Context-aware classes (room vs mat) allow spatial interpretation of engagement zones.

The activity distribution reflects realistic classroom dynamics rather than artificial class balancing.

---

# Temporal Duration Analysis

The system converts frame-level detections into duration metrics using the fixed frame rate:

Duration (seconds) = Number of detected frames / 30

This enables quantification of:

- Time spent sitting
- Time spent standing
- Time spent running
- Time spent laying
- Time spent jumping

## Observations

- Passive states (sitting, laying) show longer continuous durations.
- Active states (running, jumping) appear in shorter bursts.
- Temporal segmentation provides clearer insight than raw frame counts.
- Transition phases (e.g., standing → running) introduce brief classification fluctuations.

This analysis allows estimation of physical engagement intensity during class sessions.

---

# Active vs Inactive State Modeling

For higher-level interpretation, activities were grouped into:

## Active States
- Running
- Jumping
- Standing
- Sitting
- Laying

## Inactive State
- No Detection

"No Detection" represents frames where:
- The target child is occluded,
- Outside the camera field of view,
- Or detection confidence falls below threshold.

## Observations

- Inactive duration helps estimate monitoring gaps.
- Frequent inactive states may indicate occlusion or camera positioning issues.
- Low inactive duration suggests stable tracking under single-camera setup.

---

# Engagement Interpretation

By combining frequency and duration metrics, the system enables:

- Estimation of classroom physical activity levels
- Comparison of active vs passive behavior balance
- Identification of high-energy periods
- Monitoring of prolonged inactivity

This transforms the system from a pure object detector into a behavioral analytics pipeline.

---

# System Constraints in Activity Analysis

- Frame-wise independence may cause short-term activity switching noise.
- No identity tracking is applied across long sequences.
- Temporal smoothing is not implemented.
- Multi-child simultaneous analysis is not fully addressed.

Future improvements may include:

- Multi-frame temporal modeling
- Identity-aware tracking integration
- Statistical smoothing of short-duration spikes
- Multi-camera fusion

---

# Engineering Perspective

The analytics layer demonstrates that the system is not limited to bounding box prediction.  
It supports structured data extraction, time-series analysis, and behavioral pattern interpretation under real-time constraints.

The current implementation validates feasibility for single-camera deployment while leaving room for scalable system expansion.
