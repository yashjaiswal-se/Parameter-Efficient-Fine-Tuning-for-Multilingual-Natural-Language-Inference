# Training Workflow

**Version:** 1.0\
**Status:** Stable

------------------------------------------------------------------------

# Purpose

This document describes the end-to-end execution workflow of the
training framework. It explains how an experiment progresses from
configuration to final evaluation and how each framework component
interacts throughout the training lifecycle.

------------------------------------------------------------------------

# End-to-End Workflow

``` text
Experiment Configuration
        │
        ▼
Configuration Validation
        │
        ▼
Experiment Context Creation
        │
        ▼
Dataset Loading
        │
        ▼
Dataset Validation
        │
        ▼
Tokenization
        │
        ▼
PyTorch Dataset Preparation
        │
        ▼
DataLoader Creation
        │
        ▼
Model Construction
        │
        ▼
PEFT Injection (Optional)
        │
        ▼
Training Utility Creation
        │
        ▼
Training
        │
        ▼
Validation
        │
        ▼
Testing
        │
        ▼
Experiment Result
```

------------------------------------------------------------------------

# Workflow Stages

## 1. Configuration

### Input

-   ExperimentConfig

### Processing

-   Build experiment metadata
-   Define dataset paths
-   Configure model
-   Configure PEFT
-   Configure training
-   Configure runtime

### Output

Validated configuration object.

------------------------------------------------------------------------

## 2. Configuration Validation

### Processing

-   Validate experiment metadata
-   Validate dataset paths
-   Validate model parameters
-   Validate PEFT configuration
-   Validate training configuration
-   Validate runtime configuration
-   Validate output configuration

### Output

Validated configuration or exception.

------------------------------------------------------------------------

## 3. Experiment Context

The validated configuration is wrapped inside an `ExperimentContext`.

The context becomes the shared state passed to all framework components.

------------------------------------------------------------------------

## 4. Dataset Loading

Component:

-   DatasetManager

### Processing

-   Read Parquet files
-   Validate dataset schema
-   Convert DataFrame to Hugging Face Dataset

### Output

-   Train Dataset
-   Validation Dataset
-   Test Dataset

------------------------------------------------------------------------

## 5. Tokenization

Component:

-   TokenizerManager

### Processing

-   Load tokenizer
-   Tokenize dataset
-   Apply truncation
-   Create attention masks
-   Rename label column to `labels`
-   Convert to PyTorch format

### Output

Tokenized datasets.

------------------------------------------------------------------------

## 6. DataLoader Creation

Component:

-   TokenizerManager

### Processing

-   Create DataCollatorWithPadding
-   Build train DataLoader
-   Build validation DataLoader
-   Build test DataLoader

### Output

Three DataLoader instances.

------------------------------------------------------------------------

## 7. Model Construction

Component:

-   ModelFactory

### Processing

-   Load pretrained backbone
-   Configure classifier
-   Apply selected training strategy

Supported strategies:

-   Full Fine-Tuning (FFT)
-   LoRA
-   DoRA
-   IA³

### Output

Trainable model.

------------------------------------------------------------------------

## 8. Training Utility Construction

Component:

-   TrainingManager

### Creates

-   Optimizer
-   Learning-rate scheduler
-   Gradient scaler

### Output

TrainingBundle

------------------------------------------------------------------------

## 9. Training Loop

Component:

-   Trainer

``` text
for each epoch

    model.train()

    for each batch

        move batch to device

        zero gradients

        forward pass

        compute loss

        backward pass

        optimizer step

        scheduler step

    validate model

    update training history
```

### Output

TrainingHistory

------------------------------------------------------------------------

## 10. Validation

Executed after every epoch.

Processing:

-   Switch model to evaluation mode
-   Disable gradient computation
-   Compute validation loss
-   Record epoch statistics

------------------------------------------------------------------------

## 11. Testing

Component:

-   Evaluator

### Processing

-   Forward pass on test dataset
-   Collect predictions
-   Compute metrics

Metrics:

-   Loss
-   Accuracy
-   Precision
-   Recall
-   F1 Score

### Output

EvaluationResult

------------------------------------------------------------------------

## 12. Experiment Completion

Component:

-   ExperimentRunner

Returns an ExperimentResult containing:

-   Configuration
-   Training history
-   Evaluation metrics
-   Runtime statistics

------------------------------------------------------------------------

# Data Flow

``` text
Parquet Files
      │
      ▼
Hugging Face Dataset
      │
      ▼
Tokenized Dataset
      │
      ▼
PyTorch Dataset
      │
      ▼
DataLoader
      │
      ▼
Mini-batch
      │
      ▼
Model
      │
      ▼
Predictions
      │
      ▼
Metrics
```

------------------------------------------------------------------------

# Component Interaction

``` text
ExperimentRunner
        │
        ├── ConfigurationValidator
        ├── DatasetManager
        ├── TokenizerManager
        ├── ModelFactory
        ├── TrainingManager
        ├── Trainer
        └── Evaluator
```

------------------------------------------------------------------------

# Error Handling

Errors are handled at component boundaries.

  Component                Exception
  ------------------------ ------------------------
  ConfigurationValidator   ValueError / TypeError
  DatasetManager           DatasetError
  TokenizerManager         TokenizationError
  ModelFactory             ModelError
  TrainingManager          TrainingError
  Trainer                  TrainerError
  Evaluator                EvaluationError

------------------------------------------------------------------------

# Outputs

The workflow generates:

-   TrainingHistory
-   EvaluationResult
-   ExperimentResult

These objects provide all information required for experiment analysis,
comparison, and reporting.

------------------------------------------------------------------------

# Conclusion

The workflow follows a linear and modular execution model. Each
component performs a single responsibility and passes standardized
outputs to the next stage, enabling reproducible, maintainable, and
extensible transformer training experiments.
