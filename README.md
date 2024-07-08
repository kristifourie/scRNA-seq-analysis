# Single Cell RNA-Seq Analysis of Breast T-Cells

## Overview

This repository contains the code and data for the single-cell RNA sequencing (scRNA-seq) analysis of regulatory T cells (Tregs) from breast tumor and healthy breast tissue. The study aims to explore the distinct states and functional categories of Tregs within the tumor microenvironment (TME) and normal breast tissue (NBT).

## Table of Contents

1. [Data Acquisition](#data-acquisition)
2. [Data Processing](#data-processing)
3. [Cell Type Annotation](#cell-type-annotation)
4. [Clustering](#clustering)
5. [Differential Expression and Gene Ontology Analysis](#differential-expression-and-gene-ontology-analysis)
6. [Treg States and DEG Functional Categories](#treg-states-and-deg-functional-categories)
7. [Statistical Analysis](#statistical-analysis)
8. [Code Execution](#code-execution)
9. [Results](#results)
10. [References](#references)

## Data Acquisition

The breast tumor T-cell data was downloaded from the Broad Institute single-cell and spatially resolved atlas of human breast cancers. This atlas includes scRNA-seq data of 26 breast tumor samples, consisting of:
- 12 ER-positive (ER+)
- 9 triple-negative breast cancer (TNBC)
- 3 HER2-positive (HER2+)
- 2 HER2+/ER+

The age range of the patients from whom these samples were obtained spans from 41 to 82 years old.

The non-tumorigenic breast tissue T-cell data was downloaded from the Human Cell Atlas. This atlas includes scRNA-seq data of 126 normal breast tissues.

## Data Processing

The "Seurat" R package (v5.1.0) was used for quality control, data normalization, identification of variable features, and data scaling. The steps involved are as follows:

1. **Creation of Seurat Objects**: 
    - Tumor and healthy samples were filtered using the `CreateSeuratObject()` function with a minimum threshold of 3 cells and 200 features per cell.
    - Cells with more than 5% mitochondrial gene expression were filtered out.

2. **Normalization**:
    - The `NormalizeData()` function applied the "LogNormalize" method to both tumor and healthy samples.

3. **Identification of Variable Features**:
    - The `FindVariableFeatures()` function identified the top 2,000 variable features using the "vst" method.

4. **Scaling**:
    - The `ScaleData()` function standardized the expression of each feature.

## Cell Type Annotation

The SingleR package (v2.4.1) was used for single-cell annotation, employing the celldex MonacoImmuneData dataset (GSE107011) as a reference. Validation involved creating UMAPs and using `FeaturePlot()` in Seurat to visualize known Treg markers such as FOXP3, CD4, and CD25. The labels from SingleR were added as metadata to the Seurat objects, which were then filtered to include only Tregs for downstream analysis.

## Clustering

Cell clustering was performed using the Seurat classic workflow:

1. **Dimensionality Reduction**:
    - PCA was conducted on both tumor and healthy datasets.
    - UMAPs were created based on 30 principal components.

2. **Clustering**:
    - The `FindClusters()` function employed the Louvain algorithm with a resolution of 0.5.
    - Marker genes were identified using the `FindMarkers()` function.
    - The `FeaturePlot()` function visualized the expression of various marker genes among the clusters.

## Differential Expression and Gene Ontology Analysis

Differential gene expression (DGE) analysis was performed with DESeq2 in R using combined raw count matrices from both datasets. DESeq2 normalized the count data and tested for differential expression using a model based on the negative binomial distribution. The results were used to run a Gene Ontology (GO) analysis using the GOrilla web toolkit.

## Treg States and DEG Functional Categories

Treg clusters were subdivided into effector Tregs (eTregs) and naïve Tregs (nTregs) based on the average expression levels of FOXP3 and CD25. DEGs were grouped into five functional categories: stress, inflammatory, cytoskeletal, metabolic, and transcription factor genes, providing insights into Treg adaptive mechanisms.

## Statistical Analysis

All statistical analyses were performed using R (v4.3.3). P values were two-tailed, and P < 0.05 was considered statistically significant.

## Code Execution

To reproduce the analysis, follow these steps:

1. Clone the repository.
2. Download the raw count matrices from the cell atlases and place them in the wd directories.
3. Open the `processing.Rmd` file and run the chunks sequentially to process the data.
4. Open the `scRNA_annotation.Rmd` to annotate the Tregs
5. Open the `Data_analysis.Rmd` file and run the chunks sequentially to analyse the data.

## Results

The results include:
- Normalized and scaled data
- UMAP visualizations
- Cluster identification and marker gene expression
- Differential gene expression results
- Gene Ontology analysis

## References

1. Broad Institute Single-Cell and Spatially Resolved Atlas of Human Breast Cancers
2. Human Cell Atlas
3. celldex MonacoImmuneData dataset (GSE107011)
4. Seurat R package documentation
5. SingleR package documentation
6. DESeq2 package documentation
7. GOrilla web toolkit documentation
