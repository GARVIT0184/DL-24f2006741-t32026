# Milestone 2 Report
## Industrial Surface Defect Detection

### 1. Dataset Overview

- **Total annotation rows:** 45,572
- **Unique training images:** 11,393
- **Training images:** 9,114
- **Validation images:** 2,279
- **Image resolution:** 128 × 800 pixels
- **Defect classes:** `defect_a`, `defect_b`, `defect_c`, `defect_d`

### 2. RLE and Dataset Analysis (Q1–Q6)

#### Q1. RLE Column-Major Encoding

**Answer:** `12 2 23 4 44 2 47 1`

The mask is encoded using 1-based indexing and column-major traversal (`order='F'`).

#### Q2. RLE Decoding and Pixel Mass

For image `0ZUU8LH57Y`, all four defect masks were decoded using 1-based RLE indices and Fortran-order reshaping.

**Total active pixels: 6,514**

#### Q3. Global Class Distribution

| Defect Class | Images Containing Defect |
|---|---:|
| defect_a | 1,081 |
| defect_b | 313 |
| defect_c | 4,100 |
| defect_d | 1,707 |

- **Most common:** defect_c — 4,100 images
- **Rarest:** defect_b — 313 images

#### Q4. Multi-Label Defect Co-Occurrence

| Defect Pair | Co-occurrence Count |
|---|---:|
| defect_a + defect_b | 42 |
| defect_a + defect_c | 310 |
| defect_a + defect_d | 234 |
| defect_b + defect_c | 95 |
| defect_b + defect_d | 64 |
| defect_c + defect_d | 705 |

**Answer:** `defect_c + defect_d, 705`

#### Q5. Co-Occurrence vs. Correlation

- **Highest correlation pair:** defect_a + defect_d
- **Pearson correlation:** 0.05
- **Does the strongest raw co-occurrence imply the strongest correlation?** No.

Correlation accounts for the individual prevalence of defect classes, unlike raw co-occurrence counts.

#### Q6. Pixel-Level Spatial Overlap

- **Maximum overlapping pixels:** 0
- **Spatially mutually exclusive:** Yes

No shared active pixels were observed between distinct defect-class masks in the training dataset.

### 3. PyTorch Dataset Pipeline (Q7)

The custom PyTorch dataset pipeline loads RGB images and decodes the four defect masks.

**First training batch:**

| Tensor | Shape | Data Type |
|---|---|---|
| Images | `(4, 3, 128, 800)` | `torch.float32` |
| Masks | `(4, 4, 128, 800)` | `torch.float32` |

The images contain three RGB channels, while the masks contain four channels corresponding to the four defect classes.

**Why must target masks be float32?**

Loss functions such as `BCEWithLogitsLoss` expect floating-point targets containing 0s and 1s. Therefore, binary masks must be converted to `torch.float32` instead of remaining integer or Boolean tensors.

For Paradigm 2, the dataset would need to provide a selected defect-class mask and corresponding class-conditioning information rather than all four output masks simultaneously.

### 4. Baseline Model Training and Convergence (Q8)

- **Architecture:** Baseline segmentation model under Paradigm 1
- **Training duration:** 5 epochs

| Metric | Training Loss | Validation Loss |
|---|---:|---:|
| Epoch 1 | 0.2950 | 0.1020 |
| Epoch 5 | 0.0294 | 0.0258 |

- **Best epoch:** 5
- **Generalization assessment:** Healthy generalization.

Both training and validation losses decreased substantially over the five epochs. The validation loss remained low, indicating that the model learned effectively without showing an evident overfitting pattern in the reported results.

### 5. Summary

Milestone 2 covers RLE encoding and decoding, defect prevalence, co-occurrence and correlation analysis, pixel-level spatial overlap, PyTorch dataset construction, and baseline model training.

The analysis and implementation are documented in the Milestone 2 notebook, with the corresponding numerical results reported above.
