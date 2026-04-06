# Multitask-Learning-for-Glioma-Segmentation

📄 [Read the full report](./Multitask-Learning-for-Glioma-Segmentation.pdf)

## Overview
This project was completed as part of my **M.S. in Computer Science & Engineering at The Ohio State University**.

It presents a **multi-task deep learning framework** that jointly performs:
- Tumor **segmentation**
- Genetic **mutation classification**
- Patient **survival prediction**

from multimodal MRI scans.

Unlike traditional pipelines that treat these tasks independently, this work explores how **shared representations** can improve performance across clinically relevant objectives.

---

## Key Contributions
- Designed a **three-head multi-task architecture** using a shared 3D U-Net encoder
- Integrated:
  - **Voxel-level segmentation**
  - **Per-gene mutation prediction** (EGFR, IDH1, PTEN, TP53)
  - **Cox-based survival analysis**
- Developed an **uncertainty-aware semi-supervised pipeline** for sparse mutation labels
- Evaluated **radiomics + deep feature fusion**
- Applied **multi-task loss balancing via uncertainty weighting**

---

## Architecture

The model uses a shared encoder with task-specific heads:

- **Segmentation Head**
  - 3D U-Net decoder
  - Predicts tumor subregions from MRI

- **Mutation Head**
  - Uses global feature embeddings + radiomics
  - Multi-head classifier for gene mutations

- **Survival Head**
  - Combines imaging features with clinical data
  - Predicts risk using Cox Proportional Hazards

➡️ The *multi-head architecture diagram (page 9)* shows how all three tasks share a common feature representation and are optimized jointly. :contentReference[oaicite:0]{index=0}

---

## Dataset
- **BraTS 2023** (MRI brain tumor dataset)
- ~1,251 subjects for segmentation
- Subsets with:
  - Genomic labels (mutation prediction)
  - Clinical data (survival analysis)

MRI modalities used:
- T1
- T1ce
- T2
- FLAIR

---

## Methodology

### Segmentation
- 3D U-Net with skip connections
- Soft Dice loss
- GroupNorm (small batch stability)

### Mutation Classification
- Deep features + **289 radiomic features**
- PCA-based dimensionality reduction
- Multi-head MLP classifier
- **Pseudo-labeling with uncertainty filtering**
  - Monte Carlo dropout
  - Confidence + variance thresholds

### Survival Analysis
- Cox Proportional Hazards model
- Combines:
  - Imaging features
  - Clinical variables (age, sex, therapy)

---

## Multi-Task Learning Strategy

- Shared encoder across all tasks
- Joint optimization using:
  - **Homoscedastic uncertainty weighting**
- Balances:
  - Dense segmentation signals
  - Sparse mutation labels
  - Survival objectives

---

## Key Insights

- Multi-task learning enables **feature sharing across clinical objectives**
- Radiomics + deep features improve downstream predictions
- Semi-supervised learning helps address **label scarcity in genomics**
- Joint optimization introduces tradeoffs between task performance

---

## Why This Matters

This project sits at the intersection of:
- Deep learning
- Medical imaging
- Computational biology

It demonstrates how a single system can extract **multiple clinically relevant signals** from non-invasive imaging, potentially reducing reliance on invasive procedures.

---

## Tech Stack
- Python
- PyTorch
- NumPy / SciPy
- PyRadiomics
- Scikit-learn

---

## Author
**Ayyoub Abdel-Aziz**  
M.S. Computer Science & Engineering  
The Ohio State University  

---

## Notes
- This work is research-focused and based on real-world medical datasets
- Future extensions could include:
  - Transformer-based encoders
  - Larger multi-institutional datasets
  - Real-time clinical deployment pipelines
