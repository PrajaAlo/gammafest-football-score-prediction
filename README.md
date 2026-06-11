# Gammafest - Football Score Prediction

This repository contains the experiments, notebooks, datasets, and submissions developed for the **Gammafest Football Score Prediction Competition**. The primary goal of this workspace is to build machine learning models to accurately predict the scores of football matches.

---

## Prediction Target & Metrics

The objective is to predict two integer target variables for each football match-team row:
1. `team_goals` - The number of goals scored by the main team.
2. `opp_goals` - The number of goals scored by the opponent team.

The evaluation metric used is **Mean Absolute Error (MAE)** or its variants, measuring the average absolute difference between the predicted goals and the actual goals.

---

## Project Structure

```bash
Gammafest/
├── dataset/                     # Project datasets and submission outputs
│   ├── train.csv                # Training dataset (includes target variables)
│   ├── test.csv                 # Testing dataset (for evaluation)
│   ├── metadata.txt             # Variable definitions and metadata details
│   ├── sample submission.csv    # Benchmark submission template
│   ├── submission.csv           # Latest output submission
│   └── submission_xgb_bias_awmae.csv
├── experiments/                 # Directory containing modeling iterations
│   ├── percobaan 1 (3.15 mae)/  # Initial baseline setup (3.15 MAE)
│   ├── percobaan 2/             # Fixed iteration of Percobaan 1
│   ├── percobaan 3/             # Histogram-based postprocessing
│   ├── percobaan 4/             # Feature importance analysis and modeling
│   ├── percobaan 5 - simple baseline/     # CatBoost baseline, full ensembling, and "Jalur A" tests
│   ├── percobaan 6 - reconstructed features/ # CatBoost models trained on reconstructed features
│   ├── percobaan 7 - outcome classifier/     # Outcome classifier models using reconstructed features
│   ├── percobaan 8 - poisson bivariate/      # Poisson bivariate probability distribution modeling
│   └── percobaan 9 - dixon coles pseudo/     # Dixon-Coles model setup and pseudo-labeling
└── outputs/                     # Temporary artifacts and model outputs
    └── catboost_info/           # CatBoost training logs and performance reports
```

---

## Dataset Metadata & Features

The dataset comprises multiple categories of features, as detailed in `dataset/metadata.txt`:

1. **Identity & Core Info**:
   * `Id`: Unique row identifier (`match_id_team_name`).
   * `match_id`: Unique identifier for the match (shared between the home and away rows).
   * `date`: Match date.
   * `gender`: Match category (`M` for Men, `W` for Women).
   * `team`: Name of the primary team.
   * `opponent`: Name of the opposing team.

2. **Match Conditions**:
   * `is_home`: Binary flag (`1` = home team, `0` = away team).
   * `neutral`: Binary flag (`1` = neutral venue, `0` = non-neutral).
   * `tournament`: Name of the competition (e.g., FIFA World Cup, Friendly, etc.).
   * `venue_country`: Country hosting the match.
   * `confederation_team` / `opp`: Football confederation of the team/opponent (e.g., UEFA, CAF, CONMEBOL).

3. **Performance Metrics** *(available in `train.csv`)*:
   * `elo_team` / `elo_opponent`: Elo rating of each team.
   * `rank_team` / `rank_opponent`: FIFA ranking of each team.
   * `team_points_last5` / `last10`: Points accumulated in the last 5/10 matches.
   * `team_gd_last5`: Goal difference in the last 5 matches.
   * `team_win_rate_last10`: Win rate over the last 10 matches.
   * `h2h_points_last5`: Head-to-head points from the last 5 encounters.
   * `days_since_last_match`: Number of days since the team's last match.

4. **Geographics & Socio-Economics**:
   * `population_team` / `opp`: National population.
   * `gdp_per_capita_team` / `opp`: GDP per capita.
   * `altitude_venue`: Altitude of the venue.
   * `distance_travel_team` / `opp`: Estimated travel distance to the venue.
   * `temperature_venue`: Estimated temperature at the venue.

---

## Experiment Log & Methodology

### Baseline to Intermediate Iterations
* **Percobaan 1 (3.15 MAE)**: Initial baseline implementation with basic features.
* **Percobaan 2**: Error fixes and code cleanup from the first baseline iteration.
* **Percobaan 3**: Exploration of histogram-based postprocessing on goal predictions.
* **Percobaan 4**: Model refinement and analysis of `feature_importance.png` to find key drivers.

### Advanced Modeling
* **Percobaan 5 - Simple Baseline**: Deep dive into **CatBoost** modeling. Explored full ensembling of models and targeted ensembling strategies (e.g., "Jalur A" and ensembling prior to it).
* **Percobaan 6 - Reconstructed Features**: Feature engineering stage focused on reconstructing underlying features to better represent match-up dynamics under CatBoost.
* **Percobaan 7 - Outcome Classifier**: Built a classifier targeting match outcomes (Win/Draw/Loss) to guide and constrain regression-based goal predictions.
* **Percobaan 8 - Poisson Bivariate**: Formulated scores using a Bivariate Poisson distribution to model probabilities of specific scorelines (`team_goals` vs `opp_goals`).
* **Percobaan 9 - Dixon Coles Pseudo**: Advanced Dixon-Coles time-decay modeling specifically tuned for association football goal prediction, combined with pseudo-labeling.

---

## Getting Started

### Prerequisites
Make sure you have Python 3.8+ installed along with the following libraries:
```bash
pip install pandas numpy scikit-learn catboost xgboost lightgbm scipy statsmodels matplotlib seaborn
```

### Running the Notebooks
To run any experiment:
1. Open Jupyter Lab or Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
2. Navigate to the `experiments/` directory.
3. Open the notebook corresponding to the experiment of interest (e.g., `experiments/percobaan 9 - dixon coles pseudo/DSC26106_nasgor goreng_notebook.ipynb`) and run the cells.
