# Milestone 1 Report
## Industrial Surface Defect Detection

### 1. Dataset Overview

- **Total rows:** 45,572
- **Unique images:** 11,393
- **Image resolution:** 128 × 800 pixels
- **Pixels per image:** 102,400
- **Defect classes:** `defect_a`, `defect_b`, `defect_c`, `defect_d`

### 2. Dataset and Metric Analysis (Q1–Q10)

#### Q1. Most Frequent Defect Class Count + Clean Images

| Defect Class | Positive Masks |
|---|---:|
| defect_a | 1,081 |
| defect_b | 313 |
| defect_c | 4,100 |
| defect_d | 1,707 |
| Completely clean images | 5,516 |

**Answer:** 4,100 + 5,516 = **9,616**

#### Q2. Images Containing Multiple Defect Classes

**Answer: 1,198 images**

Images containing more than one defect class are counted by grouping the dataset by `image_id` and counting distinct classes with non-empty masks.

#### Q3. Overlapping Defect Pixels

**Answer: 0 pixels**

Class-specific RLE masks are decoded onto the native 128 × 800 canvas. No pixels overlap between two or more defect-class masks.

#### Q4. RLE Boundary Audit

- **Malformed non-empty RLE rows:** 0
- **Maximum pixel index:** 102,400
- **Valid pixel index limit:** 102,400

All non-empty RLE runs remain within the image canvas.

#### Q5. Distortion Caused by Resizing to 256 × 256

| Measurement | Calculation | Result |
|---|---|---:|
| Width scale factor | 256 / 800 | 0.32 |
| Height scale factor | 256 / 128 | 2.00 |
| Distortion ratio | 2.00 / 0.32 | 6.25 |

Direct square resizing compresses the horizontal dimension and stretches the vertical dimension, potentially distorting defect shapes.

#### Q6. Cohen's d: Defect Versus Local Background

| Defect Class | Cohen's d | Absolute Cohen's d |
|---|---:|---:|
| defect_a | 0.169046 | 0.169046 |
| defect_b | 0.549754 | 0.549754 |
| defect_c | 0.188645 | 0.188645 |
| defect_d | -0.020579 | 0.020579 |

**Answer:** `defect_d` has the smallest absolute effect size, **0.020579**.

Its local intensity contrast is weak. Texture- or frequency-based features may help distinguish this class.

#### Q7. Dice Score for Empty Ground Truth

| Ground Truth | Prediction | Dice Score |
|---|---|---:|
| Empty | Empty | 1.0 |
| Empty | Two false-positive pixels | 0.0 |

#### Q8. Toy Composite Metric

Given 10 positive pairs (6 with Dice = 0.80 and 4 with Dice = 0.40) and 40 empty pairs (36 predicted empty and 4 with 1,024 false-positive pixels each):

- **Foreground Dice:** 0.6400
- **False-positive score:** 0.9990
- **Empty accuracy:** 0.9000

Using the weights of 70% foreground Dice, 20% false-positive score, and 10% empty accuracy:

**Final score:**

`0.70 × 0.6400 + 0.20 × 0.9990 + 0.10 × 0.9000 = 0.7378`

#### Q9. Empty-Mask Prevalence and All-Empty Predictor

- **Total image-class pairs:** 45,572
- **Empty pairs:** 38,371
- **Emptiness rate:** 0.841986
- **Naive mean Dice of an all-empty predictor:** 0.841986
- **Official composite score of an all-empty predictor:** 0.3000

The naive mean Dice is high because empty ground-truth masks receive a score of 1 when the prediction is also empty. The official metric balances foreground Dice, false positives, and empty-mask accuracy.

#### Q10. Connected-Component Analysis

Connected components are identified using 8-connectivity, where diagonal neighbours are considered connected.

| Defect Class | Smallest Component (pixels) | Pixels Lost Below 50 px | Loss (%) | Pixels Lost Below 200 px | Loss (%) |
|---|---:|---:|---:|---:|---:|
| defect_a | 3 | 276 | 0.0341% | 123,165 | 15.2171% |
| defect_b | 93 | 0 | 0.0000% | 2,887 | 1.3384% |
| defect_c | 1 | 8,234 | 0.0361% | 76,537 | 0.3359% |
| defect_d | 1 | 1,507 | 0.0123% | 9,892 | 0.0810% |

For `defect_b`, the smallest component contains **93 pixels**. Filtering components smaller than 50 pixels removes no ground-truth pixels from this class, while a 200-pixel threshold removes 1.3384%.

### 3. Image-Level Stratification (Q11)

- **Total unique images:** 11,393
- **Unique defect combinations:** 15
- **Images containing defect_b:** 313
- **20% holdout target:** 63 images

The image-level combinations are:

| Defect Combination | Number of Images |
|---|---:|
| clean | 5,516 |
| defect_a | 601 |
| defect_a + defect_b | 23 |
| defect_a + defect_b + defect_c | 8 |
| defect_a + defect_b + defect_d | 11 |
| defect_a + defect_c | 215 |
| defect_a + defect_c + defect_d | 87 |
| defect_a + defect_d | 136 |
| defect_b | 151 |
| defect_b + defect_c | 67 |
| defect_b + defect_c + defect_d | 20 |
| defect_b + defect_d | 33 |
| defect_c | 3,105 |
| defect_c + defect_d | 598 |
| defect_d | 822 |

Stratification by complete defect combinations helps preserve the distribution of defect patterns between training and holdout sets.

### 4. Starter Segmentation Baseline (Q12)

The baseline evaluation uses the native image resolution of 128 × 800 pixels. The model is intended to be trained for 5–10 epochs and evaluated at probability thresholds of 0.3, 0.5, 0.7, and 0.9, without a minimum-area filter.

The threshold sweep compares segmentation performance at different confidence levels. Lower thresholds generally produce more positive predictions, while higher thresholds produce fewer positive predictions.


