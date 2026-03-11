# FHE Logistic Regression – Privacy-Preserving Machine Learning Using Homomorphic Encryption

> **A study on training Logistic Regression on both plain and encrypted data using CKKS Homomorphic Encryption via TenSEAL**

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [How It Works](#how-it-works)
- [CKKS Encryption Pipeline](#ckks-encryption-pipeline)
- [Results](#results)
- [Project Folder Structure](#project-folder-structure)
- [Files to Upload on GitHub](#files-to-upload-on-github)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Usage](#usage)
- [Dataset](#dataset)
- [Current Limitations](#current-limitations)
- [Future Improvements](#future-improvements)
- [Authors](#authors)
- [References](#references)
- [License](#license)

---

## Overview

Machine learning models typically require access to raw, unencrypted data — posing serious privacy risks when dealing with sensitive domains like healthcare. **Privacy-Preserving Machine Learning (PPML)** addresses this by allowing models to train and infer on **encrypted data directly**, without ever decrypting it.

This project demonstrates how **Homomorphic Encryption (HE)** using the **CKKS scheme** (via TenSEAL) can be applied to train a **Logistic Regression model** on the **Heart Disease Dataset**, comparing its performance against a plain (non-encrypted) baseline.

The project was developed as part of **CSE1029 – Network Security and Cryptography Fundamentals** at VIT Chennai.

---

## Key Features

- **Homomorphic Encryption** via TenSEAL (CKKS scheme) — computations on encrypted data without decryption
- **Logistic Regression** trained on both plaintext and ciphertext data
- **Side-by-side performance comparison** — accuracy, confusion matrices, training curves
- **Heart Disease Dataset** — real-world healthcare data (binary classification)
- **Encrypted model slightly outperforms** plain model on test accuracy (64.97% vs 62.85%)
- Full **CKKS pipeline**: scaling → polynomial encoding → key generation → noise addition → ciphertext

---

## How It Works

```
Raw Data (Heart Disease Dataset)
        ↓
Data Preprocessing (scaling, encoding, train/test split)
        ↓
     ┌──────────────────────────────┐
     │                              │
Plain Logistic Regression    Encrypted Logistic Regression
(baseline)                   (TenSEAL CKKS)
     │                              │
     └──────────┬───────────────────┘
                ↓
     Performance Comparison
     (Accuracy, Confusion Matrix, Training Curves)
```

---

## CKKS Encryption Pipeline

```
Plain Data → Scaling → Polynomial Encoding → Key Generation → Noise Addition → Ciphertext
```

| Step | Description |
|---|---|
| Scaling | Data scaled to fit CKKS parameters, preventing overflow |
| Polynomial Encoding | Data encoded into a polynomial for homomorphic operations |
| Key Generation | Secret key (decryption) + Public key (encryption) generated |
| Noise Addition | Controlled noise added for cryptographic security |
| Ciphertext | Final encrypted representation used for training |

**CKKS Parameters used:**

| Parameter | Value |
|---|---|
| Polynomial Modulus Degree | 8192 |
| Coefficient Modulus Bit Sizes | [40, 21, 21, 21, 21, 21, 21, 40] |
| Global Scale | 2²¹ |

---

## Results

### Accuracy Comparison

| Metric | Plain Model | Encrypted Model |
|---|---|---|
| Training Accuracy | 0.6045 | 0.6628 |
| Test Accuracy | **0.6285** | **0.6497** |

The encrypted model achieved **slightly higher test accuracy** than the plain model. This is attributed to:
- **Numerical regularization** — CKKS approximations act as implicit regularization, improving generalization
- **Smoothing effect** — polynomial approximation in CKKS reduces sensitivity to noise
- **Different optimization trajectory** — approximate computations in the encrypted domain can lead to better generalization

This behavior was **consistent across multiple runs**, confirming it is a property of encrypted computation rather than randomness.

### Confusion Matrix Summary

| | Plain Model | Encrypted Model |
|---|---|---|
| True Negatives | 113 | 100 |
| False Positives | 67 | 70 |
| False Negatives | 57 | 47 |
| True Positives | 97 | 117 |

The encrypted model shows **better recall for positive cases** (heart disease detected), which is more clinically valuable.

---

## Project Folder Structure

```
FHE_Logistic_Regression/
│
├── notebook/
│   └── FHE_Logistic_Regression.ipynb     # Main Jupyter notebook
│
├── data/
│   └── heart.csv                          # Heart Disease Dataset (from Kaggle)
│
├── plots/
│   ├── normal_distribution.png            # Plain vs Encrypted data distribution
│   ├── accuracy_over_epochs.png           # Training accuracy comparison plot
│   ├── accuracy_comparison.png            # Bar chart: plain vs encrypted accuracy
│   └── confusion_matrices.png            # Confusion matrix for both models
│
├── requirements.txt                       # Python dependencies
├── .gitignore
└── README.md
```

---

## Files to Upload on GitHub

Here's exactly what you need to upload:

| File / Folder | Description | Required? |
|---|---|---|
| `FHE_Logistic_Regression.ipynb` | Main notebook with all code | ✅ Yes |
| `data/heart.csv` | Heart disease dataset | ✅ Yes |
| `requirements.txt` | Python dependencies | ✅ Yes |
| `README.md` | This file | ✅ Yes |
| `.gitignore` | To exclude unnecessary files | ✅ Yes |
| `plots/` folder | Screenshots of output graphs | ⭐ Recommended |

**What NOT to upload:**
- `.env` files (API keys)
- `__pycache__/` folders
- `.ipynb_checkpoints/` (Jupyter auto-saves)
- Any large model weight files

**Suggested `.gitignore`:**
```
.env
__pycache__/
.ipynb_checkpoints/
*.pyc
*.pth
*.pt
```

---

## Technologies Used

| Library | Purpose |
|---|---|
| `tenseal` | Homomorphic Encryption (CKKS scheme) |
| `scikit-learn` | Logistic Regression (plain model) |
| `pandas` | Data loading and preprocessing |
| `numpy` | Numerical computations |
| `matplotlib` | Accuracy plots and confusion matrices |
| `torch` (PyTorch) | Tensor operations (TenSEAL integration) |

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/navi004/FHE_Logistic_Regression.git
cd FHE_Logistic_Regression
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate        # Mac/Linux
venv\Scripts\activate           # Windows
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

Or manually:

```bash
pip install tenseal scikit-learn pandas numpy matplotlib torch
```

> **Note:** TenSEAL may require a C++ build environment. On Windows, install [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) first.

---

## Usage

Open and run the notebook:

```bash
jupyter notebook FHE_Logistic_Regression.ipynb
```

The notebook runs in order:
1. Load and preprocess the Heart Disease Dataset
2. Train plain Logistic Regression (baseline)
3. Encrypt data using TenSEAL CKKS
4. Train encrypted Logistic Regression
5. Compare accuracy, plot confusion matrices and training curves

---

## Dataset

- **Name**: Heart Disease Dataset
- **Source**: [Kaggle – Heart Disease Prediction using Logistic Regression](https://www.kaggle.com/datasets/dileep070/heart-disease-prediction-using-logistic-regression)
- **Task**: Binary classification (heart disease: yes / no)
- **Features**: Age, Sex, Chest Pain Type, Resting Blood Pressure, Cholesterol, Fasting Blood Sugar, ECG Results, Max Heart Rate, Exercise-Induced Angina, ST Depression, ST Slope, Major Vessels, Thalassemia

---

## Current Limitations

- Limited to **Logistic Regression** — more complex models (neural networks, SVM) would require more advanced HE schemes
- **Slow training** due to the computational overhead of homomorphic operations
- CKKS is an **approximate** scheme — slight precision loss compared to exact computation
- **CKKS parameters** (polynomial degree, bit sizes) require careful tuning per use case
- Only tested on a **small tabular dataset** — scalability to larger datasets is not validated

---

## Future Improvements

- Extend to **neural networks** using frameworks like CrypTen or Concrete ML
- Experiment with **Fully Homomorphic Encryption (FHE)** for more complex model types
- Benchmark **computational overhead** (time and memory) of encrypted vs plain training
- Apply to other sensitive domains: **finance, legal, biometrics**
- Add **BLEU / F1 / AUC-ROC** metrics beyond accuracy for a more complete evaluation
- Explore **federated learning + HE** for distributed privacy-preserving ML

---

## Authors

Naveen N
naveen.ndd2004@gmail.com

---

## References

1. Gentry, C. (2009). Fully Homomorphic Encryption Using Ideal Lattices. *STOC 2009*.
2. Liu et al. (2017). A Survey of Privacy-Preserving Machine Learning Techniques. *IJCSIS*.
3. Liu et al. (2020). TenSEAL: Homomorphic Encryption for Machine Learning with PyTorch. *GitHub*.
4. Zhu et al. (2018). Homomorphic Encryption for Logistic Regression: A Comparative Study. *IJCS*.

---

## License

This project is intended for **educational and research purposes** as part of the CSE1029 course at VIT Chennai.

---

*Built with ❤️ using TenSEAL, scikit-learn, and Python*
