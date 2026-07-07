# Parameter-Efficient Fine-Tuning for Multilingual Natural Language Inference

This repository contains the complete experimental pipeline used to investigate **Parameter-Efficient Fine-Tuning (PEFT)** methods for multilingual Natural Language Inference (NLI) on Indic languages.

The project implements an end-to-end workflow covering:

- Environment setup
- Dataset acquisition and validation
- Dataset preprocessing
- Tokenization analysis
- Experimental infrastructure validation
- PEFT model configuration
- Training readiness verification

The experiments are built around **XLM-RoBERTa Base** and the **IndicXNLI** benchmark.

---

# Objectives

The primary objective of this project is to compare modern PEFT techniques for multilingual sentence-pair classification while maintaining a fully reproducible experimental pipeline.

The repository emphasizes:

- reproducibility
- modular preprocessing
- transparent validation
- experiment readiness before training
- consistent dataset generation

---

# Repository Structure

```
.
├── notebooks/
│   ├── 00-environment-setup.ipynb
│   ├── 01-dataset-validation.ipynb
│   ├── 02-data-preprocessingV2.ipynb
│   └── 03-experimental-infrastructure-validation.ipynb
│
├── docs/
│   ├── environment.md
│   ├── dataset-validation.md
│   ├── preprocessing.md
│   ├── experimental-validation.md
│   └── project-structure.md
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── subsets/
│
├── models/
│
├── outputs/
│
└── README.md
```

---

# Experimental Pipeline

```text
GPU Verification
        │
        ▼
Environment Setup
        │
        ▼
Download IndicXNLI
        │
        ▼
Dataset Validation
        │
        ▼
Preprocessing
        │
        ▼
Tokenizer Analysis
        │
        ▼
Nested Dataset Generation
        │
        ▼
PEFT Configuration
        │
        ▼
Infrastructure Validation
        │
        ▼
Training
        │
        ▼
Evaluation
```

---

# Model

Base model:

- XLM-RoBERTa Base

Task:

- Natural Language Inference

Number of classes:

| Label | Meaning |
|--------|----------|
| 0 | Entailment |
| 1 | Neutral |
| 2 | Contradiction |

---

# Dataset

The experiments use the **IndicXNLI** benchmark.

Languages currently included:

| Language | Split |
|-----------|-------|
| Hindi | Train / Dev / Test |
| Telugu | Train / Dev / Test |

Each split is validated before preprocessing to ensure:

- valid JSON structure
- expected columns
- label integrity
- missing value detection
- duplicate detection
- class balance inspection

---

# PEFT Methods

The repository is designed to compare multiple parameter-efficient fine-tuning strategies.

Current implementations include:

- Full Fine-Tuning
- LoRA
- DoRA
- IA³

Each configuration is validated before training.

---

# Environment

Experiments require:

- Python 3.10+
- CUDA-enabled GPU
- PyTorch
- Transformers
- Datasets
- PEFT
- Accelerate
- Evaluate

See:

```
docs/environment.md
```

for complete installation instructions.

---

# Reproducibility

The notebooks establish reproducibility through:

- fixed random seeds
- deterministic preprocessing
- explicit dataset validation
- tokenizer consistency checks
- infrastructure verification before training

---

# Documentation

Additional documentation is available under the `docs/` directory.

| Document | Description |
|----------|-------------|
| environment.md | Installation and environment configuration |
| dataset-validation.md | Dataset acquisition and validation |
| preprocessing.md | Preprocessing pipeline |
| experimental-validation.md | PEFT infrastructure validation |
| project-structure.md | Repository organization |

---

# Workflow

Run the notebooks in the following order:

1. `00-environment-setup.ipynb`
2. `01-dataset-validation.ipynb`
3. `02-data-preprocessingV2.ipynb`
4. `03-experimental-infrastructure-validation.ipynb`

Each notebook validates its outputs before proceeding to the next stage.

---

# Project Status

Current implementation includes:

- Environment validation
- GPU verification
- Dataset download
- Dataset integrity validation
- JSON loading utilities
- Preprocessing pipeline
- Tokenization analysis
- PEFT configuration
- Experimental infrastructure validation

Training and evaluation notebooks can be added independently without modifying the preprocessing pipeline.

---


# Citation

If this repository contributes to academic work, please cite the repository and the original datasets and model authors.
