# Iowa Liquor Demand Forecasting

## Project Overview

This repository contains my DA 401 senior seminar project on weekly liquor demand forecasting in Iowa. The project examines whether store-level liquor demand is predicted more effectively by recent historical sales patterns or by broader macroeconomic indicators from the Federal Reserve Economic Data (FRED) system.

The analysis combines the Iowa Liquor Sales dataset with selected macroeconomic indicators and compares three forecasting models:

- ARIMA
- Lasso Regression
- Random Forest

The results show that recent historical demand provides the strongest predictive signal, while the selected macroeconomic indicators add only limited additional short-run forecasting value.

## Why This Project Matters

Demand forecasting is important in retail because forecast errors affect inventory planning, staffing, pricing, and ordering decisions.

- Under-forecasting can lead to stockouts and lost sales.
- Over-forecasting can lead to excess inventory and inefficient use of capital.

This project asks whether adding macroeconomic information actually improves store-level forecasting or whether recent sales history remains the most useful source of predictive information.

## Research Question

To what extent do exogenous macroeconomic indicators and seasonal factors improve the predictive accuracy of store-level liquor demand forecasting compared with historical time-series data?

## Repository Structure

The repository is organized as follows:

- `README.md`
- `data/`
  - Iowa liquor sales data file
  - FRED macroeconomic data file
- `code/`
  - R script file used for the analysis

## Data Description

This project uses two publicly available datasets:

### 1. Iowa Liquor Sales Dataset
This dataset contains daily item-level liquor transaction records in Iowa. For this project, the raw data is aggregated to the weekly store level so that each observation represents one store in one week.

### 2. FRED Macroeconomic Data
This dataset contains monthly macroeconomic indicators downloaded from the Federal Reserve Economic Data system. These indicators are merged into the weekly store-level sales panel by matching each week to its corresponding month.

## Main Variables

### Outcome Variable
- `weekly_bottles`: total liquor bottles sold per store per week

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
Examples include:
- CPI / inflation
- unemployment
- employment
- consumer sentiment
- savings rate
- disposable personal income
- retail sales indicators

## Data Preparation

The workflow used in this project includes the following steps:

1. Load the raw Iowa Liquor Sales and FRED datasets.
2. Clean date fields and variable names.
3. Remove problematic or invalid sales records.
4. Aggregate liquor sales to the weekly store level.
5. Merge monthly FRED indicators into the weekly panel.
6. Create lagged demand and rolling average variables.
7. Split the data chronologically into:
   - training set: 2021–2024
   - test set: 2025

The year 2020 is excluded because the COVID-19 period introduced unusual purchasing behavior that would distort the forecasting comparison.

## Methods

This project compares three forecasting approaches:

### 1. ARIMA
ARIMA serves as the historical baseline model. It uses only past demand values and does not include macroeconomic variables.

### 2. Lasso Regression
Lasso includes lagged demand, store structure, seasonality, and macroeconomic indicators in a regularized linear framework. It is useful because it can shrink weaker predictors and handle correlated variables more effectively.

### 3. Random Forest
Random Forest provides a nonlinear benchmark and helps identify the relative importance of different predictors.

## Model Evaluation

Models are evaluated using out-of-sample forecasting performance on 2025 data.

The main evaluation metrics are:

- **RMSE (Root Mean Square Error)**
- **MAE (Mean Absolute Error)**

### Final Model Comparison

| Model | RMSE | MAE |
|---|---:|---:|
| ARIMA | 322.58 | 143.90 |
| Random Forest | 333.76 | 143.61 |
| Lasso | 333.93 | 137.35 |

These results suggest that historical demand remains very difficult to outperform. ARIMA achieved the best RMSE, while Lasso achieved the best MAE.

## Main Findings

The main findings of the project are:

- Weekly liquor demand shows strong short-run persistence.
- Urban stores sell more bottles than rural stores, but both follow similar time patterns.
- Lagged demand variables and rolling demand measures are the strongest predictors.
- The selected macroeconomic variables provide some additional information, but their contribution is weaker than recent demand history.
- More complex models did not clearly outperform a strong historical-demand baseline.

## Code Description

The main analysis file contains the full workflow for:

- data cleaning
- feature engineering
- exploratory data analysis
- ARIMA modeling
- Lasso modeling
- Random Forest modeling
- model comparison
- figure and table generation

## How to Run the Code

To reproduce the analysis:

1. Open RStudio.
2. Make sure the required packages are installed.
3. Place the raw data files in the correct folder paths used in the code.
4. Open the main `.qmd` or `.R` file.
5. Run the file from start to finish.

## Required R Packages

The project uses R and relies mainly on packages such as:

- `tidyverse`
- `lubridate`
- `zoo`
- `forecast`
- `glmnet`
- `randomForest`
- `ggplot2`
- `knitr`

Install any missing packages before running the code.

## Paper

The written report explains the research question, literature, data, methods, results, discussion, limitations, and conclusion in full detail.

## Additional Notes

- This project is predictive rather than causal.
- The analysis focuses on short-run forecasting at the weekly store level.
- The Random Forest model was estimated on a reduced training sample for computational efficiency.
- The macroeconomic indicators used here are monthly and aggregated, which may limit their usefulness for weekly store-level forecasting.
- All data used in this project are publicly available.

## Author

Duc Ngo  
DA 401 – Spring 2026  
Denison University
