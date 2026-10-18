Milestone 1 Report — Industrial Surface Defect Detection
Project objective
Analyse the industrial surface defect dataset, validate RLE masks, examine native-resolution geometry and defect statistics, prepare an image-level stratified split, and evaluate the starter segmentation baseline.
Dataset and conventions
- Native resolution: 128 × 800 pixels (102,400 pixels/image).
- Classes: defect_a, defect_b, defect_c, defect_d.
- The Milestone 1 baseline must be evaluated at native resolution.
- Values marked VERIFY were recalled from earlier discussion and have not been rechecked against the current notebook outputs.
Q1–Q10: Dataset and metric analysis
Q1. Most frequent defect class count + completely clean images
Previously recalled answer: 9,616 (VERIFY).
Method: count positive image/class occurrences for each class, count completely clean images, then add the most frequent class count to the clean-image count.
Q2. Images containing more than one defect class
Previously recalled answer: 1,198 images (VERIFY).
Method: group by image_id, count distinct classes with non-empty masks, and count images with more than one class.
Q3. Overlapping defect pixels
Previously recalled answer: 0 pixels (VERIFY).
Method: decode each class-specific RLE onto the native 128 × 800 canvas and sum intersections between class masks. Verify the RLE indexing convention and this result using the current notebook before submission.
Q4. RLE boundary audit
- Malformed non-empty RLE rows with a run end beyond 102,400: 0 (VERIFY).
- Maximum pixel index observed: 102,400 (VERIFY).
Method: parse each non-empty RLE into start/length pairs and check the run end against the 102,400-pixel canvas using the dataset's indexing convention.
Q5. Distortion caused by resizing to 256 × 256
- Width scale factor: 256 / 800 = 0.32.
- Height scale factor: 256 / 128 = 2.00.
- Distortion ratio: 2.00 / 0.32 = 6.25.
Interpretation: square resizing compresses the horizontal dimension and stretches the vertical dimension, distorting defect shapes. Preserve aspect ratio or use suitable padding/cropping.
Q6. Cohen's d: defect versus local background
 defect_d had the smallest absolute effect size, |d| = 0.020579 (VERIFY).
Interpretation: this effect is close to zero, so local intensity contrast may be weak for this class. Texture- or frequency-based features may help. 
Q7. Dice for empty ground truth
- Empty ground truth and empty prediction: Dice = 1.0 under the specified convention.
- Empty ground truth with 2 false-positive predicted pixels: Dice = 0.0.
Q8. Toy composite metric
For 10 positive pairs (6 with Dice 0.80, 4 with Dice 0.40), and 40 empty pairs (36 predicted empty; 4 with 1,024 false-positive pixels each on a 102,400-pixel canvas):
- Foreground Dice: (6 × 0.80 + 4 × 0.40) / 10 = 0.6400
- Mean false-positive fraction over empty pairs: (4 × 1,024 / 102,400) / 40 = 0.0010
- False-positive score: 1 − 0.0010 = 0.9990
- Empty accuracy: 36 / 40 = 0.9000
- Weighted score: 0.70 × 0.6400 + 0.20 × 0.9990 + 0.10 × 0.9000 = 0.7378
Final toy score: 0.7378.
Q9. Empty-mask prevalence and all-empty predictor
Dataset-specific value: NOT YET VERIFIED.
Count empty image/class pairs and divide by the total number of image/class pairs to obtain the emptiness rate. Under the question's convention, this is the naive empty-aware mean Dice of an all-empty predictor.
Official weighted score for an all-empty predictor:
0.70 × 0 + 0.20 × 1 + 0.10 × 1 = 0.3000.
Q10. Connected-component analysis and area filtering
Dataset-specific values: NOT YET VERIFIED.
Use 8-connectivity on each class-specific ground-truth mask. Report the smallest component for defect_b and calculate the percentage of ground-truth defect pixels removed by the specified area filters. Use measured notebook outputs, not estimates.
Q11. Image-level combination stratification
- Number of unique image-level defect-combination strings: NOT YET VERIFIED.
- defect_b-positive images in the full dataset: 313 (from the assessment instructions).
- 20% holdout target: 313 × 0.20 = 62.6, rounded to 63 images.
Stratify by each image's full defect-label combination, rather than treating individual CSV rows as independent samples. Confirm the actual holdout count in holdout.csv.
Q12. Starter segmentation baseline and threshold sweep
Measured training and threshold-sweep results: NOT YET ADDED.
Run the supplied starter notebook at native resolution for 5–10 epochs. Evaluate thresholds 0.3, 0.5, 0.7, and 0.9 without a minimum-area filter. Add measured results from runs/milestone_1_starter/threshold_sweep.csv, identify the best threshold under the required metric, and include the training history and threshold-sweep plot.
