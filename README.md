# 🧬 BioProteinFormer
### A Dilated Residual Transformer with ArcFace Loss for Few-Shot Protein Family Classification

## Overview

BioProteinFormer is a novel deep learning framework for protein family classification under limited labeled data conditions. The model combines dilated residual convolutional networks, squeeze-and-excitation attention, multi-head self-attention, and ArcFace metric learning to generate discriminative protein embeddings for few-shot learning.

The framework enables accurate protein classification even when only a few labeled examples are available, making it useful for computational biology, drug discovery, protein function prediction, and biomedical research.

---

## Features

- Novel BioProteinFormer architecture
- Three-channel protein sequence representation
- Dilated Residual CNN
- SE Channel Attention
- Multi-Head Self-Attention
- ArcFace Metric Learning
- Few-Shot Protein Classification
- Comprehensive benchmark comparison

---

## Project Domain

- Artificial Intelligence
- Deep Learning
- Bioinformatics
- Computational Biology
- Few-Shot Learning

---

## Applications

- Drug Discovery
- Protein Function Prediction
- Precision Medicine
- Computational Biology
- Biomedical Research
- Protein Annotation
- Genomics & Proteomics

---

## Dataset

Source:
- RCSB Protein Data Bank (PDB)

Dataset Statistics

- 6,000 Protein Sequences
- 12 Protein Families
- Sequence Length: 40–1024 Amino Acids

---

## Architecture

BioProteinFormer Pipeline

Protein Sequence
        │
        ▼
Three-Channel Input Representation
        │
        ▼
Projection Layer
        │
        ▼
Dilated Residual CNN Tower
        │
        ▼
SE Channel Attention
        │
        ▼
Multi-Head Self-Attention
        │
        ▼
Global Pooling + L2 Normalization
        │
        ▼
ArcFace Loss
        │
        ▼
Protein Embedding
        │
        ▼
Few-Shot Classification

---

## Results

| Metric | Score |
|---------|-------|
| Test Accuracy | 89.40% |
| Macro F1 | 89.38% |
| 1-Shot Accuracy | 55.2% |
| 10-Shot Accuracy | 79.9% |

---

## Repository Structure

BioProteinFormer/

├── notebooks/

├── report/

├── presentation/

├── images/

├── README.md

├── requirements.txt

├── LICENSE

└── .gitignore

---

## Installation

```bash
git clone https://github.com/yourusername/BioProteinFormer.git

cd BioProteinFormer

pip install -r requirements.txt
```

---

## Running

Open

```
Final_code.ipynb
```

Run all notebook cells.

---

## Technologies

- Python
- TensorFlow
- Keras
- NumPy
- Pandas
- Scikit-learn
- Matplotlib

---

## Author

**Thota Siva Anjan Kumar**

M.Tech Data Science

SRM University-AP

---

## License

This project is licensed under the MIT License.
