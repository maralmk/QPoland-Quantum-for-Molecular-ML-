# AQW-RE: Adaptive Quantum Walk–Resistance Embedding  
### QPoland Global Quantum Hackathon 2025 — QunaSys Track  

This repository contains the full implementation and notebook for **AQW-RE (Adaptive Quantum Walk–Resistance Embedding)** — a hybrid *classical + quantum-inspired* feature map for molecular graph classification.

---

## 🌟 Overview

**AQW-RE** combines interpretable classical graph descriptors with a small quantum-inspired circuit embedding implemented via **QURI Parts** (with optional Qulacs backend).  
It encodes both local and global graph properties into a fixed-length vector suitable for Support Vector Machine (SVM) classification.

The system has been evaluated on standard molecular graph benchmarks — **NCI1**, **AIDS**, **MUTAG**, **PROTEINS**, and **PTC_MR** — showing competitive accuracy and interpretability.

---

## 🧩 Key Components

| Module | Description |
|--------|--------------|
| **`aqw_re_feature()`** | Generates the full AQW-RE feature vector combining structural, spectral, and resistance descriptors. |
| **`quantum_embedding_features_quri()`** | Builds and simulates a small quantum circuit (RY + CNOT ladder) using QURI Parts. |
| **`evaluate_and_report_classical()`** | Performs classical 10-fold Stratified CV with SVM kernels (linear, RBF). |
| **`evaluate_and_report_quantum()`** | Extends evaluation by adding quantum embeddings per CV fold. |

---

## ⚙️ Installation

You can run this code directly in **Google Colab** (recommended).

### Step 1: Install dependencies
```bash
!pip install --quiet networkx pandas scikit-learn tqdm
!pip install --quiet "quri-parts[qulacs]"
# Optional (for circuit visualization)
!pip install --quiet qiskit pylatexenc matplotlib
```

### Step 2: Clone this repository
```bash
!git clone https://github.com/maralmk/QPoland-Quantum-for-Molecular-ML-.git
cd QPoland-Quantum-for-Molecular-ML-
```
### 🚀 How to Run

Open the main notebook:
Hack0_2.ipynb

Set dataset file names for the dataset you want to test:
```bash
A_FILE  = "NCI1_A.txt"
GI_FILE = "NCI1_graph_indicator.txt"
GL_FILE = "NCI1_graph_labels.txt"
NL_FILE = "NCI1_node_labels.txt"
```
### Run all cells.
The script will:

Load molecular graphs

Build the AQW-RE feature embeddings

Train SVM classifiers (linear and RBF)

Optionally include quantum-inspired embeddings via QURI Parts

Print accuracy and F1-macro results with 10-fold CV

### Datasets
Datasets used are from the TU Dortmund Graph Classification Repository
:

NCI1 — Chemical compounds for anti-cancer screening

AIDS — Molecular activity prediction

MUTAG — Mutagenic compounds

PROTEINS — Protein classification

PTC_MR — Carcinogenicity dataset

---

## 📊 Results Summary

| **Dataset** | **Classical (Linear)** | **Classical (RBF)** | **Quantum (Linear)** | **Quantum (RBF)** |
|--------------|------------------------|----------------------|----------------------|-------------------|
| **NCI1**     | 73.02 ± 2.42 % | **82.00 ± 1.41 %** | 72.41 ± 2.27 % | 72.77 ± 2.28 % |
| **AIDS**     | 99.35 ± 0.59 % | **99.55 ± 0.65 %** | 99.35 ± 0.59 % | 99.25 ± 0.68 % |
| **MUTAG**    | 79.82 ± 6.03 % | **86.75 ± 5.32 %** | 79.33 ± 6.27 % | 80.38 ± 5.65 % |
| **PROTEINS** | 74.49 ± 3.27 % | **75.38 ± 2.30 %** | 74.76 ± 3.85 % | 74.04 ± 4.09 % |
| **PTC_MR**   | 57.00 ± 6.52 % | **63.42 ± 6.17 %** | 59.29 ± 6.72 % | 58.74 ± 7.44 % |

### Interpretation:
AIDS and MUTAG datasets show near-perfect separation due to strong WL-like local features.
Quantum-inspired augmentation performed comparably to classical, demonstrating that even shallow circuits (6–8 qubits) can approximate structured graph embeddings effectively.

---

## 🧮 Quantum Circuit (QURI Parts)

The QURI-based encoder maps PCA-reduced features into qubit rotations and entanglement operations:
```bash
for each feature_i → RY(theta_i)
entangle using ladder CNOT(q, q+1)
measure expectation-like probabilities
```
A similar circuit can be visualized with Qiskit:
```bash
from qiskit import QuantumCircuit
qc = QuantumCircuit(6)
for q in range(6):
    qc.ry(0.5*q, q)
    if q < 5:
        qc.cx(q, q+1)
qc.draw('mpl')
```

---

## 🎥 Project Presentation

(Drive video link)

### The video explains:

Problem motivation

AQW-RE feature map design

Quantum embedding pipeline (QURI Parts)

Results comparison

Future improvements

---

## 👥 Authors
**Maral Mahmoudi Kamelabad** and **Abdul Hannan**
