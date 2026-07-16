# Experiment Lifecycle

**Version:** 1.0\
**Status:** Stable

------------------------------------------------------------------------

# Purpose

This document describes the complete lifecycle of an experiment executed
using the training framework. It defines how an experiment is
configured, executed, evaluated, and how its outputs are organized for
reproducibility and comparison.

------------------------------------------------------------------------

# Lifecycle Overview

``` text
Experiment Definition
        │
        ▼
Configuration Creation
        │
        ▼
Configuration Validation
        │
        ▼
Experiment Context
        │
        ▼
Dataset Preparation
        │
        ▼
Model Preparation
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
Result Collection
        │
        ▼
Experiment Completion
```

------------------------------------------------------------------------

# Stage 1 --- Experiment Definition

Every experiment begins by defining:

-   Experiment name
-   Description
-   Dataset
-   Language
-   Budget
-   Training method
-   Hyperparameters
-   Runtime configuration

The `ExperimentManager` generates a complete `ExperimentConfig` from
these inputs.

------------------------------------------------------------------------

# Stage 2 --- Configuration Validation

Before execution, the framework validates:

-   Experiment metadata
-   Dataset paths
-   Model configuration
-   PEFT configuration
-   Training configuration
-   Runtime configuration
-   Output configuration

Execution terminates immediately if validation fails.

------------------------------------------------------------------------

# Stage 3 --- Context Initialization

An `ExperimentContext` is created to provide shared state across all
framework components.

The context stores:

-   Experiment configuration
-   Runtime information
-   Device information
-   Output directories

------------------------------------------------------------------------

# Stage 4 --- Dataset Preparation

Pipeline:

``` text
Parquet Files
      │
      ▼
DatasetManager
      │
      ▼
Hugging Face Dataset
      │
      ▼
TokenizerManager
      │
      ▼
Tokenized Dataset
      │
      ▼
DataLoader
```

Outputs:

-   Train DataLoader
-   Validation DataLoader
-   Test DataLoader

------------------------------------------------------------------------

# Stage 5 --- Model Preparation

The `ModelFactory` performs:

1.  Backbone loading
2.  Classification head creation
3.  PEFT injection (if applicable)

Supported training strategies:

-   Full Fine-Tuning (FFT)
-   LoRA
-   DoRA
-   IA³

The resulting model is moved to the configured device.

------------------------------------------------------------------------

# Stage 6 --- Training Preparation

The `TrainingManager` constructs:

-   Optimizer
-   Learning-rate scheduler
-   Gradient scaler

These are packaged into a `TrainingBundle` used by the trainer.

------------------------------------------------------------------------

# Stage 7 --- Training

For each epoch:

``` text
Training
    │
    ├── Forward Pass
    ├── Loss Computation
    ├── Backpropagation
    ├── Optimizer Update
    ├── Scheduler Update
    └── Loss Recording
```

Validation is executed immediately after each training epoch.

Outputs:

-   Training loss
-   Validation loss
-   Epoch duration

------------------------------------------------------------------------

# Stage 8 --- Evaluation

After training completes:

-   Load trained model
-   Evaluate on test dataset
-   Generate predictions
-   Compute evaluation metrics

Metrics:

-   Loss
-   Accuracy
-   Precision
-   Recall
-   F1 Score

------------------------------------------------------------------------

# Stage 9 --- Result Generation

The framework creates an `ExperimentResult` containing:

-   Experiment configuration
-   Training history
-   Evaluation results
-   Runtime statistics

This object serves as the canonical record of the experiment.

------------------------------------------------------------------------

# Experiment Outputs

Current outputs include:

-   TrainingHistory
-   EvaluationResult
-   ExperimentResult

Future extensions may include:

-   Best model checkpoint
-   Training logs
-   Learning curves
-   Prediction files
-   Confusion matrices

------------------------------------------------------------------------

# Reproducibility

Experiments are designed to be reproducible through:

-   Typed configuration objects
-   Fixed random seed
-   Configuration validation
-   Standardized execution pipeline
-   Immutable experiment metadata

------------------------------------------------------------------------

# Failure Handling

The lifecycle terminates safely when failures occur during:

-   Configuration validation
-   Dataset loading
-   Tokenization
-   Model construction
-   Training
-   Evaluation

Each subsystem raises domain-specific exceptions to simplify debugging.

------------------------------------------------------------------------

# Lifecycle Summary

``` text
ExperimentManager
        │
        ▼
ExperimentConfig
        │
        ▼
ExperimentRunner
        │
        ├── DatasetManager
        ├── TokenizerManager
        ├── ModelFactory
        ├── TrainingManager
        ├── Trainer
        └── Evaluator
        │
        ▼
ExperimentResult
```

------------------------------------------------------------------------

# Conclusion

The experiment lifecycle provides a standardized execution process from
experiment definition through final evaluation. Every experiment follows
the same validated sequence of operations, ensuring consistency,
reproducibility, and comparability across different datasets, training
strategies, and hyperparameter configurations.
