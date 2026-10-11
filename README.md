# Comparative Study: CNN vs. DNN Architectures & Transfer Learning in Computer Vision

An empirical deep learning project benchmarking **Convolutional Neural Networks (CNN)** against **Deep Fully Connected Neural Networks (DNN)** and evaluating **Transfer Learning** usability across distinct visual domains using **TensorFlow / Keras**.

---

## Project Overview

This repository contains the implementation, evaluation protocol, and experimental results for:
1. **Architectural Comparison (MNIST):** Benchmarking CNN vs. DNN on handwritten digits with a focus on parameter efficiency, spatial feature retention, and overfitting.
2. **Transfer Learning & Domain Shift (Fashion-MNIST):** Evaluating knowledge transfer from simple geometric structures (digits) to complex apparel items using a two-phase transfer methodology (Layer Freezing + Fine-Tuning).
3. **Rigorous Statistical Validation:** Applying **6-Fold Stratified Cross-Validation** to eliminate evaluation bias.
4. **In-Depth Error Analysis:** Analyzing confusion matrices to isolate class-level classification challenges and morphological similarities.

---

## Key Experimental Results

### 1. Architectural Benchmark (MNIST Test Set)
*Evaluated across 6 folds:*

| Architecture | Trainable Params | Test Accuracy | Precision (macro) | Recall (macro) | F1-Score (macro) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **CNN** | **~40.5K** | **98.72%** | **98.72%** | **98.71%** | **98.71%** |
| **DNN** | ~109.4K | 97.63% | 97.63% | 97.61% | 97.61% |

> **Key Finding:** CNN achieved superior generalization with **~60% fewer parameters**, demonstrating the critical role of convolutional filters and spatial invariance over raw fully connected capacity.

---

### 2. Scratch vs. Transfer Learning (Fashion-MNIST Test Set)

| Method | Test Accuracy | Generalization / Overfitting Gap |
| :--- | :---: | :--- |
| **CNN (From Scratch)** | **89.76%** | High capacity; susceptible to overfitting on complex textures |
| **DNN (From Scratch)** | 88.57% | Moderate generalization drop |
| **DNN (Transfer Learning)** | 72.11% | **High structural stability** (< 1% train/test divergence) |
| **CNN (Transfer Learning)** | 71.60% | **High structural stability** (< 1% train/test divergence) |

> **Key Finding (Domain Shift):** Due to the semantic gap between binary digit contours and textured apparel, models trained from scratch yielded higher top accuracy on large datasets. However, transfer learning acted as a robust **regularizer**, virtually eliminating overfitting gap.

---

## Methodology & Transfer Learning Strategy

The transfer learning workflow was implemented in two distinct phases to prevent **Catastrophic Forgetting**:

1. **Phase A — Layer Freezing:** Base feature extraction layers were frozen (`trainable = False`), and only the newly initialized 10-class Dense classification head was trained (40 epochs).
2. **Phase B — Fine-Tuning:** Feature extraction layers were unfrozen (`trainable = True`) and fine-tuned alongside the classification head using the **Adam optimizer** with adaptive learning rate dynamics (20 epochs).

---

## Repository Structure

```text
├── comparingbasicnnarchitectures.py      # Part 1: MNIST CNN vs DNN training & 6-Fold CV
├── evaluatingtransferlearningusability.py # Part 2: Fashion-MNIST Scratch vs Transfer Learning
├── erotima1.csv                          # Experimental metrics per fold (Question 1)
├── erotima2.csv                          # Experimental metrics per fold (Question 2)
├── cnn_model_final.h5                    # Saved pre-trained CNN model weights
├── dnn_model_final.h5                    # Saved pre-trained DNN model weights
├── Anafora_Ergasia1.pdf                  # Comprehensive technical report (Greek)
└── README.md                             # Project documentation
