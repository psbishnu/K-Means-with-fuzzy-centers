# Adaptive Hyper-Ellipsoidal Interval Type-2 Fuzzy Cluster Centers

Python implementation and experimental notebooks for the **Adaptive Hyper-Ellipsoidal Interval Type-2 (IT2) Fuzzy Cluster Center K-Means** framework.

The method extends conventional K-Means by replacing each crisp centroid with a covariance-guided **hyper-ellipsoidal fuzzy-center nucleus** surrounded by a broader **Interval Type-2 uncertainty region**. Cluster assignment remains hard, but distances are computed relative to the fuzzy-center nucleus using Mahalanobis geometry.

## Method Overview

For each cluster, the framework:

1. Initializes clusters using K-Means.
2. Computes the cluster centroid and covariance matrix.
3. Regularizes the covariance matrix for numerical stability.
4. Constructs an inner hyper-ellipsoidal **nucleus**, representing the most reliable center region.
5. Constructs an outer hyper-ellipsoidal **Type-2 uncertainty boundary**.
6. Reassigns observations according to the minimum distance to the fuzzy-center nucleus.
7. Iterates until convergence.
8. Computes nucleus density and Type-2 coverage for uncertainty-aware interpretation.

For a point \(x\), let

\[
q_j(x)=(x-c_j)^T\Sigma_j^{-1}(x-c_j).
\]

The nucleus-distance rule used in the implementation is

\[
D_j(x)=
\begin{cases}
0, & q_j(x)\leq\tau_j^L,\\
(\sqrt{q_j(x)}-\sqrt{\tau_j^L})^2, & q_j(x)>\tau_j^L.
\end{cases}
\]

The point is assigned to the cluster with minimum \(D_j(x)\).

## Repository Notebooks

| Notebook | Purpose |
|---|---|
| `0. Conversion(1).ipynb` | Converts categorical dataset attributes to numerical form using label encoding. |
| `dataDes(1).ipynb` | Reports dataset size, feature count, number of classes, and class distribution. |
| `2_Proposed(1).ipynb` | Main implementation of the proposed adaptive hyper-ellipsoidal IT2 fuzzy nucleus-center clustering method. |
| `Fully_Integrated_Clustering_Comparison_DBNS(2).ipynb` | Compares the proposed method with K-Means, Fuzzy C-Means, Gaussian Mixture Model, and Spectral Clustering. |
| `Comparison_Option2_IT2_Nucleus_Center_KMeans_DBNS(2).ipynb` | Additional implementation of the comparative experiment using the nucleus-as-fuzzy-center formulation. |
| `Ablation(1).ipynb` | Performs sensitivity and ablation analysis for nucleus quantile, Type-2 quantile, and covariance regularization. |

## Benchmark Datasets

The experiments are designed for four UCI benchmark datasets:

- **Heart** — 1,025 samples, 13 features, 2 classes
- **Iris** — 150 samples, 4 features, 3 classes
- **Car Evaluation** — 1,727 samples, 6 features, 4 classes
- **Glass Identification** — 214 samples, 9 features, 6 classes

Place the required CSV datasets in the working directory and update `DATASET_PATH`, `LABEL_COLUMN`, and `N_CLUSTERS` in the relevant notebook.

## Compared Clustering Methods

The experimental comparison includes:

- Proposed IT2 Nucleus-Center K-Means
- Standard K-Means
- Fuzzy C-Means (FCM)
- Gaussian Mixture Model (GMM)
- Spectral Clustering

## Evaluation Metrics

The notebooks calculate several internal and external clustering measures:

- Silhouette Score
- Davies-Bouldin Index (DBI)
- **DBNS = Silhouette / DBI**
- Calinski-Harabasz Index (CHI)
- Adjusted Rand Index (ARI), when reference labels are available
- Normalized Mutual Information (NMI), when reference labels are available
- Runtime

The proposed method additionally reports:

- Nucleus count
- Type-2 region count
- Nucleus Density (ND)
- Type-2 Coverage (TC)

## Default Proposed-Method Parameters

The experiments use the following baseline configuration:

```text
Maximum iterations       = 100
Convergence tolerance    = 1e-5
Covariance regularization= 1e-4
Nucleus quantile (q_L)   = 0.40
Type-2 quantile (q_U)    = 0.80
```

The number of clusters should be configured according to the dataset.

## Sensitivity / Ablation Study

`Ablation(1).ipynb` evaluates the influence of:

- nucleus quantile \(q_L\);
- Type-2 quantile \(q_U\);
- covariance regularization \(\epsilon\);
- joint \(q_L\)-\(q_U\) configurations.

Typical outputs are written under:

```text
Result_<dataset_name>/Sensitivity_Ablation/
```

and include CSV result tables, vector PDF plots, baseline results, sensitivity summaries, and execution logs.

## Installation

A Python 3 environment with the main scientific Python packages is required.

```bash
pip install numpy pandas matplotlib scikit-learn jupyter
```

If the Fuzzy C-Means comparison requires `scikit-fuzzy`, install it with:

```bash
pip install scikit-fuzzy
```

## Running the Code

Clone or download the repository, place the benchmark CSV files in the project directory, and start Jupyter:

```bash
jupyter notebook
```

For a new dataset, modify the configuration section of the relevant notebook:

```python
DATASET_PATH = "your_dataset.csv"
N_CLUSTERS = 3
LABEL_COLUMN = "target"   # Use None when ground-truth labels are unavailable
```

A recommended execution sequence is:

```text
1. Conversion(1).ipynb              # if categorical conversion is required
2. dataDes(1).ipynb                 # inspect dataset characteristics
3. 2_Proposed(1).ipynb              # proposed method
4. Fully_Integrated_...ipynb        # baseline comparison
5. Ablation(1).ipynb                # sensitivity analysis
```

## Output

Depending on the notebook, results are saved in directories following the pattern:

```text
Result_<dataset_name>/
```

The generated artifacts include clustering metrics, comparison tables, sensitivity-analysis CSV files, logs, and PDF visualizations.

## Research Reference

This repository accompanies the research work:

**Adaptive Hyper-Ellipsoidal Interval Type-2 Fuzzy Cluster Centers**

The framework models uncertainty directly in the **cluster prototype** rather than exclusively through fuzzy memberships. The inner nucleus represents reliable center locations, while the outer Type-2 hyper-ellipsoid represents broader center uncertainty.

## Authors

**Simran Raj**  
**Partha Sarathi Bishnu**  
**Rathindra Nath Dutta**  

Department of Computer Science and Engineering  
Birla Institute of Technology, Mesra, Ranchi, India

## Citation

If you use this implementation in academic work, please cite the associated paper. Complete publication metadata/DOI can be added here after publication.

```bibtex
@article{raj_adaptive_heit2,
  title  = {Adaptive Hyper-Ellipsoidal Interval Type-2 Fuzzy Cluster Centers},
  author = {Raj, Simran and Bishnu, Partha Sarathi and Dutta, Rathindra Nath},
  note   = {Publication details to be updated}
}
```

## License

Add the license selected for the GitHub repository (for example, MIT, BSD-3-Clause, or another license appropriate for the project).
