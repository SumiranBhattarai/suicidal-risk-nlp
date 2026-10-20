# Confusion-matrix forensics

Every figure in the paper was read cell by cell from the extracted images. The metrics
below are recomputed from those cells, not copied from the results table.

| Figure | TN | FP | FN | TP | n | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|---|---|---|---|---|
| figure-2 | 22,041 | 1,251 | 1,656 | 21,448 | 46,396 | 93.73% | 94.49% | 92.83% | 93.65% |
| figure-3 | 21,889 | 1,303 | 1,471 | 21,633 | 46,296 | 94.01% | 94.32% | 93.63% | 93.97% |
| figure-4 | 23,126 | 166 | 22,345 | 759 | 46,396 | 51.48% | 82.05% | 3.29% | 6.32% |
| figure-5 | 21,005 | 2,287 | 3,976 | 19,128 | 46,396 | 86.50% | 89.32% | 82.79% | 85.93% |
| figure-6 | 22,225 | 1,067 | 3,195 | 19,909 | 46,396 | 90.81% | 94.91% | 86.17% | 90.33% |

## Attribution

- **figure-2 → Logistic Regression.** Reconciles to the reported row to two decimals.
- **figure-3 → SVM.** Reconciles within rounding (94.01% computed vs 94.02% reported).
- **figures 4, 5, 6 are unlabelled in the original** and do not match any row of the
  results table exactly. figure-5 is 0.43 pp from the Simple NN; figure-6 is 1.35 pp
  from Random Forest; figure-4 matches nothing.

## Two discrepancies worth recording

1. **The SVM matrix was computed on a different test set.** Four figures total 46,396
   rows; figure-3 totals 46,296. The positive class is 23,104 in all five, so the
   difference is 100 negative-class rows. Small, and it changes no conclusion, but
   "same split, fair comparison" is true at the design level rather than at the
   artefact level.
2. **Six models, five matrices.** One model has no confusion matrix in the published
   figure set.

## The figure that argues the case

figure-4: accuracy 51.48%, recall 3.29%.

The base rate on this test set is 50.20% (23,292 of 46,396 are class 0). A script that
answers "not at risk" to every post scores 50.20%. figure-4 scores 51.48% — **1.3
percentage points above predicting nothing**.

It still looks like a working model in a heatmap, because the eye reads a dark
diagonal. figure-4's dark cells are both in the left column: the model answered "0"
almost always. A dark column and a dark diagonal are indistinguishable at thumbnail
size.

That is the clearest evidence in the paper that a single accuracy number is not a
description of a classifier.
