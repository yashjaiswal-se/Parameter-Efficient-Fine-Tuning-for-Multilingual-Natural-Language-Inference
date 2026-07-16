# Training Framework

**Version:** 1.0\
**Status:** Stable\
**Scope:** End-to-end supervised training framework for
transformer-based text classification using Full Fine-Tuning (FFT) and
Parameter-Efficient Fine-Tuning (PEFT).

------------------------------------------------------------------------

# Purpose

The Training Framework provides a modular, reproducible, and extensible
infrastructure for transformer-based text classification experiments.
The framework separates configuration, data preparation, model
construction, optimization, evaluation, and experiment orchestration
into independent components with clearly defined responsibilities.

Design goals:

-   Separation of concerns
-   Reproducibility
-   Configurability
-   Testability
-   Extensibility

------------------------------------------------------------------------

# High-Level Architecture

``` text
ExperimentRunner
        │
        ▼
Configuration Validation
        │
        ▼
DatasetManager
        │
        ▼
TokenizerManager
        │
        ▼
ModelFactory
        │
        ▼
TrainingManager
        │
        ▼
Trainer
        │
        ▼
Evaluator
        │
        ▼
ExperimentResult
```

------------------------------------------------------------------------

# Core Components

## Configuration Layer

The framework is driven by strongly typed configuration objects:

-   ExperimentInfo
-   DatasetConfig
-   ModelConfig
-   PEFTConfig
-   TrainingConfig
-   RuntimeConfig
-   OutputConfig
-   ExperimentConfig

Configuration validation is performed before experiment execution.

------------------------------------------------------------------------

## ExperimentManager

### Responsibilities

-   Create complete experiment configurations
-   Standardize experiment defaults
-   Reduce repetitive configuration code
-   Improve experiment reproducibility

------------------------------------------------------------------------

## DatasetManager

### Responsibilities

-   Load Parquet datasets
-   Validate dataset schema
-   Convert pandas DataFrames to Hugging Face Datasets

### Excludes

-   Tokenization
-   DataLoader creation
-   Model-specific preprocessing

------------------------------------------------------------------------

## TokenizerManager

### Responsibilities

-   Load tokenizer
-   Tokenize datasets
-   Convert datasets to PyTorch format
-   Build train, validation and test DataLoaders
-   Apply dynamic batch padding

### Outputs

-   Tokenizer
-   Train DataLoader
-   Validation DataLoader
-   Test DataLoader

------------------------------------------------------------------------

## ModelFactory

### Responsibilities

-   Load pretrained transformer backbone
-   Configure sequence-classification model
-   Apply selected training strategy

Supported methods:

-   Full Fine-Tuning (FFT)
-   LoRA
-   DoRA
-   IA³

------------------------------------------------------------------------

## TrainingManager

### Responsibilities

Construct training utilities only.

Creates:

-   Optimizer
-   Learning-rate scheduler
-   Mixed-precision gradient scaler

Does not execute training.

------------------------------------------------------------------------

## Trainer

### Responsibilities

-   Batch training
-   Epoch training
-   Validation
-   Mixed-precision execution
-   Training history collection

Training sequence:

``` text
Batch
 ↓
Move to Device
 ↓
Forward Pass
 ↓
Loss Computation
 ↓
Backward Pass
 ↓
Optimizer Step
 ↓
Scheduler Step
```

Output:

-   TrainingHistory

------------------------------------------------------------------------

## Evaluator

### Responsibilities

Evaluate trained models without updating parameters.

Metrics:

-   Loss
-   Accuracy
-   Precision
-   Recall
-   F1 Score

Output:

-   EvaluationResult

------------------------------------------------------------------------

## ExperimentRunner

Coordinates the complete experiment lifecycle.

Pipeline:

``` text
Validate Configuration
        ↓
Load Dataset
        ↓
Tokenize Dataset
        ↓
Build Model
        ↓
Construct Training Utilities
        ↓
Train Model
        ↓
Evaluate Model
        ↓
Return ExperimentResult
```

------------------------------------------------------------------------

# Training Pipeline

``` text
ExperimentConfig
        ↓
ConfigurationValidator
        ↓
ExperimentContext
        ↓
DatasetManager
        ↓
TokenizerManager
        ↓
ModelFactory
        ↓
TrainingManager
        ↓
Trainer
        ↓
Evaluator
        ↓
ExperimentResult
```

------------------------------------------------------------------------

# Design Principles

## Separation of Concerns

  Component           Responsibility
  ------------------- ------------------------------
  ExperimentManager   Experiment configuration
  DatasetManager      Dataset loading
  TokenizerManager    Tokenization and DataLoaders
  ModelFactory        Model construction
  TrainingManager     Optimization utilities
  Trainer             Model optimization
  Evaluator           Model evaluation
  ExperimentRunner    Experiment orchestration

## Dependency Flow

``` text
Configuration
      ↓
Managers
      ↓
Trainer
      ↓
Evaluator
```

Dependencies flow in one direction to reduce coupling.

------------------------------------------------------------------------

# Validation

The framework has been validated using:

## Unit Validation

-   Configuration validation
-   Dataset loading
-   Tokenization
-   Model construction
-   Training utility creation
-   Trainer
-   Evaluator

## Integration Validation

Validated complete execution of:

-   Dataset loading
-   Tokenization
-   Model construction
-   Training
-   Validation
-   Evaluation
-   ExperimentRunner

------------------------------------------------------------------------

# Current Capabilities

-   Typed configuration system
-   Automatic validation
-   Hugging Face Dataset integration
-   Dynamic tokenization
-   Dynamic padding
-   PEFT support
-   Mixed precision
-   Learning-rate scheduling
-   Modular evaluation
-   End-to-end experiment execution

------------------------------------------------------------------------

# Current Limitations

-   No best-model checkpointing
-   No early stopping
-   No resume training
-   No TensorBoard integration
-   No distributed multi-GPU execution
-   No hyperparameter search
-   No automated experiment logging

------------------------------------------------------------------------

# Planned Improvements

-   Best validation checkpointing
-   Experiment logging
-   Learning-curve visualization
-   Hyperparameter search
-   Multi-seed evaluation
-   Distributed training

------------------------------------------------------------------------

# Framework Status

  Component             Status
  --------------------- ----------
  Configuration         Complete
  Dataset Pipeline      Complete
  Tokenization          Complete
  Model Factory         Complete
  Training Manager      Complete
  Trainer               Complete
  Evaluator             Complete
  Experiment Manager    Complete
  Experiment Runner     Complete
  Integration Testing   Complete

------------------------------------------------------------------------

# Conclusion

The training framework provides a complete, modular, and validated
infrastructure for transformer-based text classification research. The
architecture emphasizes maintainability, reproducibility, and
extensibility through clearly separated responsibilities and
standardized experiment orchestration. The framework has been validated
through unit tests, integration tests, and successful end-to-end
execution of both PEFT and Full Fine-Tuning experiments.
