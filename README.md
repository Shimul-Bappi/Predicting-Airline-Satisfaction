# ✈️ Predicting Airline Satisfaction

Solution for the Kaggle **[Playground Series – Season 6, Episode 10: Predicting Airline Satisfaction](https://www.kaggle.com/competitions/playground-series-s6e10)**.
The task is to predict whether an airline passenger was **satisfied** from survey ratings, flight details and passenger information (binary classification, probability output).

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Kaggle](https://img.shields.io/badge/Kaggle-Playground%20S6E10-20BEFF)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📊 Results (5-fold stratified CV, out-of-fold)

| Model | AUC | Accuracy | Log-loss | F1 |
|---|---|---|---|---|
| LightGBM | 0.95890 | 0.92647 | 0.22662 | 0.91612 |
| XGBoost | 0.95874 | 0.92630 | 0.22720 | 0.91593 |
| CatBoost | 0.95863 | 0.92642 | 0.22736 | 0.91608 |
| **Blend (equal weights)** | **0.95900** | **0.92651** | **0.22651** | **0.91618** |

Single-fold AUCs stay between 0.9576 and 0.9595, so the CV estimate is stable. The three libraries make almost identical predictions, which is why blending adds only ≈ +0.0001 AUC.

## 🧠 Approach

1. **EDA** – class balance (55.6 % / 44.4 %), missing values (only 292 in `Arrival Delay in Minutes`), satisfaction rate per category and per rating value, correlations, and a train-vs-test shift check.
2. **Feature engineering**
   - Aggregates over the 13 service ratings (mean, sum, std, min, max, zeros, low ≤ 2, high ≥ 4)
   - Delay features (total, max, difference, `log1p`)
   - Frequency encoding and pairwise combinations of the categorical columns
3. **Models** – LightGBM, XGBoost and CatBoost trained on the same 5 stratified folds with early stopping (a scikit-learn `HistGradientBoosting` fallback runs if none of them is installed).
4. **Blend** – equal mean vs. OOF-optimised weights; the better one is used.
5. **Diagnostics** – ROC, confusion matrix, calibration, model agreement, feature importance and per-segment AUC, each followed by a written interpretation.
6. **Safety checks** – assertions for feature alignment, no id/target leakage, probability sanity and an exact match with `sample_submission.csv`.

The notebook auto-detects the id column, the target and the submission format (probabilities vs. labels) from `sample_submission.csv`.

## 📁 Repository structure

```
Predicting-Airline-Satisfaction/
├── notebooks/
│   └── predicting-airline-satisfaction.ipynb   # full pipeline: EDA → features → CV → blend → submission
├── data/
│   └── README.md                               # how to get the competition data (not committed)
├── requirements.txt
├── LICENSE
├── .gitignore
└── README.md
```

## 🚀 How to run

### On Kaggle (easiest)
1. Open the competition page and click **Code → New Notebook**, or upload `notebooks/predicting-airline-satisfaction.ipynb`.
2. Make sure the competition data is attached, then **Run All**. `submission.csv` is written to `/kaggle/working`.

### Locally
```bash
git clone https://github.com/Shimul-Bappi/Predicting-Airline-Satisfaction.git
cd Predicting-Airline-Satisfaction
python -m venv .venv && source .venv/bin/activate      # Windows Git Bash: source .venv/Scripts/activate
pip install -r requirements.txt
# download the data into ./data (see data/README.md), then change DATA_DIR in the config cell to './data'
jupyter lab notebooks/predicting-airline-satisfaction.ipynb
```

## 🔭 Ideas to improve the score
- Add the original public *Airline Passenger Satisfaction* dataset as extra training rows (with an `is_original` flag).
- Add a different model family (neural network, logistic regression on rating aggregates) for blend diversity.
- Repeat CV with several seeds of the best model.

## 👤 Author — Shimul Bappi

[![GitHub](https://img.shields.io/badge/GitHub-Shimul--Bappi-181717?logo=github)](https://github.com/Shimul-Bappi)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-shimul--bappi-0A66C2?logo=linkedin)](https://www.linkedin.com/in/shimul-bappi/)
[![X](https://img.shields.io/badge/X-@Bappi__Shimul-000000?logo=x)](https://x.com/Bappi_Shimul)
[![Portfolio](https://img.shields.io/badge/Portfolio-shimul--bappi.github.io-FF5722)](https://shimul-bappi.github.io/Shimul_Bappi-portfolio/)

- GitHub: <https://github.com/Shimul-Bappi>
- LinkedIn: <https://www.linkedin.com/in/shimul-bappi/>
- X: <https://x.com/Bappi_Shimul>
- Portfolio: <https://shimul-bappi.github.io/Shimul_Bappi-portfolio/>

If this helped you, a ⭐ on the repo is appreciated!

## 📄 License
Released under the [MIT License](LICENSE).
