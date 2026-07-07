# Data Preprocessing

This notebook prepares the validated dataset for training. It cleans the data, formats it for the model, analyzes tokenization, and creates dataset subsets for experimentation.

---

## Overview

The preprocessing pipeline includes:

- Loading the validated dataset
- Cleaning the data
- Formatting sentence pairs
- Tokenization
- Sequence length analysis
- Truncation analysis
- Creating nested dataset subsets

These steps ensure the data is consistent and ready for fine-tuning.

---

## Preprocessing Pipeline

```
Raw Dataset
      │
      ▼
Data Cleaning
      │
      ▼
Sentence Formatting
      │
      ▼
Tokenization
      │
      ▼
Sequence Length Analysis
      │
      ▼
Truncation Check
      │
      ▼
Subset Generation
      │
      ▼
Processed Dataset
```

---

## Loading the Dataset

The notebook loads the validated dataset generated in the previous step.

Each sample contains:

- Premise
- Hypothesis
- Label

These are converted into the format expected by the tokenizer and model.

---

## Data Cleaning

Basic cleaning is performed before tokenization.

This includes:

- Removing invalid records
- Handling missing values (if any)
- Ensuring labels are valid
- Standardizing text formatting

The goal is to produce a consistent dataset without altering the meaning of the sentences.

---

## Tokenization

The project uses the **XLM-RoBERTa tokenizer**.

Each sentence pair is converted into token IDs that can be processed by the model.

Example:

```
Premise
        \
         --> Tokenizer --> Input IDs
        /
Hypothesis
```

The tokenizer also generates:

- Attention masks
- Token IDs
- Sequence lengths

---

## Sequence Length Analysis

The notebook analyzes the number of tokens in each sample.

This helps answer questions such as:

- What is the average sequence length?
- What is the longest sample?
- Are most samples much shorter than the maximum input length?

Understanding the token distribution helps choose an appropriate maximum sequence length.

---

## Truncation Analysis

Transformer models have a fixed maximum input length.

The notebook checks how many samples exceed this limit and would be truncated during training.

The analysis helps determine whether the chosen sequence length is suitable for the dataset.

---

## Nested Dataset Subsets

To support experiments with different training sizes, the notebook creates multiple subsets of the training data.

For example:

| Subset | Description |
|---------|-------------|
| Small | Small portion of the training set |
| Medium | Moderate amount of data |
| Large | Most of the training data |
| Full | Complete training dataset |

Using nested subsets allows training performance to be compared across different dataset sizes.

---

## Why Create Subsets?

Subset generation helps answer questions such as:

- Does more data improve performance?
- How do PEFT methods behave with limited data?
- Which method learns faster on smaller datasets?

Because the subsets are nested, larger datasets include all samples from the smaller ones, making comparisons more consistent.

---

## Output

After preprocessing, the notebook produces:

- Cleaned datasets
- Tokenized inputs
- Sequence length statistics
- Truncation statistics
- Dataset subsets for experiments

These outputs are used directly during model training.

---

## Validation

Before saving the processed data, the notebook verifies that:

- All samples were processed successfully
- Labels remain unchanged
- Tokenization completed without errors
- Dataset sizes match expectations

---

## Summary

At the end of this notebook, the dataset is fully prepared for training.

The processed data is:

- Clean
- Consistent
- Tokenized
- Analyzed
- Ready for fine-tuning experiments

---

## Next Step

Continue with **03-experimental-infrastructure-validation.ipynb**, where the training setup, PEFT configurations, and model pipeline are validated before running experiments.