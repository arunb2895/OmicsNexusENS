OmicsNexusENS

An open-source R and Shiny framework for multi-omics data analysis and biomarker discovery.

Status: early development. The single-cell RNA-seq, biomarker discovery and Shiny interface modules are implemented. Spatial deconvolution, proteomics and metabolomics preprocessing, and the Shiny server layer are in progress. The API may change before the first tagged release.

OmicsNexus takes raw count or intensity matrices through quality control, normalization, differential testing, cross-platform integration and machine-learning biomarker discovery. Every function works from the R console, and the same code powers a no-code graphical interface for researchers without programming experience.

Features
Per-modality analysis: quality control, normalization and differential expression for bulk RNA-seq, single-cell RNA-seq, spatial transcriptomics, proteomics and metabolomics.
Cross-platform integration: unsupervised multi-omics factor analysis (MOFA+) and sparse canonical correlation analysis (mixOmics).
Biomarker discovery: LASSO and Random Forest feature selection with held-out ROC/AUC validation and bootstrap confidence intervals.
No-code web interface: an interactive Shiny dashboard for upload, analysis, visualization and export.
Installation
r
if (!requireNamespace("devtools", quietly = TRUE)) install.packages("devtools")
devtools::install_github("arunb2895/OmicsNexusENS")

Several analysis backends live on Bioconductor and are installed separately:

r
if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
BiocManager::install(c("DESeq2", "limma", "MOFA2", "SingleR", "celldex"))
Quick start
r
library(OmicsNexus)

# Launch the interactive application
run_omics_app()

# Or work from the console
counts <- read.csv("counts.csv", row.names = 1, check.names = FALSE)
res <- run_scrna_pipeline(counts, resolution = 0.5, reference = "hpca")
Seurat::DimPlot(res$object, group.by = "singler_cluster_label", label = TRUE)

bio <- run_biomarker_discovery(factor_scores, y = metadata$group)
bio$performance
bio$plots$roc
Repository layout
OmicsNexus/
├── R/                            # Core analysis functions
│   ├── 01_bulk_rnaseq.R          # DESeq2, PCA, volcano plots
│   ├── 02_scrna_seq.R            # Seurat QC, clustering, cell-type annotation
│   ├── 03_spatial_transcript.R   # Spot deconvolution, spatial domains
│   ├── 04_proteomics_metab.R     # limma, imputation, normalization
│   ├── 05_multi_omics_MOFA.R     # MOFA+ and mixOmics integration
│   └── 06_biomarker_discovery.R  # LASSO, Random Forest, ROC/AUC
├── inst/app/                     # Shiny application
├── data-raw/                     # Tutorial datasets
└── vignettes/user_guide.Rmd      # Step-by-step tutorial
Statistical notes

Biomarker performance is reported on a stratified held-out test split. Feature filtering and scaling parameters are learned on the training split only, so no information from the test samples enters model fitting. Reporting performance on the same samples used for feature selection inflates the AUC substantially and is a common source of over-optimistic biomarker claims.

Contributing

Issues and pull requests are welcome. Please open an issue describing the change before submitting a large pull request.

License

MIT. Results are for research use only and are not intended for clinical decision making.
