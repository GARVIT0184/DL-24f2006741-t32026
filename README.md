# Industrial Surface Defect Detection

## DL and GenAI Project 26T3

This repository contains milestone-wise development for industrial surface defect segmentation using deep learning.

## Project Objective

The objective is to identify and segment surface defects in industrial images. The project focuses on data analysis, mask decoding, segmentation models, evaluation metrics, and threshold optimization.

## Dataset

The dataset contains industrial surface images and Run-Length Encoded (RLE) defect masks for four classes:

- `defect_a`
- `defect_b`
- `defect_c`
- `defect_d`

Images are evaluated at their native resolution of 128 × 800 pixels for the Milestone 1 baseline.

## Milestone 1

Milestone 1 covers the following tasks:

1. Class distribution and clean-image analysis
2. Multi-label defect co-occurrence analysis
3. Spatial mask overlap and output architecture
4. RLE boundary validation
5. Image aspect ratio and geometric distortion
6. Photometric contrast analysis using Cohen's d
7. Dice metric behaviour on empty masks
8. Official weighted evaluation metric
9. All-empty baseline analysis
10. Connected-component filtering and recall
11. Multi-label stratified train-validation split
12. Baseline segmentation training and decision-threshold evaluation

## Repository Structure

```text
.
├── README.md
├── milestone-1.ipynb
├── notebooks/
├── reports/
└── src/
```

The notebook and supporting documentation will be organized as the project progresses.

## Experiments and Evaluation

The baseline experiment uses a multi-output segmentation model with sigmoid probabilities for the four defect classes. Validation performance is examined across decision thresholds of 0.3, 0.5, 0.7, and 0.9.

Evaluation considers foreground Dice, false-positive behaviour on empty image-class pairs, empty-pair accuracy, and the official competition score.

Measured results and experimental settings should be documented alongside the corresponding notebook outputs.

## Reproducibility

The notebook records the data preparation, train-validation split, model training, and threshold evaluation workflow. The competition dataset must be obtained through the authorized dataset source and configured locally before running the notebook.


