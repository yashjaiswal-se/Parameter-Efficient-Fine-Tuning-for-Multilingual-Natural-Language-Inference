# Project Structure

This document describes the organization of the repository and the role of each component in the experimental pipeline.

---

# Directory Layout

```
.
├── notebooks/
├── docs/
├── data/
├── outputs/
├── models/
└── README.md
```

---

# Notebooks

## 00 — Environment Setup

Purpose:

- verify GPU availability
- install required packages
- validate PEFT installation
- verify model loading

Outputs:

- configured runtime
- validated software environment

---

## 01 — Dataset Validation

Purpose:

- download IndicXNLI
- load JSON datasets
- inspect schema
- verify labels
- inspect class distribution
- detect missing values

Outputs:

- validated raw datasets

---

## 02 — Data Preprocessing

Purpose:

- clean datasets
- normalize fields
- prepare model inputs
- generate processed datasets
- create nested experimental subsets
- analyze tokenizer behaviour

Outputs:

- processed datasets
- tokenizer statistics
- subset datasets

---

## 03 — Experimental Infrastructure Validation

Purpose:

- verify training infrastructure
- configure PEFT adapters
- compare trainable parameters
- validate training pipeline

Outputs:

- validated experimental configurations

---

# Data Flow

```
Raw Dataset
      │
      ▼
Validation
      │
      ▼
Cleaning
      │
      ▼
Preprocessing
      │
      ▼
Tokenizer Analysis
      │
      ▼
Dataset Subsets
      │
      ▼
Training Ready Dataset
```

---

# Data Directory

Recommended structure:

```
data/

├── raw/
│
├── processed/
│
├── subsets/
│
└── analysis/
```

## raw

Contains downloaded datasets without modification.

Examples:

```
train.json
dev.json
test.json
```

---

## processed

Contains cleaned datasets after preprocessing.

Examples:

```
train_processed.csv
dev_processed.csv
test_processed.csv
```

---

## subsets

Contains nested datasets used for scaling experiments.

Typical subsets include progressively larger portions of the training set while preserving label distributions.

---

## analysis

Stores intermediate analytical outputs such as:

- tokenizer statistics
- sequence length distributions
- truncation analysis
- vocabulary coverage

---

# Models

```
models/

├── checkpoints/
├── adapters/
└── final/
```

The separation allows:

- multiple PEFT methods
- checkpoint comparison
- reproducible evaluation

---

# Outputs

```
outputs/

├── logs/
├── metrics/
├── figures/
└── predictions/
```

Recommended contents include:

- training logs
- evaluation metrics
- plots
- confusion matrices
- prediction files

---

# Documentation

The `docs/` directory contains detailed technical documentation for every stage of the pipeline.

```
docs/

environment.md
dataset-validation.md
preprocessing.md
experimental-validation.md
project-structure.md
```

---

# Recommended Execution Order

```
00-environment-setup.ipynb

        ↓

01-dataset-validation.ipynb

        ↓

02-data-preprocessingV2.ipynb

        ↓

03-experimental-infrastructure-validation.ipynb
```

Each stage assumes the successful completion of the previous stage.

---

# Design Principles

The repository is organized around the following principles:

- Modular notebooks with clearly defined responsibilities.
- Reproducible preprocessing before model training.
- Validation at every major stage.
- Separation of raw, processed, and generated artifacts.
- Independent documentation for each pipeline component.

This structure supports reproducibility, simplifies debugging, and allows individual components of the pipeline to be reused in future experiments.