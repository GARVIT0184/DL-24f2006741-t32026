# Milestone 5 Report: Final Inference and Post-Processing

## Overview

This milestone focuses on threshold selection, connected-component filtering, test-time augmentation, model ensembling and inference efficiency for industrial surface defect segmentation.

## Question 1: Logits and Probability Thresholds

- Logit corresponding to T = 0.3: **-0.8473**
- Strict predictions at T = 0.5: **2**
- Greater-than-or-equal predictions at T = 0.5: **3**
- Dice at T = 0.3: **0.8571**
- Dice at T = 0.5: **0.4000**

## Question 2: Threshold and Minimum-Area Selection

| Configuration | Official Score |
|---|---:|
| X | 0.6730 |
| Y | 0.6908 |
| Z | 0.6120 |

Best configuration: **Y**

FalsePositiveScore and EmptyAccuracy explain why Y performs better despite having a lower foreground Dice than X.

## Question 3: Connected Components

- 8-connected components: **4**
- 4-connected components: **6**
- Foreground pixels remaining at N = 5: **11**
- Foreground pixels remaining at N = 6: **6**

## Question 4: Post-Processing Evaluation

- Foreground Dice after filtering: **0.5775**
- False Positive Score after filtering: **0.9976**
- Empty Accuracy after filtering: **0.8750**
- Official Score before filtering: **0.6820**
- Official Score after filtering: **0.6913**

## Question 5: Recall Cost of Minimum-Area Filtering

- Positive image-class pairs removed: **48**
- Class losing the most pairs: **defect_c**

## Question 6: Test-Time Augmentation

- Horizontal flip dimensions: **(-1,)**
- Vertical flip dimensions: **(-2,)**
- Averaged top-right probability: **0.6667**
- Correct positive pixels: **2**
- Buggy positive pixels: **1**

## Question 7: Model Ensembling

- Soft ensemble Dice: **1.0000**
- Hard voting Dice: **0.6667**
- Weighted ensemble probability at p1: **0.6600**
- Probability-space average: **0.3892**
- Logit-space average: **0.2689**
- Positive method at T = 0.30: **prob_avg**

## Question 8: Inference Memory and Time

- FP32 memory for one prediction set: **8.192 GB**
- FP16 memory: **4.096 GB**
- UINT8 memory: **2.048 GB**
- Config A naive memory: **73.728 GB**
- Config B naive memory: **32.768 GB**
- Neither configuration fits within 16 GB when all predictions are stored as FP32.
- Running accumulator memory: **0.001638 GB**
- Config A: **45,000 forward passes**, **9 minutes**
- Config B: **20,000 forward passes**, **4 minutes**
- Maximum whole models within the available time:
  - 3-view TTA: **7**
  - 2-view TTA: **11**

## Question 9: Final Inference Notebook

The final inference pipeline should apply image preprocessing, model prediction, inverse transformation of TTA outputs, ensemble averaging, per-class thresholding, connected-component area filtering and RLE encoding in the correct order.

Thresholds and minimum-area values should be selected using validation data and checked across multiple splits or seeds to reduce overfitting. An all-empty baseline highlights the risk of optimizing only for empty-image performance.

Before submission, verify the output format, image IDs, RLE encoding, absence of missing predictions, inference time and memory usage.

## Conclusion

Milestone 5 covered the complete inference workflow, from probability thresholding to post-processing and ensemble prediction. The experiments showed that post-processing improved the official score from 0.6820 to 0.6913, while efficient prediction accumulation can reduce memory requirements during inference.
