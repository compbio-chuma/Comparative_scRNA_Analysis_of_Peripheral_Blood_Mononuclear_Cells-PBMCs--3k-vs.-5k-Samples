# Peripheral Blood Mononuclear Cells (PBMCs) scRNA-Seq Analysis Comparison: 3k vs. 5k Samples

## Overview

This project aims to compare two single-cell RNA sequencing (scRNA-seq) datasets derived from peripheral blood mononuclear cells (PBMCs): a 3k sample and a 5k sample. The goal is to analyze and interpret the similarities and differences between these datasets, focusing on cell clustering, marker gene expression, and overall data distribution. Through this analysis, we aim to understand how an increase in the number of cells impacts the results of scRNA-seq data processing, visualization, and biological insights.

We utilize the Scanpy Python package for analysis and visualization, employing clustering techniques like Leiden to group cells based on gene expression profiles. The project involves preprocessing the data, identifying highly variable genes, performing dimensionality reduction (PCA and UMAP), clustering, and interpreting the results.

The project is structured as follows:
1. Data Loading and Preprocessing
2. Identifying Highly Variable Genes
3. Principal Component Analysis (PCA)
4. Clustering and UMAP Visualization
5. Differential Gene Expression Analysis
6. Comparison Between 3k and 5k Datasets
7. Interpretation of UMAP Visualizations
8. Biological Insights and Conclusions

Each section includes detailed steps and explanations, observations, and biological interpretations based on the results of the analysis. This documentation provides a comprehensive guide for replicating this analysis and understanding the impact of increasing sample size on scRNA-seq data.

## 1. Data Loading and Preprocessing

The first step in the analysis is loading the two datasets (3k and 5k samples) and performing necessary preprocessing steps. The datasets contain raw gene expression data (in the form of UMIs, or unique molecular identifiers) and require normalization, scaling, and filtering for quality control.

### 1.1 Loading the Data

Both the 3k and 5k datasets are provided in the .mtx and .h5ad format respectively. `.h5ad` is the preferred format for AnnData objects used in Scanpy. Files for loading the datasets are uploaded.

### 1.2 Quality Control

To ensure high-quality data, we filter cells and genes based on several criteria. These criteria typically include removing cells with low gene counts (likely empty droplets) or abnormally high mitochondrial gene expression (indicating dying cells).

I also computed the percentage of mitochondrial genes as a further QC measure:

### 1.3 Normalization and Log Transformation

After filtering, I normalized the gene expression data using total counts per cell and apply log.

### 1.4 Scaling the Data

Before performing dimensionality reduction, I scaled the data to ensure that all genes have a mean of 0 and a variance of 1. This step is essential for PCA and clustering.

## 2. Identifying Highly Variable Genes

Highly variable genes are the most informative genes that drive the differences between cell populations. Identifying these genes helps focus on the most relevant information for clustering and dimensionality reduction.

### Observations:

#### Comparison Between 3k and 5k:
- Number of highly variable genes in the 5k dataset: 4777
- Number of highly variable genes in the 3k dataset: 1872
- The discrepancy in HVG numbers suggests that while both datasets share some common features, the 5k dataset has additional complexity. There may be more subtle variations in gene expression in the 5k dataset that the 3k dataset doesn't capture due to its smaller size.
- The additional HVGs in the 5k dataset might correspond to more minor cell populations or states that only appear when enough cells are sequenced, which can be especially useful for discovering rare cell types.

## 3. Principal Component Analysis (PCA)

PCA is used to reduce the dimensionality of the data while preserving the most variance. It helps in denoising the data and provides a starting point for clustering and visualization.

### Observations:
- **Majority of Variance Captured by Few Components:** Both PCA plots show that the first few principal components (PC1 and PC2) capture a significant portion of the variance, as most of the data points are spread out along these axes.
- **5k Shows More Separation:** The 5k PCA plot shows more distinct separation of clusters compared to the 3k plot, likely because the higher sample size allows for better differentiation between cell populations.

## 4. Clustering and UMAP Visualization

After dimensionality reduction, I clustered the cells using the Leiden algorithm. This method groups cells based on their nearest neighbors in the reduced PCA space. Then, I visualized the clusters using UMAP (Uniform Manifold Approximation and Projection).

### Observations:
- Both datasets revealed distinct clusters representing different immune cell types.
- The 5k dataset showed more clusters (13) compared to the 3k dataset (10), indicating that the increased number of cells captured more diversity.
- Some clusters, such as clusters 11 and 12, are observed in the 5k dataset but not in the 3k dataset. This suggests that these clusters may represent rare cell populations that are only detectable in larger datasets, due to the higher number of cells sampled in the 5k dataset.

## 5. Differential Gene Expression Analysis

To understand the biological significance of the identified clusters, I performed differential gene expression analysis to identify marker genes for each cluster.

### Observations:
- The marker genes for each cluster are consistent with known immune cell types (e.g., CS3T for T cells, MS4A1 for B cells).

## 6. Comparison Between 3k and 5k Datasets

### Key Similarities:
1. **Core Cell Populations:**
    - Both datasets captured the core immune cell types, including T cells, B cells, natural killer (NK) cells, and monocytes.
    - Clusters 0, 1, and 2 in both datasets likely correspond to these major cell types.
2. **Distinct Separation:**
    - The clustering in both datasets shows well-separated populations, indicating clear distinctions between cell types.

### Key Differences:
1. **Number of Clusters:**
    - The 5k dataset captured 13 clusters, while the 3k dataset captured 10 clusters. The additional clusters in the 5k dataset represent rare cell types, which are less likely to be detected in smaller datasets.
2. **Marker Gene Expression:**
    - The 3k dataset provided more expression of marker genes, ‘CST3’, ‘NKG7’, ‘PPBP’, ‘MS4A1’ for each cluster.

## 7. Interpretation of UMAP Visualizations

### 7.1 3k Dataset UMAP
The UMAP plot of the 3k dataset shows clear separation between major cell types. However, the smaller sample size leads to a less granular representation of rare cell types. The clusters representing T cells, B cells, and monocytes are prominent, but the resolution of smaller populations is limited.

### 7.2 5k Dataset UMAP
The UMAP plot of the 5k dataset shows more detailed separation between clusters. Not only are the major cell types distinct, but additional clusters appear that likely represent rare populations or subpopulations of immune cells. This highlights the advantage of using larger datasets in scRNA-seq analyses, as more subtle biological differences can be detected.

## 8. Biological Insights and Conclusions

### Key Insights:
1. **Impact of Sample Size:**
    - The increased sample size in the 5k dataset resulted in a more granular view of the cellular diversity within the PBMC population. This demonstrates the importance of larger sample sizes for capturing rare cell populations in scRNA-seq studies.
2. **Marker Gene Expression:**
    - Both datasets provided consistent marker gene expression patterns for major immune cell types. However, the 3k dataset offered a higher expression for the selected marker gene.
3. **Clustering Resolution:**
    - The 5k dataset produced more clusters, including rare populations, reflecting the biological heterogeneity present in the PBMC sample.

### Conclusion:
This analysis demonstrates that larger datasets, such as the 5k PBMC sample, provide more comprehensive insights into cellular diversity and enable the identification of rare cell populations. By comparing the 3k and 5k datasets, I show that increasing the number of cells enhances clustering resolution and the detection of subtle transcriptional differences, ultimately leading to more detailed biological interpretations.

## Project Structure
- `data/`: Contains the datasets used in the analysis (`3k_pbmc.h5ad`, `5k_pbmc.h5ad`).
- `ipynb/`: Includes Jupyter notebook Python scripts used for analysis.
- `docs/`: Contains this README and other relevant documentation.
- `figures/`: Stores the visualizations generated during the analysis.
- `results/`: Stores the results of clustering, differential gene expression, and other analyses.
