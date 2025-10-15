# 🏀 NBA Shot Prediction Model (2024–25) — xFG% & Shot Maps

**Goal:** Predict **Expected Field Goal % (xFG%)** by shot location using NBA Stats API data, and visualize shot frequency, quality, and shot-making vs. expectation.

**Stack:** Python · Pandas · scikit-learn · Matplotlib · nba_api

---

## ✨ Highlights

- Pulls **shot-level data** via `nba_api` (`ShotChartDetail`)
- Normalizes **coordinates** to a single half-court (feet)
- Engineers features: distance, angle, 3PT/corner-3 flags, period, shot clock bins
- Trains **Logistic Regression** and **Gradient Boosted Trees (GBT)** for xFG%
- Evaluates with **Brier**, **LogLoss**, **Calibration Curve**
- Visualizes:
  - **Shot frequency** (hexbin)
  - **xFG% heatmap** (shot quality)
  - **Shot-making table**: **Actual FG% − xFG%** by player
- Exports **publication-ready images** and a **banner** for social posts

---

## 🔧 Quickstart

### 1) Environment
```bash
python -m venv .venv
# Windows
.\.venv\Scripts\activate
# Mac/Linux
source ./.venv/bin/activate

pip install --upgrade pip
pip install pandas numpy matplotlib scikit-learn nba_api tqdm jupyter
```

### 2) Run the notebook
Open **`nba_xfg_project.ipynb`** and run all cells.

By default, the notebook uses **NBA API** (2024–25, example team). To switch sources, edit the **End-to-End** cell:
```python
USE_KAGGLE = False  # keep False to use NBA API
```

Optional Kaggle path (if you ever want to run offline):
```python
KAGGLE_SHOTS_PATH = r"E:\Data\nba\shot_logs.csv"  # leave "" if not used
```

### 3) Outputs
Images and tables are saved to `outputs/`:
```
outputs/
  ├─ shot_frequency_hexbin.png
  ├─ xfg_heatmap.png
  ├─ calibration_curve.png
  ├─ shot_making_table.csv
  └─ banner.png   # good for LinkedIn/Twitter header
```

---

## 📊 Results (Current Run)

| Metric | Value |
|---|---|
| **MAE** | **0.1067** |
| **RMSE** | **0.1399** |
| **R²** | **0.2553** |

**Interpretation**
- **MAE ≈ 0.107** → Predictions are, on average, within ~10.7 percentage points of actual FG%.
- **R² ≈ 0.255** → Model explains ~25% of variation in FG% at the **zone** level (modest predictive power given limited features).

**Calibration**
- The calibration curve tracks the diagonal reasonably well in mid-probability ranges, indicating decent probability reliability. Divergence at extremes suggests room for better calibration/features.

---

## 🗺️ Visuals

- **Shot Frequency (Hexbin)** — Where shots are taken most often.
- **xFG% Heatmap** — Shot quality by location (independent of shot-making luck).
- **Shot-Making Table** — `Actual FG% − xFG%` by player (positive = outperforming expectation).

Each figure is exported at publication quality in `outputs/`.

---

## 🧠 What the Model Uses (Now)

**Features**
- `shot_distance_ft`, `shot_angle_deg`
- `is_three`, `is_corner_3` (NBA: 23'9" arc; 22' corners to 14')
- `period`, `shot_clock_bin`
- Optional (if available): `dribbles`, `touch_time`, `close_def_dist`

**Models**
- Logistic Regression (baseline)
- Gradient Boosted Trees (default best in this run)

**Evaluation**
- **Brier score**, **LogLoss**, **Calibration curve**
- Zone-level **Actual FG% vs xFG%** error analysis
- Additional numeric validation: **MAE**, **RMSE**, **R²**

---

## 🧐 Findings (Why R² is Modest & What It Means)

**R² ≈ 0.255** is expected because this version of the model uses **only location** (x, y). NBA shot outcomes depend on:
- **Shooter skill** (player identity/track record)
- **Defensive pressure** (closest defender)
- **Shot type/context** (catch-and-shoot vs pull-up, fast break vs halfcourt)
- **Clock situation** (shot clock, game clock, score context)

Additionally, some court bins have **few attempts**, so observed FG% is volatile (small-sample noise). The current system is great at **visualizing shot quality** from position alone and is **calibrated decently**, but it intentionally avoids player/context features to keep the demo minimal and reproducible.

---

## 🚀 Roadmap (Future Improvements)

- **Contextual features**: `SHOT_TYPE`, `SHOT_DISTANCE`, `CLOSEST_DEFENDER`, `SHOT_CLOCK`
- **Player priors**: season/career FG% as a feature or hierarchical shrinkage
- **More data**: multiple seasons for better generalization
- **Modeling**: XGBoost/LightGBM, calibration (isotonic/Platt), hyperparameter tuning (Optuna/GridSearchCV)
- **Defense**: team-level xFG% allowed heatmaps

---

## 🧪 Reproducibility Notes

- Random seed fixed: `RANDOM_SEED = 42`
- Train/test split is **stratified**
- Probabilities attached to test rows via **index-aligned Series** (avoid index errors)
- All plots are **matplotlib only** (one chart per figure)

---

## 🗂️ Repo Structure

```
.
├─ nba_xfg_project.ipynb    # main notebook (portfolio-ready)
├─ README.md                # this file
└─ outputs/                 # generated assets
   ├─ shot_frequency_hexbin.png
   ├─ xfg_heatmap.png
   ├─ calibration_curve.png
   ├─ shot_making_table.csv
   └─ banner.png
```

---

---

## ⚖️ License

MIT (feel free to adapt with attribution).

## Findings

1. **Shot Location Explains Part of the Story**  
   - With only location-based features, the model explains ~25% of the variance in actual FG%.  
   - This is strong enough for visualizing general shot quality trends, but limited for predicting exact outcomes.  

2. **Model Calibration is Decent**  
   - The calibration curve shows that mid-range and high-probability shots are predicted fairly well.  
   - Extreme values (very low or very high quality) need more context to improve accuracy.  

3. **Player Over/Under Performance is Visible**  
   - Players can be ranked by Actual FG% − xFG%, showing who is making tough shots and who is underperforming on good looks.  


## What I’d Do Differently in the Next Version

- **Add Contextual Features** — Include defender distance, shot type, and catch-and-shoot vs pull-up classification.  
- **Incorporate Player Skill Priors** — Add player career FG% or seasonal FG% to help adjust expectations.  
- **Multi-Season Data** — Train on several seasons to reduce small-sample noise in certain shot zones.  
- **Advanced Models** — Test XGBoost/LightGBM with hyperparameter tuning for better performance.  
- **Defense-Focused Analysis** — Create defensive xFG% maps to measure how teams impact opponent shot quality.
