# Milestone 3 – Transfer Learning and Stronger Segmentation Models

## Project
Industrial Surface Anomaly Segmentation

## Objective
This milestone focuses on understanding transfer learning for image
segmentation, comparing pretrained encoders, exploring multi-scale
feature extraction, and evaluating loss functions for highly
imbalanced defect segmentation.

---

## 1. Input Resolution and Batch Configuration

The selected configuration uses:

- Image size: 256 × 256
- Batch size: 8
- Number of workers: 1
- Input channels: 1

The grayscale input format is suitable for the industrial surface
images used in this project.

## 2. Encoder Parameter Comparison

The parameter comparison between the two U-Net architectures:

| Architecture | Total Parameters |
|---|---:|
| ResNet34 U-Net | 24,430,532 |
| EfficientNet-B0 U-Net | 6,251,328 |

Parameter difference:

24,430,532 - 6,251,328 = 18,179,204

For the frozen ResNet encoder, the trainable parameters represent
approximately 12.9% of the total parameters.

EfficientNet-B0 provides a significantly smaller model and is a
potentially useful option for resource-constrained deployment.

## 3. Grayscale Weight Adaptation

A pretrained RGB convolution can be adapted to grayscale input by
summing its three input-channel weights.

This preserves the response that would be obtained when the same
grayscale image is replicated across the RGB channels.

The input-channel dimension is reduced from three channels to one.

## 4. Atrous Spatial Pyramid Pooling (ASPP)

ASPP uses parallel convolutions with different dilation rates to
capture features at multiple spatial scales.

For a 3 × 3 kernel:

- Dilation 6 gives an effective receptive field of 13 × 13.
- Dilation 12 gives an effective receptive field of 25 × 25.

This helps capture both thin scratches and larger diffuse defects
while preserving spatial information.

## 5. Soft Dice Loss

For a highly imbalanced segmentation dataset, Soft Dice Loss
provides a useful overlap-based objective.

The calculated values for the demonstration were:

| Case | Soft Dice Loss |
|---|---:|
| Uniform prediction probability of 0.01 | 0.9851 |
| Accurate prediction with probability 0.9 on positives | 0.0524 |

The result demonstrates that Soft Dice Loss rewards better
foreground overlap.

## 6. Multi-label Segmentation

A single Softmax output assigns exactly one class to each pixel.

Therefore, it cannot represent overlapping defect annotations
at the same pixel.

For datasets where multiple defect labels can overlap, a
multi-label formulation using independent sigmoid outputs
may be more appropriate.

## 7. Combining BCE and Soft Dice Loss

Soft Dice Loss measures overall segmentation overlap, whereas
Binary Cross-Entropy provides pixel-wise supervision.

For a clean image, Soft Dice Loss is zero when both the target
and prediction are empty.

When false-positive pixels appear, the loss increases and
encourages predicted probabilities to move toward zero.

Combining BCE and Soft Dice Loss can provide complementary
supervision for highly imbalanced segmentation datasets.

## 8. Transfer Learning Experiments

The milestone investigates pretrained encoders and their
potential advantages over training a segmentation model
from scratch.

Earlier project experiments indicated that pretrained
segmentation models could improve validation performance.

However, the complete controlled five-epoch comparison
for this milestone was not reproduced in the current run.

Exact epoch-wise loss values and generalization-gap
measurements are therefore not reported here.

## 9. Model Efficiency and Deployment

The parameter ratio between ResNet34 U-Net and
EfficientNet-B0 U-Net is approximately:

24,430,532 / 6,251,328 ≈ 3.91

Rounded ratio: 4

EfficientNet-B0 is the smaller architecture and is a
reasonable candidate for edge deployment.

EfficientNet architectures use depthwise convolutions
within their MBConv blocks to improve computational efficiency.

A definitive comparison of official competition scores
between the two architectures requires controlled
experiments using the same training conditions.

---

## Conclusion

Milestone 3 explored transfer learning, pretrained encoder
parameter counts, grayscale convolution adaptation,
multi-scale feature extraction, and segmentation loss
functions.

The analysis highlights the trade-off between model size,
computational efficiency, and segmentation performance.

Further controlled experiments are needed to establish
the final competition-performance comparison between
ResNet34 and EfficientNet-B0.
