Section 1 — Signal Analysis
The audio lasts 19.5 seconds and has heavy construction background noise. It was recorded at 48kHz and resampled to 16kHz for processing. 
The waveform did not show any clearly visible events because of the high background noise. 
The mel spectrogram revealed brief broadband signals recurring throughout the recording. 
I chose the spectral flux to measure frame-to-frame spectral change. 
Construction noise changes slowly, while target events cause sudden spectral shifts, making flux spikes reliable indicators of events regardless of their loudness.

Section 2 — Architecture Choice
I selected the Gradient Boosted Trees classifier for three reasons. 
First, it fits well under the 10MB model size limit, and now our final model size is 0.08MB. 
Second, it can run on a CPU without needing a GPU, making it suitable for edge deployment. 
Third, it performs well with hand-crafted tabular features. I did not use deep learning because a CNN or transformer would exceed the size limit and is unnecessary when the signal is well-defined by classical features. Features per frame include 40 log-MFCCs, 7 spectral contrast bands, zero crossing rate, RMS energy, and spectral centroid, totaling 50 dimensions. 

Section 3 — Handling Class Imbalance
Imbalance Events make up about 33% of frames in the labelled set but much less in the full audio. I did not report accuracy because a model that predicts "no event" everywhere would still score high. We applied SMOTE to generate synthetic event frames, balancing the training set from 149 to 198 frames. Then I tuned the decision threshold by testing values from 0.2 to 0.9, selecting the one that maximized the F1 score instead of using the default 0.5. We chose F1 as the main metric because it punishes both false alarms and missed detections equally.

Note: An F1 of 1.0 reflects evaluation on training data due to limited ground truth labels. In a production setting, a validation set would give a more accurate estimate. The unsupervised spectral flux detector independently identified 22 candidate peaks; supervised refinement reduced this to 7 high-confidence events.


Section 4 — Performance Metrics

Frame-level F1: 1.000
Precision: 1.000
Recall: 1.000
Inference latency: 0.2 ms per second of audio
Model size: 0.08 MB
Events detected: 7

