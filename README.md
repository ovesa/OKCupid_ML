# OKCupid: Date-A-Scientist
### Can lifestyle features predict your zodiac sign?

---

## Overview

This project explores whether machine learning can predict a user's zodiac sign from their lifestyle and demographic information on OKCupid. About 18.4% of profiles are missing a zodiac sign, which matters because zodiac compatibility is something a lot of users actually care about when choosing matches. The idea was that if I could predict it reasonably well, those missing signs could be filled in automatically to improve match quality.

The short answer is: not really. But the process of figuring that out taught me a lot.

---

## Dataset

- **59,946** OKCupid profiles
- **31 columns** including demographics, lifestyle choices, and 10 open ended essay responses
- Target variable: `sign` (missing for 18.4% of users)

The dataset is not included in this repo. I obtained it from codeacademy.

---

## Project Structure

```
project/
├── date-a-scientist.ipynb    # exploratory data analysis
├── main_project.ipynb        # preprocessing and modeling
├── slidedeck.qmd             # quarto presentation source
├── slidedeck.html            # rendered presentation
├── plot_style.py             # centralized matplotlib styling
├── figures/                  # saved chart images
└── README.md
```

---

## Approach

### Preprocessing
- Cleaned the `sign` column by extracting just the zodiac name from messy strings
- Used `OrdinalEncoder` for ordered categorical features like drinking and smoking habits
- Wrote a custom function to convert 33 messy education strings into a 6 level numeric scale, 
  accounting for dropouts by returning the level below what they dropped out of
- Dropped diet as a feature due to 38% missing values
- Imputed remaining missing values with the column median
- Standardized all features to mean=0 and std=1
- Split into 80% training and 20% test sets

### Models
I tried three classification models and evaluated each using accuracy, F1 score, confusion matrices and classification reports:

| Model | Accuracy | Macro F1 |
|---|---|---|
| Baseline (random guessing) | 8.3% | - |
| KNN (k=20) | 8.6% | 0.08 |
| GaussianNB | 8.8% | - |
| SVM (C=0.1, Gamma=10) | 9.3% | 0.07 |
| CategoricalNB (Alpha=2.0) | 9.5% | 0.07 |

---

## Key Findings

All models performed right around the random guessing baseline of 8.3%, which confirms that lifestyle features have essentially no relationship with zodiac signs. This makes sense since zodiac signs are determined by birthday, not by how often someone drinks  or what their education level is.

The most interesting finding was that accuracy alone is misleading. CategoricalNB had the highest accuracy at 9.5% but achieved it by just predicting leo for almost everyone. KNN had lower accuracy but was more honest in spreading predictions across all 12 signs. Looking at confusion matrices and classification reports together told a much more complete story than accuracy alone.

---

## What I Would Do Differently

- **Different target variable** - predicting drinking habits or smoking status from other features would likely yield much stronger results since those have real relationships with lifestyle factors
- **Natural language processing** - the 10 essay columns contain a huge amount of untapped information that structured features cannot capture

---

## Tools and Libraries

- Python, Pandas, NumPy
- Scikit-learn (OrdinalEncoder, KNeighborsClassifier, SVC, GaussianNB, CategoricalNB, GridSearchCV)
- Matplotlib
- Quarto (presentation)
