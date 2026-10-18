# Milestone 1 Report

## Industrial Surface Defect Detection

### 1. Objective

The objective of Milestone 1 is to understand the industrial surface defect dataset, inspect Run-Length Encoded (RLE) masks, analyze class imbalance, and establish a baseline segmentation model.

### 2. Dataset

The dataset contains grayscale industrial surface images with four defect classes: `defect_a`, `defect_b`, `defect_c`, and `defect_d`.

The native image resolution is 128 × 800 pixels. Defect masks are stored using Run-Length Encoding (RLE).

### 3. Data Analysis

The analysis covers class prevalence, clean images, multi-label combinations, mask overlaps, RLE boundary validation, image aspect ratio, photometric contrast, and connected-component statistics.

Exact numerical findings will be added after validating the corresponding calculations against the original dataset.

### 4. Baseline Segmentation Model

A ResNet34 U-Net baseline was trained at the native image resolution using a stratified 80/20 image-level training and validation split.

The model uses four sigmoid output channels, corresponding to the four defect classes. Training uses a combination of binary cross-entropy and foreground Dice loss.

### 5. Training Results

The baseline was trained for five epochs.

| Epoch | Training Loss | Validation Loss |
|---|---:|---:|
| 1 | 0.3848 | 0.3500 |
| 2 | 0.3298 | 0.3328 |
| 3 | 0.3141 | 0.3104 |
| 4 | 0.3039 | 0.3016 |
| 5 | 0.2944 | 0.2969 |

Both training and validation losses decreased during the observed training period.

### 6. Decision Threshold Analysis

The validation threshold sweep evaluated thresholds of 0.3, 0.5, 0.7, and 0.9 without minimum-area filtering.

The measured results were:

| Threshold | Naive Mean Dice | Foreground Dice | Assessment Score |
|---|---:|---:|---:|
| 0.3 | 0.2733 | 0.4623 | 0.5453 |
| 0.5 | 0.2999 | 0.4630 | 0.5487 |
| 0.7 | 0.3273 | 0.4608 | 0.5508 |
| 0.9 | 0.4306 | 0.4338 | 0.5456 |

The assessment score peaked at threshold 0.7, while naive mean Dice peaked at threshold 0.9. Increasing the threshold reduced false positives on empty image-class pairs but also reduced foreground Dice at higher thresholds.

### 7. Limitations and Next Steps

The baseline results are preliminary and depend on the selected holdout split and training configuration. Further work should include per-class error analysis, improved threshold selection, careful evaluation of rare defects, and comparison with additional segmentation architectures.

Exact answers for the remaining dataset audit questions should be added only after their calculations have been verified.

### 8. Reproducibility

The notebook for Milestone 1 is stored in this repository. Dataset files are not included in the repository; the original competition dataset is required to reproduce the analysis and training.
