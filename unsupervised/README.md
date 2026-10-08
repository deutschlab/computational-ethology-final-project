# Part 2 – Unsupervised Learning: Finding Structure in Neural Data

[← Back to the main page](../README.md) · Starter notebook: [`starter_unsupervised.ipynb`](starter_unsupervised.ipynb)

General guidelines, submission and grading are on the [main page](../README.md#general-guidelines-both-parts).
In short, upload three files:

| File | Content |
|---|---|
| `UnsupervisedLearning_<your names>.ipynb` | Your notebook, saved with its outputs |
| `UnsupervisedLearning_<your names>.html` | The same notebook exported to HTML |
| `electrophysiology_data_kmeans.csv` | 62 rows: the 8 feature columns + `kmeans_labels`, without the index column – see Task 2 |

The csv file is checked for **format and consistency with your notebook**. Cluster numbers are arbitrary, so there is no single correct file to compare with.

---

## The data

The data come from **rats in dyadic (two-animal) social experiments**. Electrophysiological signals were recorded from brain areas that are important for social behavior, and features were extracted from them. For this assignment the data were cleaned and missing values were filled in (imputed).

| | |
|---|---|
| File | [`data/electrophysiology_data.csv`](data/electrophysiology_data.csv) |
| Samples (rows) | 62 |
| Features | 8, anonymous: `feature_1` … `feature_8` |
| Labels | none |

The file also has an **unnamed first column with the row number**. It is not a feature: remove it before your analysis.

---

## Tasks

**Preprocessing (all tasks):** remove the unnamed index column and work with the **standardized** features (mean 0, standard deviation 1; see Task 3), unless a task asks you to compare with the unscaled data.

### 1. Dimensionality Reduction (50 points)

1. Reduce the data to **2 dimensions** with t-SNE, UMAP and PCA, using these parameters, and show each result as a **scatter plot**:

   | Method | Parameters | Plots |
   |---|---|---|
   | t-SNE | perplexity: 5 and 50 (perplexity must be smaller than the number of samples, 62) | 2 |
   | UMAP | `n_neighbors`: 2 and 10 × `min_dist`: 0.1 and 1.0 (`min_dist` cannot be larger than the UMAP parameter `spread`, whose default is 1) | 4 |
   | PCA | default | 1 |

2. Explain how the parameters affect the result. Compare your plots: what stays similar across the settings, and what changes?
3. Also try t-SNE with **perplexity 100** and UMAP with **min_dist 10**. Read the error messages and explain, in your own words, why these settings do not work for this data set.

> In Colab, install UMAP first: `!pip install umap-learn`, then `import umap`.

### 2. Clustering (50 points)

1. Cluster the data with **KMeans**, **AgglomerativeClustering**, and **one more method** of your choice (for example Gaussian Mixture or DBSCAN). Explain why you chose it and how you set its parameters.
2. For KMeans, find the best number of clusters with the **Elbow method**:
   1. Fit 10 KMeans models with 1 to 10 clusters.
   2. Extract the inertia of each fitted model.
   3. Plot the elbow plot.
3. Add a column named **`kmeans_labels`** with the KMeans cluster labels. Use the number of clusters that you chose (state it and explain your choice). Save the data frame, **without** the unnamed index column and with the new column, as `electrophysiology_data_kmeans.csv`.

### 3. Feature Scaling (5 points)

1. Standardize the 8 features (mean 0, standard deviation 1) before running PCA, t-SNE, UMAP and the clustering.
2. Compare the PCA plot **with and without** scaling. Does the picture change?
3. Compare the elbow plot with and without scaling. Which one looks clearer, and is the clearer one also the more trustworthy? Explain.

*Why we ask:* without scaling, the feature with the largest numbers dominates every distance and every principal component.

### 4. Silhouette Score (5 points)

1. For KMeans with 2 to 10 clusters, compute the **silhouette score** and plot it against the number of clusters, next to the elbow plot.
2. Do the two methods agree on the number of clusters?
3. Repeat the KMeans fits with a different `random_state`. Does the best number of clusters change? What does that tell you about how much you can trust the choice of k? (It is a valid result to conclude that the data do not show one clear number of clusters.)

*Why we ask:* the silhouette score describes how well each point fits its own cluster compared with the nearest other cluster (higher is better). It is useful because the bend of the elbow plot is not always obvious.

### 5. Cluster Profiles (5 points)

1. After choosing the number of clusters, show the **mean of each original feature per cluster** (a table or a bar plot).
2. Because the features are on very different scales, also compare **standardized** means (for example in a heatmap), and report **how many samples** each cluster contains.
3. Describe each cluster in one sentence using the original features.
4. Are there very small clusters (2–3 samples)? What could they be?

*Why we ask:* looking at the original features per cluster tells you what the clusters are beyond their color.

### 6. Are the Clusters Real? (5 points)

1. Create a **control data set** by shuffling every column of your standardized data independently. This keeps each feature's values but destroys the relationships between features. Run KMeans on it with the same number of clusters, compute the silhouette score, and repeat a few times.
2. Compare these scores with the score of the real data. What do you conclude about the clusters in the real data? There is no single right answer: explain what this comparison does and does not show.

*Why we ask:* a clustering algorithm always returns clusters, even for data without any structure. A control shows what a "no structure" result looks like.

### 7. Summary: Strengths and Limitations (5 points)

Write a short summary (about 6–10 sentences, in a markdown cell) that answers:

- (a) What structure, if any, did you find in the data, and how confident are you?
- (b) Which of your results depend on your choices (scaling, method, number of clusters, random seed), and which are stable?
- (c) What would you need (more samples, labels, knowing what the features are) to be sure?

*Why we ask:* this is the part that shows that you understand your work and not only that you ran it.
