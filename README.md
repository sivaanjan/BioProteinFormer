# 🧬 BioProteinFormer
### A Dilated Residual Transformer with ArcFace Loss for Few-Shot Protein Family Classification

BioProteinFormer is a deep learning framework for protein family classification when labeled training data is limited. It combines dilated residual convolutional networks, squeeze-and-excitation (SE) channel attention, multi-head self-attention, and ArcFace metric learning to generate discriminative protein sequence embeddings.

The project implements an end-to-end machine learning workflow covering protein sequence acquisition, feature engineering, model development, baseline benchmarking, few-shot evaluation, ablation studies, and visualization.

**Project:** BioProteinFormer  
**Domain:** Deep Learning · Bioinformatics · Computational Biology  
**Author:** Thota Siva Anjan Kumar  
**GitHub:** [BioProteinFormer Repository](https://github.com/sivaanjan/BioProteinFormer)

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Problem Statement](#-problem-statement)
- [Architecture](#-architecture)
- [Dataset](#-dataset)
- [Experimental Results](#-experimental-results)
- [Technology Stack](#-technology-stack)
- [Engineering Contributions](#-engineering-contributions)
- [Repository Structure](#-repository-structure)
- [Installation and Execution](#-installation-and-execution)
- [Generated Outputs](#-generated-outputs)
- [Limitations and Future Work](#-limitations-and-future-work)
- [Author](#-author)
- [License](#-license)

## 🎯 Overview

Protein family classification can help researchers understand the potential functions of proteins. However, some families have very few labeled examples, making accurate classification challenging.

BioProteinFormer addresses this challenge by combining three types of per-residue information:

1. **Amino-acid identity:** Character-level token embeddings.
2. **Physicochemical properties:** Seven biochemical feature scales.
3. **Bigram features:** Neighboring amino-acid co-occurrence statistics.

These features are processed by a compact neural encoder. ArcFace loss encourages angular separation between training-family embeddings, while cosine-based prototype matching supports few-shot classification using a small number of labeled examples.

The model is trained from scratch without relying on a pretrained protein language model.

## ✨ Key Features

- Protein sequence acquisition through the RCSB Protein Data Bank APIs.
- Sequence filtering, preprocessing, tokenization, and feature engineering.
- Dilated residual CNN for multi-scale sequence pattern extraction.
- Squeeze-and-excitation channel attention.
- Multi-head self-attention for contextual sequence representation.
- ArcFace angular-margin loss for discriminative embedding learning.
- Few-shot classification using cosine prototype matching.
- Comparison against ten baseline neural network architectures.
- Ablation studies to analyze individual architectural components.
- Exploratory data analysis, confusion matrices, training curves, and t-SNE visualization.
- Reproducible random-seed configuration and configurable quick/full execution modes.

## 🚩 Problem Statement

Protein classification systems often face the following challenges:

- **Limited labeled examples:** Rare families may have very few annotated sequences.
- **Variable sequence lengths:** Protein sequences can contain different numbers of amino-acid residues.
- **Multi-scale patterns:** Local motifs and longer-range sequence relationships both contribute useful information.
- **Biochemical variation:** Amino-acid identity alone does not represent all relevant physicochemical properties.
- **Generalization:** A model should learn useful representations that remain effective when only a few examples of a target class are available.

BioProteinFormer combines sequence modeling, biochemical feature engineering, and metric learning to address these challenges.

## 🏗️ Architecture

The model follows the pipeline:

**Read → Scan → Attend → Embed → Match**

```text
              Protein Sequence
                     │
                     ▼
        Three-Component Representation
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       Amino-Acid  Physicochemical  Bigram
        Tokens       Features     Features
          └──────────┼──────────┘
                     ▼
              Feature Fusion
                     │
                     ▼
           Projection Conv1D
                     │
                     ▼
          Dilated Residual CNN
                     │
                     ▼
         SE Channel Attention
                     │
                     ▼
          Multi-Head Attention
                     │
                     ▼
        Global Average + Max Pooling
                     │
                     ▼
          L2-Normalized Embedding
                     │
             ┌───────┴────────┐
             ▼                ▼
        ArcFace Loss      Prototype Matching
          Training          Inference
                                │
                                ▼
                     Predicted Protein Family
```

### Core Components

| Component | Purpose |
|---|---|
| Character-level tokenization | Encodes individual amino-acid residues. |
| Physicochemical encoding | Adds biochemical properties to each residue representation. |
| Bigram features | Captures local relationships between neighboring residues. |
| Dilated residual CNN | Extracts sequence patterns at multiple receptive-field scales. |
| SE channel attention | Learns the relative importance of feature channels. |
| Multi-head self-attention | Models contextual relationships across sequence positions. |
| Global pooling | Combines average sequence information and prominent features. |
| ArcFace loss | Encourages angular separation between training classes. |
| Prototype matching | Predicts a query's family using cosine distance to class prototypes. |

## 🧬 Dataset

**Source:** [RCSB Protein Data Bank](https://www.rcsb.org/)

The notebook uses the RCSB Search API to retrieve protein entries and the Data API to obtain canonical protein sequences. It filters sequences, removes duplicates, and prepares the data for classification.

### Target Protein Families

1. Hydrolase
2. Transferase
3. Oxidoreductase
4. Immune System
5. Lyase
6. Transcription
7. Signaling Protein
8. Transport Protein
9. Viral Protein
10. Isomerase
11. Ligase
12. Membrane Protein

### Dataset Configuration

| Property | Configuration |
|---|---|
| Data source | RCSB Protein Data Bank |
| Target families | 12 |
| Maximum sequence length | 400 residues in the notebook's full-run configuration |
| Sequence representation | Character-level tokens |
| Additional features | Seven physicochemical scales and bigram features |
| Random seed | 42 |
| Data splitting | Stratified train, validation, and test sets |

The actual number of sequences depends on successful data acquisition, filtering, class availability, and the selected run configuration. The notebook also contains a synthetic-data fallback when the RCSB API is unavailable. Results obtained from synthetic data should not be interpreted as results on real protein sequences.

## ⚙️ Experimental Configuration

The notebook supports two execution modes.

| Setting | Quick Run | Full Run |
|---|---:|---:|
| Maximum sequences per class | 300 | 1,500 |
| Target classes | 8 | 12 |
| Maximum sequence length | 200 | 400 |
| Epoch limit for baseline models | 6 | 30 |
| BioProteinFormer epoch limit | 8 | 40 |
| Batch size | 64 | 128 |
| Few-shot evaluation episodes | 50 | 200 |
| Few-shot support examples | 1, 3, 5 | 1, 2, 3, 5, 10 |

Quick Run is intended for a faster pipeline check. Full Run enables the broader experiments and evaluations configured in the notebook.

The notebook also configures a 5-way few-shot task with five query examples per class. Training and evaluation duration depend on the hardware, dataset, and runtime environment.

## 📊 Experimental Results

The following results are reported in the accompanying project showcase. They should be treated as reported experimental results, not as a guarantee that every notebook execution will reproduce the same values.

### Supervised Classification

| Metric | Reported Result |
|---|---:|
| Test accuracy | 89.40% |
| Macro F1-score | 89.38% |
| Macro precision | 89.52% |
| Macro recall | 89.40% |

### Few-Shot Classification

| Evaluation | Reported Accuracy |
|---|---:|
| 5-way 1-shot | 55.2% ± 3.2 |
| 5-way 3-shot | 68.8% ± 2.9 |
| 5-way 5-shot | 74.6% ± 2.7 |
| 5-way 10-shot | 79.9% ± 2.4 |

Few-shot results are reported as mean accuracy ± 95% confidence interval across 100 episodes in the project showcase. The notebook's current full-run configuration uses 200 few-shot evaluation episodes, so results should be regenerated and reported with the corresponding configuration when reproducing the experiments.

### Baseline Comparison

The project evaluates BioProteinFormer against ten baseline architectures:

- Simple CNN
- Deep CNN
- TextCNN
- Dilated CNN
- BiLSTM
- GRU with Attention
- CNN-BiLSTM
- ResNet-1D
- SE-CNN
- DPCNN

The project showcase reports 89.40% test accuracy for BioProteinFormer and 87.21% for ResNet-1D. The notebook includes comparative metric tables and visualizations for model evaluation.

### Ablation Analysis

The project showcase reports the following selected ablation results:

| Configuration | Test Accuracy | 5-Shot Accuracy |
|---|---:|---:|
| Full BioProteinFormer | 89.40% | 74.56% |
| Without physicochemical features | 86.12% | 70.24% |
| Without dilated tower | 84.78% | 67.83% |
| ArcFace replaced by cross-entropy | 86.91% | 68.32% |
| Without SE channel attention | 87.23% | 72.14% |
| Without multi-head self-attention | 87.84% | 72.89% |

These experiments examine how biochemical features, multi-scale convolutions, attention, and metric learning contribute to the reported performance.

## 🛠️ Technology Stack

| Category | Technologies |
|---|---|
| Programming language | Python |
| Deep learning | TensorFlow, Keras |
| Data manipulation | NumPy, Pandas |
| Machine learning utilities | scikit-learn |
| Visualization | Matplotlib, Seaborn |
| HTTP requests | Requests |
| Progress tracking | tqdm |
| Data acquisition | RCSB PDB REST APIs |
| Notebook environment | Kaggle |
| GPU acceleration | TensorFlow-compatible GPU, when available |

## 👨‍💻 Engineering Contributions

This individual project covers the design and implementation of an end-to-end deep learning workflow.

- **Data pipeline:** Implemented RCSB PDB API integration, sequence retrieval, filtering, and dataset preparation.
- **Feature engineering:** Combined amino-acid tokens, seven physicochemical properties, and bigram statistics.
- **Model architecture:** Developed the BioProteinFormer encoder using dilated convolutions, residual connections, SE attention, and self-attention.
- **Training and evaluation:** Implemented ArcFace-based training, baseline comparisons, and few-shot prototype evaluation.
- **Analysis and visualization:** Included exploratory analysis, ablation experiments, confusion matrices, classification reports, and t-SNE.
- **Reproducibility:** Added random-seed configuration, runtime options, and result-export functionality.

## 📂 Repository Structure

The main implementation is provided in the Jupyter notebook. The following is a suggested organization; adjust paths to match the files actually committed to GitHub.

```text
BioProteinFormer/
├── notebooks/
│   └── Final_code.ipynb
├── Reports/
│   └── Project documentation
├── README.md
├── Requirement.txt
├── LICENSE
└── .gitignore
```

The notebook also generates result files during execution, including CSV metric tables and PNG visualizations. These are saved to `/kaggle/working/` in the Kaggle environment.

## 🚀 Installation and Execution

### Prerequisites

- Python 3.10 or a compatible Python environment.
- pip.
- Jupyter Notebook or Kaggle Notebooks.
- TensorFlow and the other notebook dependencies.
- Internet access for live RCSB PDB retrieval.

A compatible GPU is recommended for full training runs.

### 1. Clone the Repository

```bash
git clone https://github.com/sivaanjan/BioProteinFormer.git
cd BioProteinFormer
```

### 2. Install Dependencies

If the repository contains `Requirement.txt`, run:

```bash
python -m pip install -r Requirement.txt
```

If you standardize the filename to `requirements.txt`, use that filename instead.

The notebook also checks for and installs some additional dependencies, including Requests, tqdm, and Seaborn.

### 3. Open the Notebook

Open `notebooks/Final_code.ipynb` if that is the committed path, or open `Final_code.ipynb` from its actual location in the repository.

Alternatively, upload the notebook to Kaggle and select a compatible runtime.

### 4. Select the Run Configuration

At the beginning of the notebook:

```python
QUICK_RUN = True
```

Use `True` for a faster pipeline check. Change it to `False` to use the full-run configuration.

### 5. Execute the Notebook

Run the notebook cells in order to perform:

1. Dependency checks and imports.
2. Protein data acquisition.
3. Exploratory data analysis.
4. Sequence preprocessing and feature engineering.
5. BioProteinFormer training.
6. Baseline model training and comparison.
7. Few-shot classification evaluation.
8. Embedding visualization and classification analysis.
9. Results export and final summary.

**Note:** Full execution can take substantially longer than a quick check. If live data retrieval fails, inspect the notebook's synthetic-data fallback before interpreting any results.

## 📈 Generated Outputs

Depending on successful execution and the availability of intermediate results, the notebook generates:

- Dataset and amino-acid composition analysis.
- Sequence-length and k-mer diversity visualizations.
- Comparative model performance charts.
- Model comparison and few-shot result CSV files.
- Confusion matrices and classification reports.
- Validation-accuracy training curves.
- Few-shot accuracy plots and heatmaps.
- t-SNE embedding visualizations.
- A final results dashboard.

The output inventory is displayed at the end of the notebook.

## ⚠️ Limitations and Future Work

- The evaluation covers 12 broad protein families rather than fine-grained protein subfamilies.
- The current model uses sequence-derived features without three-dimensional structural inputs.
- Large pretrained protein language models have not been benchmarked in the reported experiments.
- Episodic meta-training is a potential extension.
- Further validation on larger benchmarks such as Pfam, SCOP, and CATH would help evaluate generalization.

Future work could incorporate pretrained protein representations, structural information, prototype refinement, and more extensive cross-dataset evaluation.

## 👨‍🎓 Author

**Thota Siva Anjan Kumar**  
M.Tech in Data Science  
SRM University–Andhra Pradesh

- **GitHub Profile:** [sivaanjan](https://github.com/sivaanjan)
- **Project Repository:** [BioProteinFormer](https://github.com/sivaanjan/BioProteinFormer)

## 📄 License

This project is intended to be distributed under the MIT License. See the repository's `LICENSE` file for the applicable terms.
