
# NBA Shot Quality vs. Shot-Making (xFG%)

**What this is**  
A portfolio-ready NBA analytics project that:
- Builds shot frequency hexbins and xFG% heatmaps
- Trains Logistic Regression and Gradient Boosted Trees to predict probability of a make (xFG%)
- Evaluates with Brier score, Log Loss, and a Calibration Curve
- Produces a shot-making table: Actual FG% − xFG% by player
- Saves visuals in `outputs/`, including a social banner

## Data Sources
- NBA Stats API via `nba_api` (`ShotChartDetail`)

**Please comply with NBA.com Terms of Use when accessing data.**

## Quickstart
1. `pip install pandas numpy matplotlib scikit-learn nba_api tqdm`
2. Open `nba_xfg_project.ipynb`
3. Choose either Kaggle or NBA API within the notebook and run all cells.
4. See results in `outputs/`.

## Engineered Features
- `shot_distance_ft`, `shot_angle_deg`
- `is_three`, `is_corner_3` (NBA: 23.75 ft arc; 22 ft corners to 14 ft)
- `period`, `shot_clock_bin`
- Optional: `dribbles`, `touch_time`, `close_def_dist` (if available)

## Visuals
- Shot frequency hexbin (player/team)
- xFG% heatmap (shot quality)
- Calibration curve
- Shot-making table (Actual FG% − xFG%)

## References
- nba_api ShotChartDetail docs
- Official NBA court specs (3-point arc and corner lengths)
- Brier score and calibration references
