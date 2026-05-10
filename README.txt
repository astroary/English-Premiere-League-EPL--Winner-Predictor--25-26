# ⚽ EPL Winner Predictor (2025/26 Season)

An objective, evidence-based machine learning model designed to predict the winner of the English Premier League using historical performance, financial data, and statistical modeling.

## 📋 Problem Description
This project addresses the uncertainty and bias often found in football forecasting. By analyzing key factors such as transfer spending, managerial success, and scoring efficiency, the tool provides a data-driven perspective on league outcomes rather than relying on opinion or narrative. It identifies patterns historically linked to finishing 1st to offer a more objective evaluation of potential champions.

## 📊 Dataset Architecture & Curation
A significant portion of this project involved extensive manual data engineering. While raw match logs were sourced from Kaggle, every attribute used in the final model was manually aggregated and calculated from individual match rows.

### 1. Historical Training Data (`dataset_for_model.csv`)
The core model was trained on 14 seasons of data (2010/11 – 2023/24). We performed manual aggregation of individual match statistics to create season-level attributes:
* **Manually Aggregated Match Stats**: Total Shots, Shots on Target, Corners, Fouls, and Yellow/Red Cards per team per season.
* **Manually Calculated Performance Metrics**: Total Points (Home/Away), Goals Scored/Conceded, and Goal Difference.
* **Team Success Metrics**: Total Team Wins and Team Win Percentages.

### 2. 2025/26 Prediction Features (`prediction_2025_26.csv`)
For the upcoming season forecast, we manually collected and verified "preseason" indicators that do not exist in standard historical datasets:
* **Financial Data**: Preseason Transfer Budgets (Net/Gross Spend) sourced from public club financial records.
* **Managerial Data**: Manager Names and manually calculated Manager Career Win Percentages at the start of the 25/26 campaign.

### 3. Final Modeling Dataset (`final_combined_dataset_for_model.csv`)
The primary file used for the modeling phase was the **final_combined_dataset_for_model.csv**. This dataset is a comprehensive merge of the `dataset_for_model.csv` historical data and the `prediction_2025_26.csv` variables. By combining these into a single dataset, the model evaluated current team profiles against over a decade of historical league trends.

## 🛠️ Machine Learning Approach
We utilized a combination of classification and regression models to predict both the league winner and final point totals.

* **Random Forest Classifier (Calibrated)**: Captures non-linear interactions between historical performance, finances, and form while correcting for overconfidence via probability calibration.
* **Gradient Boosting Classifier**: Used to model subtle feature interactions missed by linear models under extreme class imbalance.
* **Gradient Boosting Regressor**: Specifically used to estimate total final league points, capturing complex continuous-value functions and non-linear effects.
* **Feature Engineering**: Utilized Exponentially-Weighted Moving Averages (EWMA) for "Recency Form" to identify teams trending upward or downward based on momentum.

## 📈 Evaluation Metrics
To account for the extreme class imbalance in league winner prediction, we used:
* **Top-N Accuracy**: Measures if the true league winner is ranked among the model's top N predicted teams.
* **RMSE**: Evaluated point prediction accuracy against season-adjusted targets.
* **Probability Calibration**: Ensured fair comparison between teams' win likelihoods and avoided overconfident predictions.

CS 441: Applied ML, Prof. Derek Hoiem, Fall 2025
University of Illinois Urbana-Champaign
