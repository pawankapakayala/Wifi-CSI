# Wi-Fi CSI Human Activity Recognition — CNN-LSTM Baseline

This repository contains a clean and reproducible implementation of a **CNN-LSTM model for human activity recognition using Wi-Fi Channel State Information (CSI)**.

The current version uses the **UT-HAR dataset** as a baseline experiment and is intended to establish a reliable deep-learning pipeline before reproducing the results of the target research paper.

> **Note:** This implementation is a baseline using UT-HAR. It is **not an exact reproduction of the target paper**, as the paper uses a different custom dataset, preprocessing pipeline, and training configuration.

---

## Project Objective

The main objective of this project is to study how Wi-Fi CSI can be used for **non-intrusive human activity recognition** using deep learning.

The workflow includes:

* Loading and verifying CSI data
* Preprocessing and normalization
* Converting CSI data into PyTorch tensors
* Training a CNN-LSTM model
* Monitoring training and validation performance
* Evaluating the model on unseen test data
* Generating classification reports and confusion matrices
* Comparing the obtained results with the target research paper

---

## Current Version

### Version: UT-HAR CNN-LSTM Baseline

This version focuses only on establishing a clean **CNN-LSTM baseline** using the UT-HAR dataset.

The notebook has been reorganized to remove duplicated code, unnecessary debugging cells, and scattered configuration sections.

### Notebook

`UT_HAR_CNN_LSTM_Baseline_Cleaned.ipynb`

---

## Dataset

The current implementation uses the **UT-HAR dataset**, a publicly available Wi-Fi CSI human activity recognition dataset.

The dataset contains CSI measurements corresponding to different human activities, including:

* Lie Down
* Fall
* Walk
* Pickup
* Run
* Sit Down
* Stand Up

The data is represented using CSI measurements across multiple subcarriers and time samples.

---

## Model Architecture

The baseline model follows a **CNN → LSTM → Fully Connected** architecture.

### CNN Feature Extraction

The CNN layers learn spatial/features patterns from the CSI representation.

```text
Input CSI
   ↓
Conv2D
   ↓
Batch Normalization
   ↓
ReLU
   ↓
Max Pooling
   ↓
Conv2D
   ↓
Batch Normalization
   ↓
ReLU
   ↓
Max Pooling
   ↓
Conv2D
   ↓
Batch Normalization
   ↓
ReLU
```

### Temporal Modeling

The extracted CNN features are rearranged into a sequence and passed to a two-layer LSTM.

```text
CNN Features
     ↓
Sequence Formation
     ↓
LSTM
     ↓
LSTM
     ↓
Dropout
     ↓
Fully Connected Layer
     ↓
Output Classes
```

The LSTM component helps the model learn temporal patterns present in CSI measurements.

---

## Training Pipeline

The notebook follows this workflow:

```text
UT-HAR Dataset
      ↓
Dataset Verification
      ↓
Train / Validation / Test Data
      ↓
Train-only Normalization
      ↓
PyTorch Tensors
      ↓
DataLoaders
      ↓
CNN-LSTM
      ↓
Training
      ↓
Best Validation Checkpoint
      ↓
Test Evaluation
      ↓
Classification Report
      ↓
Confusion Matrix
```

### Important preprocessing decision

Normalization statistics are calculated using the **training data only**.

This prevents information from the validation or test sets from leaking into the training process.

---

## Training Monitoring

The notebook records:

* Training loss
* Validation loss
* Training accuracy
* Validation accuracy

These values are plotted after training to observe the learning behaviour of the model.

The best-performing model on the validation set is saved and used for final test evaluation.

---

## Baseline Results

The current UT-HAR CNN-LSTM experiment achieved approximately:

| Metric          |     Result |
| --------------- | ---------: |
| Test Accuracy   | **98.00%** |
| Macro Precision | **96.80%** |
| Macro Recall    | **97.04%** |
| Macro F1-Score  | **96.91%** |

### Fall Detection

| Metric    |      Result |
| --------- | ----------: |
| Precision |  **97.83%** |
| Recall    | **100.00%** |
| F1-Score  |  **98.90%** |

These results are specific to the **UT-HAR dataset and this implementation**.

They should **not be interpreted as a reproduction of the target paper's reported results**.

---

## Training Accuracy Fluctuations

During training, the validation accuracy showed noticeable fluctuations in some epochs.

For example, the model reached high validation accuracy in some epochs and then temporarily dropped before recovering.

This behaviour was investigated during the notebook cleanup.

Possible causes include:

* Small validation set
* Dataset characteristics
* Learning-rate updates
* Model sensitivity to the training batches
* Difference between training and validation distributions

To make the experiment more reliable, the implementation:

* Saves the best validation checkpoint
* Separates training and evaluation clearly
* Uses fixed random seeds where possible
* Uses train-only normalization
* Tracks both loss and accuracy
* Evaluates the final model only on the test set

The fluctuations are therefore documented rather than hidden.

---

## Comparison With Target Paper

The target research paper reports results using a **different custom Wi-Fi CSI dataset and experimental setup**.

Some reported values from the paper are:

| Model / Configuration | Paper Accuracy |
| --------------------- | -------------: |
| CNN-LSTM              |         92.65% |
| CNN-LSTM + PCA        |         94.85% |
| GNN + PCA             |         85.59% |
| Transformer           |         71.32% |
| Transformer + PCA     |         83.09% |

The current UT-HAR result of approximately **98% cannot be directly compared with these values as an equivalent reproduction**, because the dataset, preprocessing, model configuration, and training setup are different.

The purpose of this baseline is to verify that the complete CSI → preprocessing → PyTorch → CNN-LSTM → evaluation pipeline works correctly before moving to exact paper reproduction.

---

## Repository Structure

```text
.
├── UT_HAR_CNN_LSTM_Baseline_Cleaned.ipynb
├── README.md
└── data/
    └── UT-HAR/
        └── ...
```

> Dataset files are not included in this repository if their redistribution is restricted. Please obtain the dataset from its original source.

---

## Technologies Used

* Python
* PyTorch
* NumPy
* Pandas
* Matplotlib
* Scikit-learn
* Jupyter Notebook

---

## Reproducibility

The notebook includes a centralized configuration section for:

* Random seed
* Batch size
* Learning rate
* Number of epochs
* Dataset paths
* Device selection

The goal is to make future experiments easier to reproduce and compare.

---

## Next Steps

The next stage of this research is to reproduce the target paper more closely.

### Planned workflow

```text
UT-HAR CNN-LSTM Baseline
          ↓
Obtain Target Paper Dataset
          ↓
Verify Dataset Structure
          ↓
Reproduce Paper Preprocessing
          ↓
Implement Paper CNN-LSTM
          ↓
CNN-LSTM + PCA
          ↓
GNN + PCA
          ↓
Transformer
          ↓
Transformer + PCA
          ↓
Compare Results
          ↓
Analyze Differences
```

The eventual goal is to determine whether the reported results can be reproduced under the same dataset, preprocessing, architecture, and training conditions described in the paper.

---

## Research Direction

This work is part of a broader research project on **Wi-Fi CSI-based human activity and fall detection**.

The long-term objective is to investigate non-intrusive sensing systems that can recognize human activities without requiring cameras or wearable devices.

The project will eventually explore:

* CNN-LSTM architectures
* Graph Neural Networks
* Transformer-based models
* PCA-based dimensionality reduction
* Custom ESP32-S3 CSI data
* Real-time activity recognition
* Fall detection

---

## Disclaimer

This repository is a research and learning implementation.

The baseline results are specific to the experimental setup, dataset split, preprocessing pipeline, and model implementation used in this repository. Results from different datasets or implementations should not be treated as directly comparable without matching the experimental conditions.

---

## Author

**Pawan Kapakayala**

BS Computer Science
Sri Sathya Sai Institute of Higher Learning

Research interests:

* Artificial Intelligence & Machine Learning
* Wi-Fi CSI Sensing
* Human Activity Recognition
* Fall Detection
* Deep Learning
* Computer Vision & Data Science

---

## Status

🟢 **Baseline implementation completed**

🔄 **Target paper reproduction in progress**

📌 **Current focus:** Dataset acquisition and exact reproduction of the paper's CNN-LSTM + PCA methodology.
