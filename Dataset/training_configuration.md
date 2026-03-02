# Training Configuration

## Model & Input
- Model: YOLOv8s
- Input resolution: 640x640
- Classes: 9 activity-position classes

## Training Hyperparameters
- Epochs: 300
- Batch size: 16
- Optimizer: SGD
- Learning rate schedule: [describe if used, e.g., cosine decay or step schedule]
- Hardware: NVIDIA A100 GPU

## Preprocessing & Augmentation
- Normalization and resizing
- Data augmentations: [e.g., horizontal flip, brightness adjustments, if any]

## Engineering Rationale
- Batch size 16 balances memory with throughput
- SGD selected for stable convergence on small dataset
- Input size ensures small objects remain detectable without compromising speed
- Real-time inference target <10 ms/frame influenced resolution and model choice
