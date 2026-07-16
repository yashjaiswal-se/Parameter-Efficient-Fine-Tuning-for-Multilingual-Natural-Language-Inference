# Architecture Overview

**Version:** 1.0\
**Status:** Stable

------------------------------------------------------------------------

# Purpose

This document provides a high-level architectural view of the training
framework. It describes the major subsystems, their responsibilities,
interactions, and the design principles governing the overall
implementation.

------------------------------------------------------------------------

# System Architecture

``` text
                    Experiment Definition
                            │
                            ▼
                  ExperimentManager
                            │
                            ▼
                  ExperimentConfig
                            │
                            ▼
              ConfigurationValidator
                            │
                            ▼
                 ExperimentContext
                            │
        ┌───────────────────┼────────────────────┐
        ▼                   ▼                    ▼
 DatasetManager      TokenizerManager      ModelFactory
        │                   │                    │
        └──────────────┬────┴──────────────┬─────┘
                       ▼                   ▼
                 TrainingManager      Trainer
                       │                   │
                       └──────────┬────────┘
                                  ▼
                             Evaluator
                                  │
                                  ▼
                          ExperimentResult
```

------------------------------------------------------------------------

# Architectural Layers

## Configuration Layer

Responsible for defining and validating all experiment settings.

Components:

-   ExperimentManager
-   ExperimentConfig
-   ConfigurationValidator

Responsibilities:

-   Create experiment configurations
-   Validate configuration integrity
-   Ensure reproducibility

------------------------------------------------------------------------

## Data Layer

Responsible for data ingestion and preparation.

Components:

-   DatasetManager
-   TokenizerManager

Responsibilities:

-   Load datasets
-   Validate schema
-   Tokenize samples
-   Create DataLoaders

------------------------------------------------------------------------

## Model Layer

Responsible for model construction.

Component:

-   ModelFactory

Responsibilities:

-   Load pretrained backbone
-   Configure classifier
-   Apply FFT or PEFT

Supported training methods:

-   FFT
-   LoRA
-   DoRA
-   IA³

------------------------------------------------------------------------

## Optimization Layer

Responsible for model optimization.

Components:

-   TrainingManager
-   Trainer

Responsibilities:

-   Build optimizer
-   Build scheduler
-   Mixed precision
-   Training loop
-   Validation loop

------------------------------------------------------------------------

## Evaluation Layer

Responsible for performance assessment.

Component:

-   Evaluator

Computes:

-   Loss
-   Accuracy
-   Precision
-   Recall
-   F1 Score

------------------------------------------------------------------------

## Orchestration Layer

Component:

-   ExperimentRunner

Responsibilities:

-   Coordinate all framework components
-   Execute complete experiment lifecycle
-   Produce ExperimentResult

------------------------------------------------------------------------

# Dependency Graph

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

Dependencies are unidirectional to minimize coupling.

------------------------------------------------------------------------

# Design Principles

## Separation of Concerns

Each class owns a single responsibility.

## Modular Design

Components can be modified independently without affecting unrelated
subsystems.

## Reproducibility

All experiments are driven by immutable configuration objects.

## Extensibility

New datasets, models, PEFT methods, or evaluation metrics can be added
with minimal changes.

## Testability

Each component supports independent unit testing and integration
testing.

------------------------------------------------------------------------

# Data Flow

``` text
Parquet Files
      │
      ▼
Dataset
      │
      ▼
Tokenized Dataset
      │
      ▼
DataLoader
      │
      ▼
Model
      │
      ▼
Training
      │
      ▼
Evaluation
      │
      ▼
Experiment Result
```

------------------------------------------------------------------------

# Runtime Flow

``` text
Create Configuration
        ▼
Validate Configuration
        ▼
Prepare Runtime
        ▼
Load Dataset
        ▼
Tokenize Data
        ▼
Build Model
        ▼
Create Optimizer
        ▼
Train
        ▼
Validate
        ▼
Evaluate
        ▼
Return Results
```

------------------------------------------------------------------------

# Current Architecture Status

  Subsystem                  Status
  -------------------------- ----------
  Configuration              Complete
  Data Pipeline              Complete
  Tokenization               Complete
  Model Construction         Complete
  Optimization               Complete
  Evaluation                 Complete
  Experiment Orchestration   Complete
  Integration Testing        Complete

------------------------------------------------------------------------

# Planned Extensions

-   Best-model checkpointing
-   Early stopping
-   Experiment logging
-   Hyperparameter search
-   Multi-seed execution
-   Distributed training
-   Automated experiment matrix

------------------------------------------------------------------------

# Conclusion

The framework follows a layered, modular architecture with clearly
defined responsibilities and one-way dependencies. This design supports
reproducible experimentation, simplifies maintenance, and provides a
scalable foundation for comparative research involving multiple
fine-tuning strategies and datasets.
