# Cell Image Project

**Investigating Unsupervised Learning Techniques on Cell-Image Data**
Honors thesis in Mathematics, The College of William and Mary — John Pendergrass
Advisor: Gregory Hunt

## Overview

High-throughput imaging (HTI) produces datasets with hundreds of thousands of rows (drug treatments) and thousands of columns (phenotypic features extracted from cell images, such as size, shape, texture and intensity). This project evaluates how well dimensionality reduction methods — PCA (linear), t-SNE and UMAP (nonlinear) — reveal structure in this data, in particular clusters corresponding to a compound's mechanism of action (MoA), which matters for drug discovery and repurposing. It also examines how batch effects can obscure the biological signal.

## Approach

1. **Simulations.** Synthetic 2D shapes (clusters, spiral, concentric circles, uniform random, grid) are embedded into 10 dimensions, either linearly (random projection) or nonlinearly (RBF kernel), standardized, and perturbed with Gaussian noise. PCA, t-SNE (perplexity 10, 30) and UMAP (10, 30 neighbors) are compared on how well they recover the original structure.
2. **Real datasets.** The methods are applied to three image-profiling datasets, using k-nearest-neighbors classification on the reduced data to quantify how well each embedding captures cell line, batch date, or MoA:
   - **Small-molecule dataset** (Cox et al., 2020): 15 reporter cell lines, 1,008 reference compounds, 218 MoAs.
   - **Drug Repurposing Hub dataset** (Way et al., 2022): Cell Painting profiles of A549 cells treated with 1,327 compounds.
   - **Cell Image Library dataset** (Bray et al., 2017): Cell Painting profiles of U2OS cells for 30,616 treatments.
3. **Batch correction.** Plate-wise z-score normalization is used to reduce technical variation before assessing MoA structure.

## Key Findings

- **Simulations:** PCA preserves structure well for linearly embedded data but fails on nonlinearly embedded data. t-SNE and UMAP separate groups that a linear method cannot, but can also create artificial structure (for example, fragmenting purely random data). Results depend on hyperparameters; UMAP was more stable than t-SNE across settings.
- **Small-molecule data:** Cell line is the dominant source of clustering (about 99.9% kNN accuracy with t-SNE/UMAP vs. about 89% with PCA). Within one cell line (A549-ACTB-RAB5A), MoA classification is modest (about 31% for t-SNE vs. 26% for PCA), but nonlinear methods still do better.
- **Drug Repurposing Hub data:** Batch date and plate dominate the embedding (about 97% date accuracy with t-SNE vs. 63% with PCA). Plate normalization removes most of this visually, after which t-SNE (perplexity 10) gives the best MoA classification (test accuracy 0.257, vs. 0.176 for the best UMAP and 0.080 for PCA).
- **Cell Image Library data:** Strong date-driven batch effects again; after plate normalization, several MoA classes are classified with perfect or very high F1 scores.

## Conclusion

Nonlinear methods, particularly t-SNE and UMAP, offer clear advantages over PCA for HTI data in drug discovery, but they can impose spurious structure and need careful tuning. Batch effects are consistently the dominant source of variation, and plate-level normalization is essential before drawing biological conclusions. Future work: standardized preprocessing pipelines for batch effects, and combining dimensionality reduction with supervised models for better MoA prediction.

## Contents

| Notebook | Thesis section |
|---|---|
| `simulation.ipynb` | Ch. 3 — Simulations |
| `Copy_of_smallmolecule.ipynb` | 4.1 — Small-molecule dataset |
| `LINCS.ipynb` | 4.2 — Drug Repurposing Hub dataset |
| `Cell_Image_Project.ipynb` | 4.3 — Cell Image Library dataset |
