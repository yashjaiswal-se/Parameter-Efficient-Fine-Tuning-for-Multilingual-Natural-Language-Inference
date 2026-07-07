# Dataset Validation

This notebook downloads the IndicXNLI dataset and performs a series of validation checks to ensure the data is clean and ready for preprocessing.

The goal is to identify common issues such as missing values, duplicate records, incorrect labels, or inconsistent file structures before starting model training.

---

## Dataset

The project uses the **IndicXNLI** dataset, which is a multilingual Natural Language Inference (NLI) benchmark.

Each sample contains:

- Premise
- Hypothesis
- Label

The labels represent the relationship between the two sentences.

| Label | Meaning |
|-------|----------|
| 0 | Entailment |
| 1 | Neutral |
| 2 | Contradiction |

---

## Validation Steps

The notebook performs the following checks:

- Load dataset files
- Verify file structure
- Check required columns
- Detect missing values
- Detect duplicate samples
- Verify label values
- Analyze class distribution
- Display sample records

These checks help ensure the dataset is suitable for preprocessing and training.

---

## Loading the Dataset

The dataset is loaded using the Hugging Face `datasets` library.

Example:

```python
from datasets import load_dataset

dataset = load_dataset(...)
```

Once loaded, the notebook displays the available dataset splits and the number of samples in each split.

---

## Dataset Structure

Each record should contain the following fields:

| Column | Description |
|----------|-------------|
| premise | First sentence |
| hypothesis | Second sentence |
| label | NLI class |

The notebook verifies that all required columns are present before continuing.

---

## Missing Value Check

Missing values can affect preprocessing and model training.

The notebook checks every column for null or empty values.

Expected result:

- No missing premises
- No missing hypotheses
- No missing labels

If missing values are found, they should be removed or handled before training.

---

## Duplicate Detection

Duplicate sentence pairs can introduce bias into the training process.

The notebook checks for duplicate rows and reports the total number found.

If duplicates exist, they can be removed during preprocessing.

---

## Label Validation

The notebook verifies that every label belongs to one of the expected classes.

Expected labels:

```
0
1
2
```

Unexpected label values indicate data corruption or formatting issues.

---

## Class Distribution

The distribution of labels is visualized to understand whether the dataset is balanced.

Example:

| Label | Samples |
|-------|----------|
| Entailment | ... |
| Neutral | ... |
| Contradiction | ... |

A balanced dataset generally leads to more reliable model training.

---

## Sample Inspection

A few dataset samples are displayed to manually verify that:

- sentences are readable
- labels are correct
- text formatting is consistent

This provides a quick sanity check before preprocessing.

---

## Validation Summary

After all checks are completed, the dataset should satisfy the following conditions:

- Dataset loaded successfully
- Required columns are present
- No missing values
- Valid labels
- Duplicate count recorded
- Class distribution inspected

If all checks pass, the dataset is ready for preprocessing.

---

## Next Step

After validating the dataset, continue with **02-data-preprocessingV2.ipynb**.

The next notebook cleans the data, prepares it for model input, and generates the processed datasets used for training.