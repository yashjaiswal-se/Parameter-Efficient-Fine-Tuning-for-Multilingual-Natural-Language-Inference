# Environment Setup

This notebook sets up the development environment required for the project. It installs the necessary libraries, checks GPU availability, and verifies that the required models and packages can be loaded successfully before moving to dataset processing and training.

## Requirements

The project was developed using:

- Python 3.10+
- PyTorch
- Transformers
- Datasets
- PEFT
- Accelerate
- Evaluate
- Pandas
- NumPy
- Matplotlib

Install the required packages using:

```bash
pip install torch transformers datasets peft accelerate evaluate pandas numpy matplotlib scikit-learn
```

---

## GPU Check

The notebook first checks whether a CUDA-enabled GPU is available.

```python
import torch

print(torch.cuda.is_available())
print(torch.cuda.get_device_name(0))
```

This helps confirm that training can use GPU acceleration.

---

## Installing Dependencies

The notebook installs and imports all required libraries from the Hugging Face ecosystem.

Main libraries include:

- `torch`
- `transformers`
- `datasets`
- `peft`
- `accelerate`

These libraries are used throughout the project for preprocessing, model loading, and fine-tuning.

---

## Loading the Model

After the environment is ready, the notebook loads the pretrained **XLM-RoBERTa** model along with its tokenizer.

This step verifies that:

- the model downloads successfully
- the tokenizer loads correctly
- all dependencies are working as expected

---

## PEFT Support

The project uses the PEFT library to experiment with different parameter-efficient fine-tuning methods.

The environment setup confirms that PEFT is installed correctly before any experiments begin.

The methods used later in the project include:

- LoRA
- DoRA
- IA³

---

## Output

If everything runs successfully, the notebook confirms that:

- Python environment is ready
- GPU is detected (if available)
- Required libraries are installed
- Model and tokenizer load without errors
- The system is ready for dataset processing

---

## Next Step

Once the environment setup is complete, continue with **01-dataset-validation.ipynb**, where the IndicXNLI dataset is downloaded and validated before preprocessing.