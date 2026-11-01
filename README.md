# XAI-Epileptic-Seizure-Detection from EEG Signals


**Detects epileptic seizures from brain waves with a 0.9985 AUC score (1.0 = perfect)** — and shows *why*.

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
[![Colab-ready](https://img.shields.io/badge/Colab-ready-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com/github/<USERNAME>/<REPO>/blob/main/seizure_detection.ipynb)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikitlearn&logoColor=white)
![imbalanced-learn](https://img.shields.io/badge/imbalanced--learn-SMOTETomek-informational)
![SHAP](https://img.shields.io/badge/SHAP-explainable-blueviolet)

## 📊 Results at a Glance
Test set: 2,300 snippets (460 seizures). Ranked by F1.

| Model | Sensitivity | Specificity | F1 | AUC-ROC |
|---|---|---|---|---|
| **Voting Ensemble** | 0.9826 | 0.9870 | 0.9658 | 0.9985 |
| HistGradientBoosting | 0.9804 | 0.9870 | 0.9647 | 0.9985 |
| Extra Trees | 0.9696 | 0.9897 | 0.9643 | 0.9986 |

[SCREENSHOT: confusion_matrix_voting.png]

[SCREENSHOT: roc_curves.png]

## 📖 Project Overview
Epilepsy affects millions of people, and seizures strike without warning.
Specialists often scan long recordings by eye, which is slow and tiring.
This project teaches a computer to spot seizure patterns in one-second slices of brain activity, and explains its reasoning.
*Research prototype — not a medical device.*

## 🔄 End-to-End Pipeline
![End-to-end pipeline](images/pipeline_flowchart.png)

## ✨ Key Features
- [x] Turns raw brain waves into 37 readable measurements
- [x] Picks the 20 most useful using four independent methods
- [x] Corrects for seizures being rare
- [x] Tests 11 models side by side
- [x] Explains decisions with visual charts

## 🛠 Tech Stack
Python · pandas · NumPy · SciPy · scikit-learn · imbalanced-learn · SHAP · LIME · Matplotlib · Seaborn · Jupyter

## 🗂 Dataset
[UCI Epileptic Seizure Recognition](https://archive.ics.uci.edu/ml/datasets/Epileptic+Seizure+Recognition): 11,500 one-second EEG snippets (178 readings each, 173.6 Hz); one in five is a seizure. A small demo subset sits in `/data/sample/`; put the full CSV in `/data/` to reproduce results.

## 🚀 How to Run
```bash
git clone https://github.com/<USERNAME>/<REPO>.git && cd <REPO>
pip install -r requirements.txt
jupyter notebook seizure_detection.ipynb
```
> **Open in Colab:** [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/<USERNAME>/<REPO>/blob/main/seizure_detection.ipynb) — upload the CSV and set `DATA_PATH`.

## 📓 Notebook Structure
| # | Step | What happens |
|---|---|---|
| 1 | Data loading & cleaning | Load CSV, drop ID column, build seizure vs non-seizure label |
| 2 | Feature engineering | 37 time, Hjorth and frequency features (215 combined) |
| 3 | Train/val/test split | 70/10/20, stratified, scaler fit on train only |
| 4 | Feature selection | Chi², MI, RFECV, SFS consensus → top 20 |
| 5 | Balancing | SMOTETomek, 4:1 → 1:1 (train only) |
| 6 | Model training | 11 models incl. Voting and Stacking ensembles |
| 7 | Hyperparameter tuning | RandomizedSearchCV |
| 8 | Evaluation | ROC, PR curves, confusion matrices, radar chart |
| 9 | Explainability | SHAP (plus LIME for single predictions) |
| 10 | Summary leaderboard | All models ranked on one heatmap |

## 📈 Results & Visualizations
![Leaderboard heatmap](images/leaderboard_heatmap.png)
![ROC curves](images/roc_curves.png)
![Confusion matrices](images/confusion_matrices.png)
![Feature distributions](images/feature_distributions.png)

## 🧩 Challenges Solved
- **Class imbalance:** Rebalanced *training* data only (4:1 → 1:1), keeping the test set honest.
- **Patient-level split caveat:** Split by snippet, not patient or recording, so scores may be optimistic.
- **Clean inputs, no leakage:** Dropped the ID column, confirmed no missing values, fitted scaling and selection on training data only.
- **Interpretability:** SHAP charts show which traits (mainly overall amplitude) push toward "seizure".

## ⚠️ Limitations & Future Work
- One public dataset; no external validation (next: CHB-MIT).
- Snippet-level split may inflate scores; patient-wise cross-validation planned.
- No real-time or deep-learning (EEGNet, 1D-CNN-LSTM) baselines yet.

## 👤 Author
**Name:** <Your Name> · **LinkedIn:** <linkedin.com/in/your-handle> · **Email:** <you@example.com>

## 📜 License & Acknowledgements
MIT License. Data: Andrzejak et al. (2001), *Phys. Rev. E* 64, 061907, via the UCI ML Repository.
