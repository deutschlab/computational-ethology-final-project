# Part 1 – Supervised Learning: Telling Fly Populations Apart

[← Back to the main page](../README.md) · Starter notebook: [`starter_supervised.ipynb`](starter_supervised.ipynb)

General guidelines, submission and grading are on the [main page](../README.md#general-guidelines-both-parts).
In short, upload four files:

| File | Content |
|---|---|
| `SupervisedLearning_<your names>.ipynb` | Your notebook, saved with its outputs |
| `SupervisedLearning_<your names>.html` | The same notebook exported to HTML |
| `blind_test_prediction.csv` | 120,000 rows, column `prediction` (0/1) – see Task 7 |
| `blind_test_short_prediction.csv` | 800 rows, column `prediction` (0/1) – see Task 7 |

---

## The data

You will analyze videos of **pairs of fruit flies** (one male and one female). The videos were tracked with **SLEAP** (Social LEAP Estimates Animal Poses), which gives the (x, y) pixel position of 13 body parts of each fly in every frame.

Each row is one video frame. Each pair of flies belongs to one of two populations, labeled **0** and **1**.
You are not told what the two populations are: do not assume anything about them beyond what the data show.

### Files

All files are in [`data/`](data/). The `.csv.gz` files are compressed; read them directly with `pd.read_csv("data/train.csv.gz")`.

| File | Rows | Labels | Use it for |
|---|---|---|---|
| `train.csv.gz` | 240,000 (1,600 chunks) | yes, column `Label` | Training your models |
| `test.csv.gz` | 120,000 (800 chunks) | yes, column `Label` | Evaluating your models on data they have not seen |
| `blind_test.csv.gz` | 120,000 (800 chunks) | **no** | Your final predictions (Task 7) |
| `train_short_labels.csv` | 1,600 | column `labels` | One label per 150-row chunk of the training set |
| `test_short_labels.csv` | 800 | column `labels` | One label per 150-row chunk of the test set |

Both classes are equally common: half of the rows (and chunks) in each labeled file have label 0, half have label 1.

### Columns

Every file starts with an **unnamed first column that only holds the row number**. It is not a feature.

The next **52 columns** are the features: 13 body parts × 2 coordinates (x, y) × 2 flies (male, female) = 52.
Column names follow the pattern `<fly>_<axis>_<bodypart>`:

| Part of the name | Values |
|---|---|
| fly | `M` = male, `F` = female |
| axis | `X`, `Y` (pixels in the video image) |
| body part | `head`, `thorax`, `abdomen`, `wingL`, `wingR`, `forelegL4`, `forelegR4`, `midlegL4`, `midlegR4`, `hindlegL4`, `hindlegR4`, `eyeL`, `eyeR` |

For example, `F_Y_thorax` is the y position of the female's thorax. `L`/`R` mean left/right.

`train` and `test` have one more column at the end, **`Label`** (0 or 1). `blind_test` has no `Label` column.

### Chunks of 150 frames

- The rows come in **chunks of 150 consecutive rows**. A chunk is 1 second of video recorded at 150 frames per second.
- Rows 0–149 are chunk 0, rows 150–299 are chunk 1, and so on (chunk number = row number // 150).
- All 150 rows of a chunk come from the same pair of flies, so **the label is the same for the whole chunk**.
- Consecutive chunks are **not** consecutive in time: the chunks have been shuffled. Never compute a temporal feature across the boundary between two chunks.
- The **short labels** files contain one label per chunk, in chunk order.

### Fly pairs

- The training set and the test/blind sets contain **different pairs of flies**.
- **Within** each set, the same pair appears many times, in chunks from different parts of its video. Chunks from the same pair are therefore similar to each other. Keep this in mind when you split and evaluate your data (Tasks 4, 5 and 8).

---

## Tasks

### 1. Data Cleaning (2 points)

1. Check for outliers or possible missing values in the dataset. (1 pt)
2. Describe the identification process for outliers. (1 pt)

Interpolation of outliers is not required.

### 2. Create Features (10 points)

1. Create at least **five features** from the raw data (for example speeds, distances or angles; explain what each feature is meant to capture). At least one is **temporal**. (6 pts)
2. Use at least **two different window sizes** for a temporal feature. (2 pts)
3. Show a temporal feature with the two window sizes **on the same graph**. For example, plot the feature against the frame number for one 150-row chunk, with one line per window size. (2 pts)

### 3. Visual Testing of Features by Label (3 points)

1. Make graphs to test visually whether your features differ between the labels. Make sure the graphs are well labeled and explain each graph. (1 pt)
2. Which features are likely to be better predictors? Explain your answer using evidence from your graphs, and **state what accuracy you expect** from the models before you train them. (2 pts)

### 4. Train and Evaluate Models on the Training Set (3 points)

1. Perform an **80%/20% split of the rows** of the training set (a random split, for example with `train_test_split` from scikit-learn) and apply two machine learning algorithms: **Random Forest** and one more algorithm of your choice. (1 pt)
2. Include a **confusion matrix** and the **accuracy** for each model, and say which type of mistake each model makes more often. (2 pts)
3. If your temporal features leave some rows without a value (for example at the beginning of each window), explain what you did with those rows. Make sure that a window never runs across the boundary between two chunks.

### 5. Evaluation Using the Full Training and Test Data (3 points)

1. Apply the models and performance measures from Task 4, but now **train on the whole training set and evaluate on the test set**. Display the results for comparison. (2 pts)
2. Compare these results with the results of Task 4 and explain possible differences in performance. *Hint: think about how similar the rows within the training set are to each other, and whether the same is true between the training set and the test set.* (1 pt)

### 6. Enhanced Preprocessing (2 points)

Re-apply the models (on the training and test data) with additional preprocessing that matches the **short labels**: one row and one label per 150-row chunk, for example by summarizing each feature over the chunk with its mean and standard deviation. Display the results for comparison. (2 pts)

### 7. Blind Test Prediction (2 points)

Apply the preprocessing of Task 6 to the blind test set and predict its labels with your Random Forest model. Submit **two prediction files**:

| File | Rows | From |
|---|---|---|
| `blind_test_short_prediction.csv` | 800, one per chunk | Your chunk-level model of Task 6 |
| `blind_test_prediction.csv` | 120,000, one per row of `blind_test.csv.gz` | Your row-level model of Task 5 |

- Each file must have a column named **`prediction`** with the values 0 or 1, **in the same order as the blind test data**.
- Train your final models on the **training set only** (not on the test set), and state in your notebook which data each model was trained on.
- Check both files with the last cell of the starter notebook before you submit.

(2 pts)

### 8. Split by Chunk (5 points)

1. Repeat the 80%/20% split of Task 4 so that **all 150 rows of a chunk end up on the same side of the split** (for example with `GroupShuffleSplit` from scikit-learn, using the chunk number as the group). Report the accuracy and compare it with the row-by-row split.
2. Explain the difference.
3. The chunk split will probably still give a higher accuracy than the test set in Task 5. Suggest at least two possible reasons.

*Why we ask:* rows from the same chunk are almost identical. If a chunk is split between training and testing, the model is tested on data it has effectively already seen, and the accuracy is inflated.

### 9. Cross-Validation (5 points)

1. Use **5-fold cross-validation** and report the mean and standard deviation of the accuracy across the folds. Keep all rows of a chunk in the same fold (for example with `GroupKFold`).
2. Is the model performance stable across folds?
3. Compare the cross-validation mean with the test accuracy of Task 5. Does a stable result across folds mean that the model will work equally well on the test set? Explain.

*Why we ask:* a single split gives one number that depends on luck. Cross-validation shows how much the result changes with different data, so you know how much to trust it.

### 10. What Is the Model Using? (5 points)

1. For your Random Forest, show the **importance of every feature** in a bar plot (for example with the `feature_importances_` attribute).
2. Look at the most important feature. Does it describe **behavior** (what the flies do), or could it reflect a **physical property** of the flies or of the recording (for example body size or position in the arena)? Explain.
3. Retrain the Random Forest **without that feature** and report the accuracy on the test set. How much did it change, and what does that tell you about the model?

*Why we ask:* a model can be accurate for the wrong reason. Checking what it relies on is a first step in deciding whether to trust it.

### 11. How Much Can You Trust Your Numbers? (5 points)

1. What accuracy would you get by guessing (the **chance level**) on the test set? How far above it are your models?
2. The test set has 120,000 rows but only 800 chunks, and the rows of a chunk are almost identical, so the number of independent examples is closer to 800. Estimate the uncertainty of your test accuracy with the **standard error**

   SE = √( p (1 − p) / n )

   where *p* is the accuracy and *n* = 800. A range of about ±2 SE contains the true accuracy with about 95% confidence.
3. Are the differences between your models larger than this uncertainty? Which conclusions in your notebook are affected?

*Why we ask:* two models with 71% and 72% accuracy are usually not really different. Knowing how large a difference must be to matter is part of interpreting results.

### 12. Summary: Strengths and Limitations (5 points)

Write a short summary (about 6–10 sentences, in a markdown cell) that answers:

- (a) How well does your final model work, and how do you know?
- (b) Where does it make mistakes? (Use your confusion matrix.)
- (c) Which of your evaluation numbers do you trust most and which least, and why?
- (d) What additional data or information would help you evaluate it better?

*Why we ask:* this is the part that shows that you understand your work and not only that you ran it.
