# glioma-crispr-transcriptomics-ml

An interpretable machine learning framework integrating CRISPR dependencies and transcriptomic states to reveal therapeutic vulnerabilities in gliomas. Presented at the EMBO | EMBL AI and Biology Symposium.

# 🧬 Glioma CRISPR-Transcriptomics ML Pipeline

[![Python 3.9+](https://shields.io)](https://python.org)
[![Workflow-Snakemake](https://shields.io)](https://github.io)
[![Conference-EMBO/EMBL](https://shields.io)](https://embl.org)

An explainable machine learning framework that integrates **CRISPR dependencies (CERES scores)** and **transcriptomic states (RNA-Seq)** to uncover state-associated therapeutic vulnerabilities in human adult gliomas. 

> 📊 **Symposium Presentation:** This computational framework was presented at the **EMBO | EMBL Symposium: AI and Biology** (Heidelberg) under *Session 3: Explainable AI and Mechanistic Interpretability*.

---

## 📌 Project Architecture

The pipeline is split into logical computational chunks to handle high-dimensional multi-omic datasets cleanly:

```text
├── README.md
├── environment.yml             # One-click Conda environment definition
├── Snakefile                   # Snakemake workflow automating the pipeline
├── run_hpc_job.sh              # Slurm script for Linux HPC cluster execution
├── data/
│   ├── raw/                    # Instructions to download full DepMap matrices
│   └── processed/              # Cleaned datasets (<50MB for runnable validation)
├── notebooks/
│   └── 01_glioma_pipeline.ipynb # End-to-end master analysis notebook
└── src/
    ├── data_processing.py      # Multi-omic matching & cohort isolation
    ├── feature_engineering.py  # Pathway activity scoring & VIF diagnostics
    ├── models.py               # PLSR feature extraction & Logistic Regression
    └── validation.py           # Bootstrap resampling & AUROC evaluation
```

---

## 🔬 Computational Workflow

### 🛠️ Step 1: Data Isolation & Matching
* Filters out pediatric, GSARC, and uncharacterized models from the Broad Institute **Cancer Dependency Map (DepMap)**.
* Resolves profile duplicates and isolates an intersecting analytical cohort of exactly **58 adult glioma cell lines** across 18,436 dependency genes and 60,654 expression genes.

### 🧬 Step 2: Pathway Activity Scoring
* Summarizes high-dimensional RNA-Seq profiles by calculating mean gene expression across curated tumor programs (Metabolism, Stress Response, Signaling).
* Evaluates feature redundancy and multicollinearity using **Variance Inflation Factor (VIF)** thresholds to prune overlapping transcriptomic programs.

### 🤖 Step 3: Explainable ML Modeling
* Binarizes target CRISPR CERES scores using a selective loss-of-fitness threshold of `-0.5` (\(0 = \text{non-dependent}\), \(1 = \text{dependent}\)).
* Extracts biological latent axes using **Partial Least Squares Regression (PLSR)** and uses **Logistic Regression** to map those axes to dependency states.

---

## 📊 Model Performance Highlights

Generalization stability was validated via cross-validation combined with **Bootstrap Resampling** to calculate stable Area Under the Receiver Operating Characteristic (AUROC) confidence intervals. 

Our top identified state-dependent vulnerabilities achieved high predictive performance:

| Candidate Target Gene | Model Performance | Associated Biological Axis |
| :--- | :--- | :--- |
| **COX10** | 📈 \(\text{AUROC} \approx 0.88\) | Oxidative Phosphorylation-driven metabolic state |
| **MRPS17** | 📈 \(\text{AUROC} \approx 0.85\) | Mitochondrial ribosomal protein / Stress-response |
| **MET** | 📈 \(\text{AUROC} \approx 0.83\) | Receptor tyrosine kinase / Growth signaling |
| **SAR1A** | 📈 \(\text{AUROC} \approx 0.83\) | Intracellular vesicle transport |

---

## 🚀 Getting Started & Reproducibility

### 1. Environment Setup
Clone the repository and install all dependencies via Conda:
```bash
git clone https://github.com
cd glioma-crispr-transcriptomics-ml
conda env create -f environment.yml
conda activate glioma-ml
```

### 2. Local Execution (Automated Workflow)
Run the complete, end-to-end pipeline locally utilizing 4 cores:
```bash
snakemake --cores 4
```

### 3. High-Performance Computing (HPC) Deployment
For execution on cluster environments using a **Slurm** workload manager:
```bash
sbatch run_hpc_job.sh
```

---

## 📝 Contact & Citation

**Fadime Öztoprak, PhD**  
Independent Researcher  
📧 [fadimeoztoprak@gmail.com](mailto:fadimeoztoprak@gmail.com)  

*Feel free to open an issue or reach out directly for collaborations and research opportunities!*

Open to collaborations and research opportunities
