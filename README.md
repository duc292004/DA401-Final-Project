# Iowa Liquor Demand Forecasting

## Project Overview

This repository contains my DA 401 senior seminar project on weekly liquor demand forecasting in Iowa. The project examines whether macroeconomic indicators improve forecasting accuracy beyond what can already be predicted from recent historical sales patterns.

The final forecasting target is **weekly bottles sold at the store-week level**. The analysis combines the Iowa Liquor Sales dataset with selected macroeconomic indicators from the Federal Reserve Economic Data (FRED) system and compares three forecasting approaches:

- ARIMA
- Lasso Regression
- Random Forest

The results show that recent historical demand provides the strongest predictive signal, while the selected macroeconomic indicators add only limited additional short-run forecasting value.

## Why This Project Matters

Demand forecasting matters in retail because forecast errors affect inventory planning, staffing, pricing, and ordering decisions.

- Under-forecasting can lead to stockouts and lost sales.
- Over-forecasting can lead to excess inventory and inefficient use of capital.

This project asks whether adding broader macroeconomic information actually improves short-run store-week forecasting, or whether recent sales history remains the most useful source of predictive information.

## Research Question

To what extent do exogenous macroeconomic indicators and seasonal factors improve the predictive accuracy of weekly store-level liquor demand forecasting, measured as weekly bottles sold, compared with historical time-series data?

## Repository Structure

The repository is organized as follows:

- `README.md`
- `data/`
  - Iowa liquor sales data file
  - FRED macroeconomic data file
- `code/`
  - Main Quarto or R analysis file
- `paper/`
  - Final paper PDF
- `outputs/`
  - figures and tables generated from the analysis

If your actual folder names are different, update this section to match your repository.

## Data Description

This project uses two publicly available datasets:

### 1. Iowa Liquor Sales Dataset
This dataset contains daily item-level liquor transaction records in Iowa. For this project, the raw sales data are aggregated to the weekly store level so that each row in the forecasting dataset represents one **store-week**.

### 2. FRED Macroeconomic Data
This dataset contains monthly macroeconomic indicators downloaded from the Federal Reserve Economic Data system. These indicators are merged into the weekly store-level panel by matching each store-week to the corresponding month.

## Forecasting Target

The forecasting target used in all model comparisons is:

- `weekly_bottles`: total liquor bottles sold per store per week

Weekly sales dollars are also created in the dataset for descriptive purposes, but the main forecasting analysis focuses on **weekly bottles sold**.

## Main Variables

### Outcome Variable
- `weekly_bottles`

### Historical Demand Features
- `demand_lag1`
- `demand_lag2`
- `demand_lag4`
- `demand_lag12`
- `rolling_mean_4`

### Store and Seasonal Features
- `store_type`
- `month`
- `week_of_year`
- `is_holiday`

### Macroeconomic Variables
The merged FRED data include indicators related to:
- inflation
- employment
- unemployment
- consumer sentiment
- savings
- income
- broader retail conditions

## Data Preparation

The workflow used in this project includes the following steps:

1. Load the raw Iowa Liquor Sales and FRED datasets.
2. Clean date fields and standardize column names.
3. Remove problematic or invalid sales records.
4. Aggregate liquor sales to the weekly store level.
5. Merge monthly FRED indicators into the weekly panel.
6. Create lagged demand and rolling mean features.
7. Split the data chronologically into:
   - training set: 2021–2024
   - test set: 2025

The year 2020 is excluded because the COVID-19 period introduced unusual purchasing behavior that would distort the forecasting comparison.

Because the lagged demand and rolling mean variables require prior history, the first observed weeks for each store do not have valid lag values. Those rows are excluded from the lag-based modeling sample so that all predictors are defined using only past information.

## Methods

This project compares three forecasting approaches:

### 1. ARIMA
ARIMA serves as the historical baseline model. It is implemented **separately for each store** using pre-2025 weekly demand history, and the resulting forecasts are evaluated on the same 2025 store-week test sample used for the other models.

### 2. Lasso Regression
Lasso includes lagged demand, store structure, seasonality, and macroeconomic indicators in a regularized linear framework. Its penalty parameter is selected using cross-validation within the training set only.

### 3. Random Forest
Random Forest provides a nonlinear benchmark and helps identify the relative importance of predictors. Its `mtry` parameter is selected using out-of-bag performance on a training subset only.

## Model Evaluation

All three models are evaluated on the **same 2025 store-week test sample**.

The main evaluation metrics are:

- **RMSE (Root Mean Square Error)**
- **MAE (Mean Absolute Error)**

## Final Model Comparison

| Model | RMSE | MAE |
|---|---:|---:|
| ARIMA | 322.58 | 143.90 |
| Random Forest | 333.76 | 143.61 |
| Lasso | 333.93 | 137.35 |

These results suggest that historical demand remains very difficult to outperform. ARIMA achieved the best RMSE, while Lasso achieved the best MAE.

## Robustness Check

As a short robustness check, I re-estimated the Lasso model using a reduced macroeconomic specification.

| Model | RMSE | MAE |
|---|---:|---:|
| Main Lasso | 333.9252 | 137.3457 |
| Reduced-macro Lasso | 333.9175 | 137.6419 |

The reduced-macro model performed almost identically to the main Lasso model, which suggests that the overall conclusion does not depend heavily on one particular macroeconomic variable set.

## Main Findings

The main findings of the project are:

- Weekly liquor demand shows strong short-run persistence.
- Urban stores sell more bottles than rural stores, but both follow similar time patterns.
- Lagged demand variables and rolling demand measures are the strongest predictors.
- The selected macroeconomic variables provide some additional information, but their contribution is weaker than recent demand history.
- More complex models did not clearly outperform a strong historical-demand baseline.
- The main conclusion is robust to a reduced macroeconomic specification.

## Code Description

The main analysis file contains the full workflow for:

- data cleaning
- weekly aggregation
- feature engineering
- exploratory data analysis
- ARIMA modeling
- Lasso modeling
- Random Forest modeling
- robustness checking
- model comparison
- figure and table generation

If your main file has a different name, update this section to match the exact filename in your repository.

## How to Run the Code

To reproduce the analysis:

1. Open RStudio.
2. Make sure the required packages are installed.
3. Place the raw data files in the correct folder paths used in the code.
4. Open the main `.qmd` or `.R` file.
5. Run the file from start to finish.

## Required R Packages

The project uses R and mainly relies on:

- `tidyverse`
- `lubridate`
- `zoo`
- `forecast`
- `glmnet`
- `randomForest`
- `ggplot2`
- `knitr`
- `caret`

Install any missing packages before running the code.

## Paper

The written report explains the research question, literature, data, methods, results, discussion, limitations, and conclusion in full detail.

## Additional Notes

- This project is **predictive rather than causal**.
- The analysis focuses on short-run forecasting at the **store-week** level.
- Lagged demand and rolling mean features are constructed **within store using only past weeks**.
- The Random Forest model is estimated on a reduced training sample for computational efficiency.
- The macroeconomic indicators used here are monthly and aggregated, which may limit their usefulness for weekly store-level forecasting.
- The results are best interpreted as applying most directly to **Iowa store-level liquor demand in the post-2020 period**, under the sample restrictions used in this study.
- All data used in this project are publicly available.

## Author

Duc Ngo  
DA 401 – Spring 2026  
Denison University
