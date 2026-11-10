# Milestone 4 – Training Dynamics and Controlled Experimentation

## Project
**Industrial Surface Anomaly Segmentation**  
Course: Introduction to DL and GenAI Project (BSDA2001P)

## 1. Training Dynamics – Question 8

A baseline U-Net was trained for 6 epochs using a 960/240 train-validation split.

| Epoch | Train Loss | Validation Loss | Official Score |
|---:|---:|---:|---:|
| 1 | 0.852509 | 0.843234 | 0.166613 |
| 2 | 0.805390 | 0.796908 | 0.245359 |
| 3 | 0.778405 | 0.771254 | 0.235321 |
| 4 | 0.762075 | 0.759901 | 0.266638 |
| 5 | 0.753695 | 0.749945 | 0.252012 |
| 6 | 0.750715 | 0.749608 | 0.250038 |

- Lowest validation loss: Epoch 6
- Highest official validation score: Epoch 4
- Best official score: 0.267
- Epoch 6 generalization gap (validation loss − training loss): approximately -0.001

The checkpoint from Epoch 4 is preferred when optimizing for the official validation score.

## 2. Defect-Size Binning – Question 9

The 1,200-image subset produced the following positive image-class pair counts:

| Defect area | Positive pairs |
|---|---:|
| 1–100 px | 5 |
| 101–500 px | 99 |
| 501+ px | 657 |

- Percentage of positive ground-truth pixels in the 501+ bin: 99.24%
- Expected missed positive pairs: 66.05
- Missed pairs from 1–100 px: 3.50
- Missed pairs from 101–500 px: 29.70
- Missed pairs from 501+ px: 32.85
- Bin contributing the most missed pairs: 501+ px

The bin contributing the most missed pairs depends on both the number of pairs and the miss rate, not simply the total number of pixels.

## 3. Controlled Experimentation – Question 10

Four experiments were evaluated over six epochs.

| Run | Best Official Score | Empty False-Alarm Rate |
|---|---:|---:|
| R0 | 0.315 | 6.9% |
| R0b | 0.308 | 3.6% |
| R1 | 0.185 | 100.0% |
| R2 | 0.279 | 40.6% |

### Noise and isolated gains

- Noise band: approximately 0.007
- R1 gain over R0: approximately -0.130
- R2 gain over R0: approximately -0.036
- R1 exceeds twice the noise band: No
- R2 exceeds twice the noise band: No

### Interpretation

Changing the sampler, warmup/cosine schedule and AMP together does not isolate the effect of any individual component.

The higher false-alarm rate with R2 suggests that square-root sampling shifts training toward positive examples, increasing false positives on empty image-class pairs.

## 4. Conclusion

The experiments show that the best validation loss and the best official validation score do not necessarily occur at the same epoch. Defect-size distribution and false-positive behavior on empty image-class pairs are important considerations when evaluating segmentation performance.

The controlled experiments also demonstrate why individual training components should be changed separately when measuring their effects.
