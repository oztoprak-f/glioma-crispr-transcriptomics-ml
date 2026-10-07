# glioma-crispr-transcriptomics-ml
An interpretable machine learning framework integrating CRISPR dependencies and transcriptomic states to reveal therapeutic vulnerabilities in gliomas. Presented at the EMBO | EMBL AI and Biology Symposium.


Integrating CRISPR Dependencies and Transcriptomic States Using Interpretable Machine Learning to Reveal Therapeutic Vulnerabilities in Gliomas

This repository contains the end-to-end computational framework and reproducibility pipeline for the pilot study presented at the EMBO | EMBL Symposium: AI and Biology (Heidelberg). The framework integrates functional genomics screening data with baseline transcriptomic states using an explainable machine learning architecture to map and predict context-specific gene essentiality in human adult glioma models.

Project Overview

Gliomas exhibit substantial transcriptional heterogeneity reflecting diverse cellular states that influence which genes become essential for tumor survival. While large-scale functional genomics tools like CRISPR-Cas9 screens isolate essential genes across cancer models, mapping the exact biological mechanisms underlying these dependencies remains an open challenge. This tool addresses this bottleneck by summarizing high-dimensional RNA-seq expression profiles at the biological pathway level to predict binarized CRISPR gene dependency states.

Key Features

• Multi-omic data integration mapping matched RNA-seq expressions (TPM) and CRISPR knockout scores (CERES) across adult-like glioma cohorts.
• Multicollinearity diagnostic engine utilizing Variance Inflation Factor (VIF) filtering to prune overlapping feature sets across major curated tumor programs.
• Unsupervised patient/cell-line stratification via k-means clustering (\(k = 4\)) to capture pathway-driven expression patterns.
• Explainable modeling pipeline utilizing Partial Least Squares Regression (PLSR) for dimensionality reduction followed by Logistic Regression classification.
• Validation engine utilizing Bootstrap Resampling to calculate robust Area Under the Receiver Operating Characteristic (AUROC) confidence intervals.

Mathematical & Methodological Framework


1. Cohort Isolation & Preprocessing

Initial feature spaces comprised 60,654 expression genes and 18,436 dependency genes. Filtering out pediatric, GSARC, and uncharacterized samples isolated a clean cohort of matching adult glioma lines. Duplicate resolving steps yielded an intersecting analytical space of exactly 58 cell lines.

2. Pathway Scoring & Multi-Collinearity Pruning

Gene expression profiles were summarized into pathway activity scores calculated as the mean expression of genes within a curated set representing major tumor programs (including metabolism, stress response, and signaling). To prevent model instability, features were evaluated using the Variance Inflation Factor (VIF):
\(\text{VIF}_{i}=\frac{1}{1-R_{i}^{2}}\)
Pathways exhibiting high redundancy (e.g., Stress_UPR_ROS with VIF of 11.75 and Glycolysis with VIF of 8.27) were audited, while the remaining features showing acceptable collinearity boundaries were preserved for downstream classification.

3. Dimensionality Reduction & Dependency Classification

Target CRISPR CERES dependency scores were binarized using a selective loss-of-fitness threshold of \(-0.5\) (where \(0 = \text{non-dependent}\) and \(1 = \text{dependent}\)). Extracted latent axes from PLSR capture highly distinct biological programs across the components:
• PLS1: Mesenchymal / Inflammatory Axis
• PLS2: Proliferative / Oxidative Metabolism Axis
• PLS3: Metabolic Rewiring / Lipid–Hypoxia Axis
• PLS4: Ferroptosis / Oxidative Stress Axis
Logistic regression classifiers were subsequently trained on these latent components to predict the binary dependency states.

Performance & Validation

Model stability and generalization were evaluated using extensive cross-validation backed by Bootstrap Resampling to estimate stable AUROC confidence intervals. The framework achieved strong classification performance for top selective vulnerabilities:
• COX10: \(\text{AUROC} \approx 0.88\) (Dependency linked to oxidative phosphorylation-driven metabolic states)
• MRPS17: \(\text{AUROC} \approx 0.85\) (Dependency associated with metabolic demand and stress-response pathways)
• MET: \(\text{AUROC} \approx 0.83\) (Dependency linked to growth signaling and inflammatory programs)
• SAR1A: \(\text{AUROC} \approx 0.83\)

Directory Structure

text
├── README.md
├── environment.yml             # Conda environment file for total replication
├── Snakefile                   # Snakemake workflow manager orchestrating steps
├── run_hpc_job.sh              # Slurm/Bash execution script for cluster deployment
├── data/
│   ├── raw/                    # Instructions to download raw DepMap matrices
│   └── processed/              # Cleaned matrices (under 50MB for runnable validations)
├── notebooks/
│   └── 01_glioma_crispr_pipeline.ipynb  # End-to-end master analysis notebook
└── src/
    ├── data_processing.py      # Multi-omic matching and missingness filters
    ├── feature_engineering.py  # Pathway activity scoring and VIF computation
    ├── models.py               # PLSR feature extraction and Logistic Regression
    └── validation.py           # Bootstrap resampling and AUROC plotting
Use code with caution.

Getting Started & Reproducibility


Prerequisites

Ensure you have Conda installed on your system. This pipeline is optimized for local deployment as well as Linux High-Performance Computing (HPC) environments.

1. Clone the Repository

bash
git clone https://github.com
cd glioma-crispr-transcriptomics-ml
Use code with caution.

2. Set Up the Environment

Create and activate the environment using the locked package configurations:
bash
conda env create -f environment.yml
conda activate glioma-ml
Use code with caution.

3. Run the Pipeline

Execute the automated end-to-end Snakemake pipeline locally:
bash
snakemake --cores 4
Use code with caution.
For cluster deployment (HPC with Slurm workload manager):
bash
sbatch run_hpc_job.sh
Use code with caution.

Data Availability

Raw datasets are derived from the Cancer Dependency Map (DepMap) public portal. Detailed download scripts and specific database version tags are documented within the data/raw/ path. Lightweight, downsampled testing structures are provided under data/processed/ to guarantee pipeline functionality in under two minutes.

Symposium Citation

Presented as an authorized abstract and poster session at:
• EMBO | EMBL Symposium: AI and Biology
• March 10-13, Heidelberg
• Session 3: Explainable AI and mechanistic interpretability

Contact

Fadime Öztoprak, PhD
Independent Researcher
Email: fadimeoztoprak@gmail.com
Open to collaborations and research opportunities
