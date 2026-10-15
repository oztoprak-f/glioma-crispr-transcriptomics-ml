# 🧬 Glioma CRISPR-Transcriptomics ML Pipeline

[![Python 3.9+](https://shields.io)](https://python.org)
[![Conference-EMBO/EMBL](https://shields.io)](https://embl.org)

An explainable machine learning framework that integrates **CRISPR dependencies (CERES scores)** and **transcriptomic states (RNA-Seq)** to uncover state-associated therapeutic vulnerabilities in human adult gliomas. 

> 📊 **Symposium Presentation:** This computational framework was presented at the **EMBO | EMBL Symposium: AI and Biology** (Heidelberg) under *Session 3: Explainable AI and Mechanistic Interpretability*.

---

## 📌 Project Architecture

The entire analytical pipeline is self-contained within an end-to-end master Jupyter Notebook, removing complex setup requirements:

```text
├── README.md
├── environment.yml               # One-click Conda environment definition
├── data_raw/                     # Local folder for DepMap source matrices (git-ignored)
└── notebooks/
    └── 01_glioma_pipeline.ipynb  # End-to-end master analysis notebook
```

---

## 🔬 Computational Workflow (Inside the Notebook)

### 🛠️ Step 1: Data Isolation & Matching
* Filters out pediatric, GSARC, and uncharacterized models from the Broad Institute **Cancer Dependency Map (DepMap)**.
* Resolves profile duplicates and isolates an intersecting analytical cohort of exactly **58 adult glioma cell lines** across 18,436 dependency genes and 60,654 expression genes.

### 🧬 Step 2: Pathway Activity Scoring & VIF Diagnostics
* Summarizes high-dimensional RNA-Seq profiles by calculating mean gene expression across curated tumor programs (Metabolism, Stress Response, Signaling).
* Evaluates feature redundancy and multicollinearity using **Variance Inflation Factor (VIF)** thresholds to prune overlapping transcriptomic programs.

### 🤖 Step 3: Explainable ML Modeling
* Binarizes target CRISPR CERES scores using a selective loss-of-fitness threshold of `-0.5` (0 = non-dependent, 1 = dependent).
* Extracts biological latent axes using **Partial Least Squares Regression (PLSR)** and uses **Logistic Regression** to map those axes to dependency states.

---

## 📊 Model Performance Highlights

Generalization stability was validated via cross-validation combined with **Bootstrap Resampling** to calculate stable Area Under the Receiver Operating Characteristic (AUROC) confidence intervals (95% CI). 

Our top identified state-dependent vulnerabilities perfectly align with specific mitochondrial and metabolic tumor programs:

| Candidate Target Gene | Median AUROC | 95% Confidence Interval (CI) | Associated Biological Axis / Mechanism |
| :--- | :---: | :---: | :--- |
| 🧬 **TFB1M** | **0.891** | `[0.790 - 0.919]` | Mitochondrial translation / OXPHOS machinery |
| 🧬 **COX10** | **0.875** | `[0.777 - 0.913]` | Cytochrome c oxidase / Oxidative phosphorylation |
| 🧬 **MRPS17** | **0.853** | `[0.762 - 0.882]` | Mitochondrial ribosome demand / Metabolic stress |

---

## 🚀 Getting Started & Reproducibility

### 1. Environment Setup
Clone the repository and install all dependencies via Conda to ensure the notebook packages match exactly:
```bash
git clone https://github.com
cd glioma-crispr-transcriptomics-ml
conda env create -f environment.yml
conda activate glioma-ml
```

### 2. Execution
Launch Jupyter Notebook and run `notebooks/01_glioma_pipeline.ipynb` from top to bottom:
```bash
jupyter notebook
```

*Note: The raw input datasets are kept locally due to file size constraints. Full instructions to pull the standard DepMap matrices are annotated inside the notebook layout.*

---

## 📝 Contact & Citation

**Fadime Öztoprak, PhD**  
Independent Researcher  
📧 [fadimeoztoprak@gmail.com](mailto:fadimeoztoprak@gmail.com)  

*Feel free to open an issue or reach out directly for collaborations and research opportunities!*
