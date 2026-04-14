# Audio Event Detection (Edge AI)

This project detects target audio events from noisy recordings under low-data and edge deployment constraints.

## Features
- Spectral flux-based event detection
- Pseudo-labeling for low-data learning
- Lightweight ML model (<10MB)
- Real-time inference capability

## Approach
- Signal processing (spectral flux)
- Feature extraction (MFCC, spectral features)
- Gradient Boosting classifier
- Post-processing for clean timestamps

## Results
- F1 Score: 1.0 (training estimate)
- Model Size: 0.08 MB
- Latency: ~0.2 ms/sec audio

## Files
- `notebook/` → main pipeline
- `outputs/` → predictions
- `docs/` → explanation

## How to Run
Open the notebook in Google Colab and run all cells.

Note: F1 of 1.0 reflects evaluation on training data due to limited ground truth labels. In a production setting, a held-out validation set would give a more honest estimate. The unsupervised spectral flux detector independently identified 22 candidate peaks before supervised refinement reduced this to 7 high-confidence events.
