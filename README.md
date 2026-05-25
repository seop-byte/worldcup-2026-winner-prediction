# 2026 FIFA World Cup Performance Prediction

## 1. Project Overview

This project predicts the expected performance level of national football teams in the 2026 FIFA World Cup using player market values, FIFA rankings, and recent international match results.

The goal of this project is not to perfectly predict the actual winner of the 2026 World Cup. Instead, the project aims to analyze which teams are likely to achieve relatively high performance based on quantitative features such as best-11 market value, recent match performance, and FIFA ranking.

The prediction target is defined as a `stage_score`, which converts each team's World Cup final stage into a numerical value. Therefore, this project is formulated as a regression problem.

## 2. Problem Definition

World Cup performance is affected by many factors, such as player quality, team strength, tactical organization, injuries, match-ups, and tournament randomness. However, many pre-tournament predictions rely on subjective opinions or simple rankings.

This project attempts to evaluate national team strength more objectively by using public football datasets and machine learning models. The main research question is:

> Can player market value, FIFA ranking, and recent international match performance explain or predict national teams' World Cup performance?

## 3. Datasets

This project uses three public datasets from Kaggle.

### 3.1 Football Data from Transfermarkt

- Source: Kaggle - Football Data from Transfermarkt
- License: CC0 1.0 Universal
- Main files used:
  - `players.csv`
  - `player_valuations.csv`
- Purpose:
  - To obtain player nationality, position, and market value.
  - To construct national team best-11 market value features.

### 3.2 International Football Results: Daily Updates

- Source: Kaggle - International Football Results: Daily Updates
- License: CC0 1.0 Universal
- Main files used:
  - `all_matches.csv`
  - `countries_names.csv`
- Purpose:
  - To calculate national team match performance before each World Cup.
  - Features include points per match, goals per match, goals against per match, and weighted points per match.

### 3.3 FIFA World Ranking

- Source: Kaggle - FIFA World Ranking
- License: CC0 1.0 Universal
- Main file used:
  - `fifa_ranking-2024-06-20.csv`
- Purpose:
  - To calculate average FIFA ranking.
  - To adjust match performance by opponent strength.

## 4. Feature Engineering

The main features used in this project are:

- Best-11 market value by position group:
  - GK market value
  - DF market value
  - MF market value
  - FW market value
- Average FIFA ranking
- Points per match
- Weighted points per match
- Goals for per match
- Goals against per match

### Weighted Points per Match

A custom feature called `weighted_points_per_match` was designed to reflect opponent strength.

General points are calculated as:

- Win: 3 points
- Draw: 1 point
- Loss: 0 points

However, this treats a win against a strong team and a win against a weak team equally. To address this limitation, this project adjusts points using the FIFA ranking difference between the team and its opponent.

If the opponent is stronger, a win or draw receives a higher weight. If the opponent is weaker, the reward is reduced. For losses, losing to a weaker team receives a larger penalty.

## 5. Target Variable

The target variable is `stage_score`, which converts World Cup performance into a numerical value.

Example stage score mapping:

| Stage | Score |
|---|---:|
| Group stage | 0 |
| Round of 16 | 1 |
| Quarter-final | 2 |
| Semi-final | 3 |
| Runner-up | 4 |
| Champion | 5 |

Because the target is numerical, this project is treated as a regression task.

## 6. Models

The following models were trained and compared:

- Mean Baseline
- Linear Regression
- Ridge Regression
- Decision Tree Regressor
- Random Forest Regressor
- Gradient Boosting Regressor

A simple mean baseline was used for comparison. The baseline predicts the average `stage_score` of the training data for every test team.

## 7. Experimental Setup

The main experiment uses a year-based train/test split.

- Training data:
  - 2006 World Cup
  - 2010 World Cup
  - 2014 World Cup
  - 2018 World Cup

- Test data:
  - 2022 World Cup

This split is used because the goal is to simulate a realistic prediction setting, where past World Cup data is used to predict a future World Cup.

Hyperparameter tuning was performed using 3-fold cross-validation on the training data.

Evaluation metrics:

- MAE
- RMSE
- R2 Score

## 8. Main Results

In the basic model comparison, Random Forest Regressor achieved the lowest RMSE among the untuned models.

After hyperparameter tuning, Gradient Boosting Regressor achieved the best performance on the 2022 test set.

The final 2026 prediction was performed using the tuned Gradient Boosting Regressor.

## 9. 2026 Prediction

The model predicts the expected `stage_score` for the 2026 World Cup candidate teams.

The output should not be interpreted as a direct winning probability. Instead, it represents a data-based ranking of teams that are expected to achieve relatively high performance according to the selected features.

## 10. Limitations

This project has several limitations.

- World Cup data is limited because the tournament is held only once every four years.
- Market value does not directly capture team chemistry, tactics, injuries, or player condition.
- Tournament-specific factors such as group draw, match-ups, red cards, and penalty shootouts are not directly included.
- Some older data, especially from 2006, may have missing or incomplete player market value information.
- The predicted stage score is not a direct probability of winning the World Cup.

Therefore, the model should be interpreted as a data-driven analysis tool rather than a fully reliable deployment-ready prediction system.

## 11. How to Run

### Option 1: Run in Google Colab

1. Open `worldcup_2026_prediction.ipynb` in Google Colab.
2. Install the required libraries if needed.
3. Run all cells from top to bottom.
4. The notebook will generate:
   - Model performance comparison
   - Hyperparameter tuning results
   - 2022 test-set prediction analysis
   - 2026 predicted stage score ranking

### Option 2: Run Locally

1. Clone this repository.

```bash
git clone https://github.com/seop-byte/worldcup-2026-winner-prediction.git
cd worldcup-2026-winner-prediction
