# Customer Segmentation with K-Means Clustering

## Project Overview

This project focuses on customer segmentation using the **Mall Customers** dataset.

The main objective is to identify different groups of customers based on their purchasing behavior using **unsupervised learning**, specifically the **K-Means clustering algorithm**.

The segmentation is mainly based on two variables:

- `Annual_Income`: the customer's annual income;
- `Spending_Score`: the customer's spending score.

This type of analysis can help businesses better understand their customers and design more targeted marketing strategies.

---

## Dataset

The dataset used in this project is the **Mall Customers** dataset.

It contains **200 customers** and **5 columns**:

| Column | Description |
|---|---|
| `CustomerID` | Unique customer identifier |
| `Gender` | Customer gender |
| `Age` | Customer age |
| `Annual_Income` | Annual income in thousands of dollars |
| `Spending_Score` | Spending score between 1 and 100 |

The `CustomerID` column was not used for clustering because it is only an identifier and does not provide useful behavioral information.

---

## Project Objective

The main goal of this project is to answer the following question:

> Can we identify meaningful customer groups based on annual income and spending score?

This segmentation can help a business to:

- understand customer behavior;
- identify high-value customers;
- detect customers with strong business potential;
- personalize marketing campaigns;
- adapt offers according to customer profiles.

---

## Exploratory Data Analysis

An initial exploratory data analysis was performed to understand the structure of the dataset.

### Main Statistics

| Variable | Minimum | Mean | Maximum |
|---|---:|---:|---:|
| `Age` | 18 | 38.85 | 70 |
| `Annual_Income` | 15 | 60.56 | 137 |
| `Spending_Score` | 1 | 50.20 | 99 |

The dataset contains:

- 200 rows;
- 5 columns;
- no missing values;
- no duplicated rows.

---

## Correlation Analysis

A correlation matrix was computed to study the linear relationships between the numerical variables.

The main correlation values are:

| Variables | Correlation |
|---|---:|
| `Age` / `Annual_Income` | -0.012 |
| `Age` / `Spending_Score` | -0.327 |
| `Annual_Income` / `Spending_Score` | 0.010 |

The correlation between `Annual_Income` and `Spending_Score` is almost zero.

However, the absence of a strong linear correlation does not mean that there is no structure in the data.

Correlation measures a global linear relationship between variables, while clustering looks for groups of observations that are close to each other.

In this project, even though `Annual_Income` and `Spending_Score` are not linearly correlated, the scatter plot shows several distinct groups of customers.

---

## Features Used for Clustering

For the first clustering model, two variables were selected:

| Feature | Role |
|---|---|
| `Annual_Income` | Represents the customer's income level |
| `Spending_Score` | Represents the customer's spending behavior |

These two variables were chosen because they allow a clear two-dimensional visualization of the customer segments.

---

## Data Preprocessing

Before applying K-Means, the selected variables were scaled using `StandardScaler`.

This step is important because K-Means is a distance-based algorithm.

Without scaling, a variable with a larger range could have a stronger influence on the clustering result.

After standardization, both variables have:

- a mean close to 0;
- a standard deviation close to 1.

---

## Choosing the Number of Clusters

Two methods were used to choose the number of clusters:

1. the Elbow Method;
2. the Silhouette Score.

---

## Elbow Method

The Elbow Method consists of testing several values of `k` and observing the evolution of inertia.

Inertia measures how compact the clusters are.  
The lower the inertia, the closer the points are to their cluster centers.

### Inertia Results

| k | Inertia |
|---:|---:|
| 1 | 400.000 |
| 2 | 269.691 |
| 3 | 157.704 |
| 4 | 108.921 |
| 5 | 65.568 |
| 6 | 55.057 |
| 7 | 44.865 |
| 8 | 37.228 |
| 9 | 32.392 |
| 10 | 29.982 |

The inertia decreases strongly until `k = 5`.  
After that, the decrease becomes much smaller.

Therefore, the Elbow Method suggests that the best number of clusters is:

```text
k = 5
```

---

## K-Means Model

The final K-Means model was trained with the following parameters:

```python
KMeans(n_clusters=5, random_state=42, n_init=10)
```

Parameter explanation:

| Parameter | Meaning |
|---|---|
| `n_clusters=5` | The model creates 5 clusters |
| `random_state=42` | Ensures reproducible results |
| `n_init=10` | Runs K-Means several times and keeps the best result |

---

## Final Clustering Results

The K-Means model identified **5 customer segments**.

### Number of Customers per Cluster

| Cluster | Number of Customers |
|---:|---:|
| 0 | 81 |
| 1 | 39 |
| 2 | 22 |
| 3 | 35 |
| 4 | 23 |

The largest cluster is **Cluster 0**, with 81 customers.

---

## Average Profile of Each Cluster

| Cluster | Average Age | Average Annual Income | Average Spending Score | Number of Customers |
|---:|---:|---:|---:|---:|
| 0 | 42.72 | 55.30 | 49.52 | 81 |
| 1 | 32.69 | 86.54 | 82.13 | 39 |
| 2 | 25.27 | 25.73 | 79.36 | 22 |
| 3 | 41.11 | 88.20 | 17.11 | 35 |
| 4 | 45.22 | 26.30 | 20.91 | 23 |

The `Age` variable was not used to build the clusters.  
It was only used after clustering to better interpret the customer profiles.

---

## Cluster Centroids

The centroids represent the average position of each cluster.

| Cluster | Centroid Annual Income | Centroid Spending Score |
|---:|---:|---:|
| 0 | 55.30 | 49.52 |
| 1 | 86.54 | 82.13 |
| 2 | 25.73 | 79.36 |
| 3 | 88.20 | 17.11 |
| 4 | 26.30 | 20.91 |

These centroids confirm that the customer groups are mainly structured around two dimensions:

- income level;
- spending behavior.

---

## Business Interpretation of the Clusters

The clusters were renamed to make their interpretation easier from a business perspective.

| Cluster | Segment Name | Description |
|---:|---|---|
| 0 | Standard / Balanced Customers | Customers with average income and average spending score |
| 1 | High-Value Premium Customers | Customers with high income and high spending score |
| 2 | Young Low-Income Big Spenders | Young customers with low income but high spending score |
| 3 | Wealthy but Careful Customers | Customers with high income but low spending score |
| 4 | Low-Income Conservative Customers | Customers with low income and low spending score |

---

## Detailed Segment Analysis

### Cluster 0 — Standard / Balanced Customers

This cluster contains **81 customers**.

Average profile:

- average age: 42.72 years;
- average annual income: 55.30k$;
- average spending score: 49.52.

These customers have a balanced profile.  
They represent the largest group in the dataset and can be considered a stable customer base.

---

### Cluster 1 — High-Value Premium Customers

This cluster contains **39 customers**.

Average profile:

- average age: 32.69 years;
- average annual income: 86.54k$;
- average spending score: 82.13.

This is one of the most valuable customer segments.

These customers have both high income and high spending behavior.

They could be targeted with:

- premium offers;
- loyalty programs;
- personalized services;
- exclusive benefits.

---

### Cluster 2 — Young Low-Income Big Spenders

This cluster contains **22 customers**.

Average profile:

- average age: 25.27 years;
- average annual income: 25.73k$;
- average spending score: 79.36.

This segment includes younger customers who spend a lot despite having a lower income.

They could be targeted with:

- affordable offers;
- discounts;
- trend-based products;
- youth-oriented marketing campaigns.

---

### Cluster 3 — Wealthy but Careful Customers

This cluster contains **35 customers**.

Average profile:

- average age: 41.11 years;
- average annual income: 88.20k$;
- average spending score: 17.11.

These customers have high income but low spending behavior.

They represent an important business potential, but they are currently less engaged.

They could be targeted with:

- personalized recommendations;
- discovery offers;
- premium product trials;
- campaigns designed to understand their purchase barriers.

---

### Cluster 4 — Low-Income Conservative Customers

This cluster contains **23 customers**.

Average profile:

- average age: 45.22 years;
- average annual income: 26.30k$;
- average spending score: 20.91.

These customers have both low income and low spending score.

They could be targeted with:

- budget-friendly offers;
- discounts;
- entry-level products;
- promotional campaigns.

---

## Clustering Evaluation

The clustering quality was evaluated using the **Silhouette Score**.

The Silhouette Score measures whether each point is:

- close to the points in its own cluster;
- far from the points in other clusters.

The score ranges from `-1` to `1`.

| Score Range | Interpretation |
|---|---|
| Close to 1 | Well-separated clusters |
| Close to 0 | Overlapping clusters |
| Close to -1 | Poor cluster assignment |

---

## Silhouette Score Results

The Silhouette Score was computed for several values of `k`.

| k | Silhouette Score |
|---:|---:|
| 2 | 0.321 |
| 3 | 0.467 |
| 4 | 0.494 |
| 5 | 0.555 |
| 6 | 0.540 |
| 7 | 0.528 |
| 8 | 0.455 |
| 9 | 0.457 |
| 10 | 0.443 |

The best Silhouette Score is obtained for:

```text
k = 5
```

with a score of approximately:

```text
0.555
```

This confirms the result obtained with the Elbow Method.

---

## Final Choice of k

Both methods lead to the same conclusion:

| Method | Suggested Number of Clusters |
|---|---:|
| Elbow Method | 5 |
| Silhouette Score | 5 |

The final model therefore uses:

```text
k = 5
```

---

## Technologies Used

This project was developed with Python and the following libraries:

| Library | Usage |
|---|---|
| `pandas` | Data loading, cleaning and analysis |
| `matplotlib` | Data visualization |
| `scikit-learn` | Feature scaling, K-Means clustering and evaluation |

---

## Project Structure

```text
.
├── Mall_Customers.ipynb
└── README.md
```

---

## How to Run the Project

1. Clone the repository:

```bash
git clone <repository-url>
```

2. Open the notebook:

```text
Mall_Customers.ipynb
```

3. Run the cells in order.

The notebook contains the complete workflow:

- data loading;
- exploratory data analysis;
- data preprocessing;
- feature scaling;
- cluster selection;
- K-Means training;
- cluster interpretation;
- clustering evaluation.

---

## Key Takeaways

This project shows that customer segmentation can be performed even without a target variable.

The analysis also highlights an important point:

> A lack of linear correlation does not necessarily mean a lack of structure.

In this dataset, `Annual_Income` and `Spending_Score` are almost not correlated, but they still form clear customer groups.

K-Means successfully identified five meaningful customer segments that can be used for marketing analysis and business decision-making.

---

## Limitations

This project has some limitations:

- only two variables were used for clustering;
- `Age` and `Gender` were not included in the final model;
- the cluster names are business interpretations, not official labels;
- K-Means assumes compact and roughly spherical clusters;
- the results may change if more variables or other algorithms are used.

---

## Possible Improvements

Future improvements could include:

- adding `Age` to the clustering process;
- encoding and using the `Gender` variable;
- comparing K-Means with DBSCAN;
- comparing K-Means with hierarchical clustering;
- creating more advanced visualizations;
- saving plots in an `images/` folder and displaying them in this README;
- building marketing recommendations for each customer segment.

---

## General Conclusion

This project demonstrates how unsupervised learning can be used to segment customers based on their behavior.

Using only annual income and spending score, the K-Means algorithm identified five clear customer profiles:

1. Standard / Balanced Customers;
2. High-Value Premium Customers;
3. Young Low-Income Big Spenders;
4. Wealthy but Careful Customers;
5. Low-Income Conservative Customers.

The final choice of `k = 5` was validated by both the Elbow Method and the Silhouette Score.

The final Silhouette Score is approximately **0.555**, which indicates a reasonable clustering quality for a simple two-feature model.

Overall, this project provides a clear introduction to customer segmentation, clustering evaluation and business interpretation of unsupervised learning results.
