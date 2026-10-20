# Suicidal Risk Identification in Social Media Text

Detection of suicidal ideation in Reddit posts using classical NLP and a controlled
comparison of six classifiers.

**Published:** IEEE ICSC 2023 (9th International Conference on Signal Processing and
Communication), Majitar, Sikkim.
**DOI:** [10.1109/ICSC60394.2023.10441398](https://doi.org/10.1109/ICSC60394.2023.10441398)
**Indexed:** Scopus

Authors: Shanskar Rai, Sumiran Bhattarai, Premjyoti Dhar, Biraj Upadhyaya,
Kalpana Sharma, Sandeep Gurung.

---

## Why this problem

Sikkim recorded a suicide rate of 20.2 per 100,000 against a national average of 10.4
(NCRB 2019). This was not an abstract dataset exercise.

The question: can a cheap, explainable text classifier identify posts that indicate
suicidal ideation, and can we state honestly how well it works?

---

## Data

| | |
|---|---|
| Source | Kaggle, "Suicide and Depression Detection" |
| Size | 232,074 labelled Reddit posts |
| Split | 80 / 20, `random_state=42` |

Text was lowercased, stripped of HTML, punctuation and numbers, stopword-removed,
lemmatised, and filtered of non-English fragments. Features were TF-IDF (bag-of-words
was also computed).

---

## Models and results

Six models, one split, one pipeline, so the comparison is like for like.

| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| **SVM** | **0.9402** | 0.9431 | 0.9363 | 0.9397 |
| Logistic Regression | 0.9373 | **0.9448** | 0.9283 | 0.9365 |
| CNN (Conv1D, 128 filters, kernel 5, GloVe 100-d) | 0.9209 | 0.9165 | 0.9256 | 0.9210 |
| Random Forest | 0.8946 | 0.8891 | 0.9022 | 0.8956 |
| Simple NN (GloVe 100-d) | 0.8693 | 0.8768 | 0.8582 | 0.8674 |
| KNN | 0.6248 | 0.7073 | 0.6120 | 0.5931 |

## What the accuracy column hides

The best model's confusion matrix, recomputed from the published figure:

| | Predicted 0 | Predicted 1 |
|---|---|---|
| **True 0** | 21,889 | 1,303 |
| **True 1** | **1,471** | 21,633 |

**1,471 at-risk posts were classified as not at risk** — 6.4% of the positive class.
On a near-balanced test set, accuracy weighs that the same as a false alarm. In this
problem it does not: a false positive is an unnecessary check-in, a false negative is
someone who was never seen.

The operating point should be chosen by **recall at an acceptable precision**, not by
whichever model tops the accuracy column.

## Two findings worth more than the headline

**The deep model lost.** A Conv1D scored 1.9 points below a linear SVM. With sparse,
very high-dimensional TF-IDF features and 232k samples, the class boundary is close to
linear; a linear SVM finds it directly, while the CNN has to learn what a keyword
n-gram is from scratch. Extra capacity bought variance, not signal. That sets an honest
floor for how far cheap models get before a transformer is needed.

**KNN collapsed to 62%.** In tens of thousands of dimensions, distances concentrate
until every point is nearly equidistant from every other. The one purely distance-based
model degrades first. Curse of dimensionality, observed rather than recited.

---

## Repository contents

- `code/` — the notebooks from the project: preprocessing, transformation, and the
  SVM, logistic regression and random forest runs
- `figures/` — the pipeline diagram and the five confusion matrices from the paper
- `analysis/confusion-matrix-forensics.md` — every matrix decoded cell by cell, with
  metrics recomputed from the cells and the discrepancies with the published table
  written down

**Not included:** the CNN and KNN notebooks, and the processed CSVs (about 115 MB;
they are rebuilt by running `code/preprocessing.ipynb` against the Kaggle corpus).
The notebooks are published as they were written in 2023 — unpolished, and they assume
`Output.csv`, `TestSet.csv` and the preprocessed CSVs sit beside them.

## Limitations

- **Within-dataset result.** The corpus is stylistically separable: support-community
  posts differ from ordinary posts in vocabulary, length and tone, partly because of
  which subreddit they came from. 94% is not a real-world claim.
- **Class imbalance was handled weakly.** Aggregate metrics only; stratified splits and
  class weights would be the fix.
- **English-only, Reddit-only.** No cross-platform or dialect validation.
- **Ethics.** Decision support, never diagnosis, never an automatic action. Human in
  the loop, crisis resources beside any flag, no silent flagging of individuals.

## Citation

```bibtex
@inproceedings{rai2023suicidal,
  title     = {Analyzing Social Media Texts for Suicidal Risk Identification using Natural Language Processing},
  author    = {Rai, Shanskar and Bhattarai, Sumiran and Dhar, Premjyoti and Upadhyaya, Biraj and Sharma, Kalpana and Gurung, Sandeep},
  booktitle = {2023 9th International Conference on Signal Processing and Communication (ICSC)},
  year      = {2023},
  doi       = {10.1109/ICSC60394.2023.10441398}
}
```
