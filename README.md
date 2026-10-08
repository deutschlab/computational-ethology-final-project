# Final Project – Machine Learning for Behavior

**Quantifying Natural Behavior – The Road Toward Computational Neuroethology**
Summer School, University of Haifa · October 4–8, 2026
Coordinators: Dr. Lilach Avitan (Hebrew University) · Dr. David (Dudi) Deutsch (University of Haifa)

**Questions about the project? Contact Aviad Sivan: [asivan08@campus.haifa.ac.il](mailto:asivan08@campus.haifa.ac.il)**

Welcome! This repository contains everything you need for the final project of the summer school:
the instructions, the data, and starter notebooks that load the data for you.

The project has two parts. You will work with real data from neuroscience labs:

| Part | Data | What you do | Instructions |
|---|---|---|---|
| **1. Supervised learning** | Pose tracking (SLEAP) of pairs of fruit flies: 13 body parts per fly, 150 frames per second | Build features from the raw poses, train classifiers that tell two populations of fly pairs apart, and find out how far you can trust the result | [supervised/README.md](supervised/README.md) |
| **2. Unsupervised learning** | Electrophysiology from rats in social experiments: 62 samples × 8 anonymous features | Reduce dimensions (PCA, t-SNE, UMAP), cluster the samples, and decide whether the clusters are real | [unsupervised/README.md](unsupervised/README.md) |

The same instructions as printable PDFs: [Supervised](docs/SupervisedLearningAssignment.pdf) · [Unsupervised](docs/UnsupervisedLearningAssignment.pdf)

**SLEAP exercise:** [raw SLEAP output files](sleap_exercise/README.md) of two fly experiments, to see where pose tables like ours come from and to practice working with the `.h5` files directly.

> **Key dates**
> - **Submission deadline: November 17, 2026** (both parts)
> - **Online meetings about your work: November 25–26, 2026** (Google Meet). Book your slot by **November 20**.
>
> **Submit here:** [submission form](https://docs.google.com/forms/d/e/1FAIpQLSfs2qfkkCtoy6zOEEFKLV6E8L24RO0alo2U9gMcg4PfKKDoyA/viewform) · **Book your meeting here:** [booking page](https://calendar.app.google/t4j3KoTfcUXpawDEA)

---

## Contents

1. [Getting started](#getting-started)
2. [What is in this repository](#what-is-in-this-repository)
3. [General guidelines (both parts)](#general-guidelines-both-parts)
4. [What to submit](#what-to-submit)
5. [Grading and the online meeting](#grading-and-the-online-meeting)
6. [Tips and common pitfalls](#tips-and-common-pitfalls)
7. [Contact](#contact)

---

## Getting started

You can work in **Google Colab** (nothing to install) or on **your own computer**.

### Option A – Google Colab (easiest)

Click a badge to open a starter notebook in Colab:

| Part | Starter notebook |
|---|---|
| Supervised | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/deutschlab/computational-ethology-final-project/blob/main/supervised/starter_supervised.ipynb) |
| Unsupervised | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/deutschlab/computational-ethology-final-project/blob/main/unsupervised/starter_unsupervised.ipynb) |

The first cell of each notebook downloads this repository, including the data, into your Colab session.
Then **File → Save a copy in Drive** so your work is kept.

> UMAP is not pre-installed in Colab. Run `!pip install umap-learn` once in a cell before you use it.

### Option B – Your own computer

```bash
git clone https://github.com/deutschlab/computational-ethology-final-project.git
cd computational-ethology-final-project
pip install -r requirements.txt
jupyter notebook
```

Then open `supervised/starter_supervised.ipynb` or `unsupervised/starter_unsupervised.ipynb`.

No git? Use the green **Code → Download ZIP** button on this page and unzip it.

### Reading the data

The large supervised files are compressed (`.csv.gz`) so that they fit on GitHub.
**You do not need to unzip them.** pandas reads them directly:

```python
import pandas as pd
train = pd.read_csv("supervised/data/train.csv.gz")
```

---

## What is in this repository

```
.
├── README.md                      ← you are here
├── requirements.txt               ← Python packages for running locally
├── docs/                          ← the task instructions as PDF files
├── supervised/
│   ├── README.md                  ← supervised task: data description + tasks
│   ├── starter_supervised.ipynb   ← loads the data, explains the columns, checks your output files
│   └── data/
│       ├── train.csv.gz           ← 240,000 frames, with labels
│       ├── test.csv.gz            ← 120,000 frames, with labels
│       ├── blind_test.csv.gz      ← 120,000 frames, NO labels (you predict them)
│       ├── train_short_labels.csv ← one label per 150-frame chunk (1,600)
│       └── test_short_labels.csv  ← one label per 150-frame chunk (800)
├── unsupervised/
│   ├── README.md                  ← unsupervised task: data description + tasks
│   ├── starter_unsupervised.ipynb ← loads the data, checks your output file
│   └── data/
│       └── electrophysiology_data.csv  ← 62 samples × 8 features
└── sleap_exercise/                ← SLEAP exercise: raw .h5 files (README, download links, how to read them)
```

The starter notebooks only load and describe the data. They do not solve any task.
You can build your own notebook on top of them.

---

## General guidelines (both parts)

Each part is submitted as a **Jupyter notebook**. We read your notebook like a short report, so it should be easy to follow.

**Notebook structure**

- Organize the notebook into clear sections with **headers and subheaders**.
- **Graphs:** every graph has a title, x- and y-axis labels, and suitable scales. Say which color stands for what (a legend or a sentence).
- After each graph, add a **markdown cell** that explains what the graph shows and why it matters for your analysis.
- Use at least one **ordered or unordered list** in a markdown cell.
- *Supervised part only:* include at least one **formula** that you actually use (for example accuracy, a standard error or a distance) and explain in words what each part means. Any notation is fine: plain text, or LaTeX in a markdown cell.

**Working in pairs**

- The project is done in **pairs** (two students per submission).
- Put both names in the first cell of each notebook and submit once per pair.
- If you have a problem finding a partner, or any other problem with working in a pair, contact Aviad Sivan ([asivan08@campus.haifa.ac.il](mailto:asivan08@campus.haifa.ac.il)) as early as possible.

**AI assistants**

You may use AI assistants (for example ChatGPT, Claude or Copilot) to help you write code.
You are responsible for understanding every step. In the online meeting we will ask how you solved the task, why you chose your methods, what your results mean and what their limitations are.
**We grade your understanding of both the code and the models: you must be able to explain the code you submit, and above all what your models do and what the results mean.**

**Methods beyond the course**

Tools that were not taught in the course are fine. If you use one, explain it so that a first-year engineering or science student could follow it.

---

## What to submit

| Part | Files to upload | Rows |
|---|---|---|
| Supervised | `SupervisedLearning_<your names>.ipynb`, saved **with its outputs** | |
| | The same notebook exported to **HTML** | |
| | `blind_test_prediction.csv` (column `prediction`, values 0/1, one row per row of `blind_test.csv.gz`) | 120,000 |
| | `blind_test_short_prediction.csv` (column `prediction`, values 0/1, one row per 150-frame chunk) | 800 |
| Unsupervised | `UnsupervisedLearning_<your names>.ipynb`, saved **with its outputs** | |
| | The same notebook exported to **HTML** | |
| | `electrophysiology_data_kmeans.csv` (the 8 feature columns + `kmeans_labels`, no index column) | 62 |

**What the csv files should look like, in words:** the two prediction files are plain csv files with a header row and a single column named `prediction`, containing 0 or 1 on every row, in the order of the blind test data, and nothing else (no row numbers, no features). The k-means file is the original data table (the 8 feature columns, original unscaled values, original row order, no row-number column) with one extra last column `kmeans_labels` holding the cluster number of each sample. The starter notebooks show the first lines of each file, the code that saves it, and a checking cell that tells you what to fix if something is off. If the checker reports a problem you cannot fix, submit anyway and explain it in a sentence in your notebook or in an email.

Before you submit:

1. Run **Kernel → Restart & Run All** (Colab: **Runtime → Restart session and run all**) once, so the notebook runs from the first cell to the last and the saved outputs match the code.
2. Export to HTML: **File → Download as → HTML** (Jupyter), or **File → Download → Download .ipynb** in Colab and convert with `jupyter nbconvert --to html <notebook>.ipynb`.
3. Check your csv files with the checking cell at the end of each starter notebook.
4. **Do not upload the data files.**

**Upload to:** [the submission form](https://docs.google.com/forms/d/e/1FAIpQLSfs2qfkkCtoy6zOEEFKLV6E8L24RO0alo2U9gMcg4PfKKDoyA/viewform). You need to be signed in to a Google account to upload files. The form also asks for the student ID numbers of both partners.
Need to replace a file before the deadline? Use the **"Edit response"** link on the confirmation page or in the emailed copy of your response, instead of submitting a second time.
**Deadline:** November 17, 2026

---

## Grading and the online meeting

- Most of the grade is for **evidence that you understand and apply what you learned**: explain why you chose each method and show visual evidence for your explanations.
- A high accuracy is a bonus, not the goal. **A modest result that you understand and report honestly is worth more than a high score you cannot explain.**
- After you submit, we will meet each of you **online (Google Meet)** on **November 25–26, 2026** for about 15 minutes and ask about your work. Be ready to share your screen and walk us through your notebook.
- **Book your meeting yourself** on the [booking page](https://calendar.app.google/t4j3KoTfcUXpawDEA) after you submit, and no later than November 20.
- **Each partner books their own meeting.**
- The booking confirmation and calendar invitation contain the Google Meet link. To change your time, cancel with the link in the confirmation email and book another free slot.

| Part | Points |
|---|---|
| Supervised | Tasks 1–7: 25 points · Tasks 8–12: 5 points each (25) · total 50 |
| Unsupervised | Tasks 1–2: 50 + 50 points · Tasks 3–7: 5 points each (25) · total 125 |

The point values for each task are given in the task pages.

---

## Tips and common pitfalls

- **Read the data description first.** Both task pages explain how the data are organized. The supervised data come in **chunks of 150 consecutive frames**, and this matters for almost every task.
- **Look before you model.** Plot the raw data before you train or cluster anything.
- **Write down what you expect before you see the result.** It helps you notice when a number is suspiciously good.
- **Fix the random seed** (`random_state=...`) so that your notebook gives the same numbers every time it runs.
- **Error messages are information.** Read them. Some tasks ask you to explain one.
- **Memory:** the supervised training file has 240,000 rows. If your computer is slow, develop your code on part of the data and run it on everything at the end.

---

## Contact

Questions about the project: Aviad Sivan, [asivan08@campus.haifa.ac.il](mailto:asivan08@campus.haifa.ac.il)

Course coordinator: Dr. David (Dudi) Deutsch, ddeutsch1@univ.haifa.ac.il

<!--
Notes for instructors (not rendered on GitHub):
- If the repository moves or is renamed, update its address in this file (Colab badges, git clone) and REPO_URL in both starter notebooks.
- For a new year: update the course dates at the top, the key dates (deadline, online meetings, booking deadline; also under "What to submit" and "Grading") and the points.
- The submission form, its responses sheet and the booking page (a Google Calendar appointment schedule: 30-minute slots, one per student, Google Meet) belong to the person running the project that year. Create them in your own Google account (all in the same account, since uploads go to the form owner's Drive) and replace the form link, the booking link and the contact email.
-->
