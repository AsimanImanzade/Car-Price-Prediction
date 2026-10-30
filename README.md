# Car Price Prediction

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white) ![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white) ![XGBoost](https://img.shields.io/badge/XGBoost-2C728E?style=flat-square) ![LightGBM](https://img.shields.io/badge/LightGBM-21918C?style=flat-square)

End-to-end regression project that estimates used-car prices in Azerbaijan from turbo.az listings.

## Results

Compared on an 80/20 split with 44,752 test cars. The median price is 20,800 AZN.

| Model | R² | MAE (AZN) |
|---|---|---|
| Random Forest (best) | 0.955 | 2,368 |
| Linear baseline | 0.740 | 5,929 |

- Random Forest has 60% lower error than the linear baseline.
- LightGBM reaches R² 0.945, trains 6x faster (3.3 s vs 20.6 s) and its model is about 200x smaller (4.3 MB vs 890 MB).
- Boosting models keep the train/test R² gap below 0.01.
- Voting and Stacking ensembles did not beat a well-tuned Random Forest.

## Data

The raw dataset has 653,721 listings across 56 columns. After cleaning, 223,758 listings remain, covering 205 brands and 2,529 make-model combinations.

## Method

1. **Cleaning:** removed duplicates, irrelevant fields (URLs, phone numbers, images, VINs) and listings whose title did not match the brand and model. Converted prices to AZN (USD x 1.70, EUR x 1.98), parsed engine strings into horsepower and fuel type, and converted production year to car age.
2. **Feature engineering:** added 6 features to the 16 originals: log mileage, km per year, horsepower per liter, age x horsepower, a zero-mileage flag and brand.
3. **Preprocessing:** scikit-learn pipelines prevent leakage, with median imputation and RobustScaler for numeric columns, one-hot encoding for categorical columns, and cross-fitted target encoding for make-models. Price is modeled on a log scale.
4. **Modeling:** compared Linear, Ridge, Elastic Net, Random Forest, Extra Trees, Gradient Boosting, HistGradientBoosting, XGBoost, LightGBM, Voting and Stacking.

## Files

| File | Purpose |
|---|---|
| `data_cleaning_cars.ipynb` | Data cleaning |
| `workspace_cars.ipynb` | Features, modeling and evaluation |
| `cleaned_data.csv.zip` | Cleaned dataset |

---

By [Asiman Imanzade](https://asiman-imanzade.pages.dev). More projects on [my portfolio](https://asiman-imanzade.pages.dev).
