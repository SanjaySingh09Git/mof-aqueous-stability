# Descriptor Parsimony in Data-Driven Prediction of MOF Aqueous Stability

> **A Machine Learning Analysis for Computational Materials Discovery**

**Author:** Sanjay Singh  
**Contact:** sanjay.singh@navencia.com

---

## Overview

This repository contains the complete analysis code and datasets accompanying the above manuscript. The study applies gradient-boosted decision trees (XGBoost) with Shapley additive explanations (SHAP) to predict the aqueous stability of metal-organic frameworks (MOFs), using the benchmark dataset of Batra et al. (2020) as a foundation.

**Key findings:**
- Predictive performance plateaus at **15–20 SHAP-ranked features** despite 88 descriptors being available
- **Linker-derived descriptors account for ~81%** of aggregate SHAP importance; metal electronic properties ~19%
- Per-framework SHAP attribution reveals **context-dependent descriptor effects** that explain the limited generality of single-parameter stability heuristics
- High-confidence misclassifications concentrate in **Zn-MOFs** with coordinated solvent or weakly bound auxiliary ligands

---

## Repository Structure

```
mof-aqueous-stability/
│
├── README.md                            — this file
├── requirements.txt                     — Python dependencies
├── LICENSE                              — MIT License
│
├── data/
│   ├── traindata_binary_prepared.csv    — 207-MOF training set (Batra et al. 2020)
│   ├── predictdata_binary.csv           — 88-MOF external test set (Batra et al. 2020)
│   ├── validationdata_binary.csv        — 10-MOF post-publication validation set
│   └── validation_groundtruth.csv       — Ground truth labels for 10-MOF set (see note below)
│
└── notebook/
    └── descriptor_parsimony_mof_stability.ipynb   — main analysis notebook
```

---

## Data Files

| File | MOFs | Description | Source |
|------|------|-------------|--------|
| `traindata_binary_prepared.csv` | 207 | Training set with 88 descriptors and binary stability labels | Batra et al. (2020) |
| `predictdata_binary.csv` | 88 | Independent external test set with ground truth labels | Batra et al. (2020) |
| `validationdata_binary.csv` | 10 | Post-publication validation set (features only) | Batra et al. (2020) |
| `validation_groundtruth.csv` | 10 | Ground truth labels for post-publication validation set | Batra et al. (2020) Table 2 |

> **Note on `validation_groundtruth.csv`:** The `validationdata_binary.csv` file contains the original Batra et al. model predictions (`predict_binary` column) rather than experimental ground truth labels. Ground truth stability classifications (S/HK = stable = 1, LK = likely degraded = 0) were extracted from Batra et al. (2020) Table 2 and stored separately in `validation_groundtruth.csv` to preserve the integrity of the original data file.

**Label encoding:** Stable = 1, Unstable = 0  
**Class distribution (training set):** 143 stable (69.1%), 64 unstable (30.9%)

---

## Notebook

The analysis is contained in a single Jupyter notebook:

**`descriptor_parsimony_mof_stability.ipynb`**

The notebook is self-contained and runs sequentially from top to bottom. Cell outputs are preserved so results can be inspected without re-running. Key sections:

| Cells | Section |
|-------|---------|
| 1–7 | Data loading, feature alignment, matrix construction |
| 8–9 | XGBoost model definition, feature cleaning |
| 10–15 | 5-fold stratified cross-validation and Fold 1 diagnostic analysis |
| 16–21 | Global SHAP feature importance and ranking |
| 22–27 | Descriptor parsimony benchmark (Full, Top-20, Top-15, Top-10, Top-3, Top-1) |
| 28–33 | SHAP domain aggregation, Figure 3 generation |
| 34–35 | Final Top-20 model, confusion matrix (Figure S1) |
| 36 | Figure 5: Metal-specific stability predictions (HSAB validation) |
| 37 | Misclassification analysis (Table 6) |
| 38 | SHAP waterfall for MOF index 80 (Figure S2), Top-15/10 feature lists |
| 39 | Table 2: Post-publication 10-MOF validation |

---

## Installation and Usage

### Requirements

Python 3.11+ is required. Install all dependencies with:

```bash
pip install -r requirements.txt
```

### Running the Notebook

```bash
jupyter notebook notebook/descriptor_parsimony_mof_stability.ipynb
```

Place all CSV files from the `data/` folder in the **same directory** as the notebook before running, or update the file paths in Cell 2 accordingly:

```python
train_df = pd.read_csv("traindata_binary_prepared.csv")
val_df   = pd.read_csv("validationdata_binary.csv")
pred_df  = pd.read_csv("predictdata_binary.csv")
```

### Reproducibility

All results are fully reproducible using a fixed random seed (`RANDOM_SEED = 42`) set in Cell 1. The notebook was developed and tested on:

- Python 3.11.5
- Windows 11 (also compatible with Linux/macOS)
- See `requirements.txt` for exact package versions

---

## Generated Outputs

Running the notebook end-to-end produces the following files:

| Output File | Corresponds To |
|-------------|---------------|
| `Figure2_XGBoost_Full_vs_Top20.pdf/.png` | Manuscript Figure 2 |
| `Figure5_Metal_Stability_HSAB.pdf/.png/.jpg` | Manuscript Figure 5 |
| `FigureS1_ConfusionMatrix_Top20_XGBoost.pdf/.png` | SI Figure S1 |
| `FigureS2_SHAP_Waterfall_MOF80.pdf/.png/.jpg` | SI Figure S2 |
| `xgb_top20_features.csv` | Top-20 SHAP-ranked features |
| `Table1_metrics.csv` | Table 1 performance metrics |

Note: Figure 3 and Figure 4 are displayed inline in the notebook but not saved to disk by default.

---

## Citation

If you use this code or data in your work, please cite:

```
Singh S (2026) Descriptor Parsimony in Data-Driven Prediction of MOF Aqueous Stability:
A Machine Learning Analysis for Computational Materials Discovery.
Computational Materials Science. [DOI: pending]
```

Please also cite the original dataset:

```
Batra R, Chen C, Evans TG, Walton KS, Ramprasad R (2020)
Prediction of water stability of metal-organic frameworks using machine learning.
Nature Machine Intelligence 2:704–710.
https://doi.org/10.1038/s42256-020-00249-z
```

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

The datasets (`traindata_binary_prepared.csv`, `predictdata_binary.csv`, `validationdata_binary.csv`) are derived from Batra et al. (2020) and are subject to the terms of the original publication.
