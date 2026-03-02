# Dataset Description

## Overview
- Total images: 6,489 extracted from 30 FPS classroom video
- Classes: 9 activity-position classes (Sitting, Standing, Running, Laying, Jumping, In-room/In-room+mat context)
- Resolution: 640x640 pixels

## Data Collection & Labeling
- Extracted frames at intervals ensuring minimal redundancy
- Each image manually labeled for activity and position
- Considered occlusion, lighting variations, and multi-child scenarios

## Engineering Rationale
- 640x640 chosen to balance detection accuracy and real-time inference
- Class coverage ensures model generalizes across common classroom activities
- Sparse rare activities were intentionally included for system robustness

## Limitations & Considerations
- Dataset size is modest → may not cover all edge cases
- Single classroom context → deployment in other environments may require fine-tuning
- No release of images to protect privacy → evaluation relies on metrics rather than reproducing experiments
