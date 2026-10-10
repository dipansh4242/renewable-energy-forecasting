# Renewable Energy Forecasting

A Python project that uses historical weather data from NASA POWER to estimate solar and wind energy generation and compare forecasting models for renewable energy output.

## Project Overview

The project downloads daily weather and solar-radiation data, estimates solar and wind generation using simplified assumptions, and trains forecasting models to predict:

- Solar energy generation (`solar_energy_mwh`)
- Wind energy generation (`wind_energy_mwh`)

The forecasting models included are:

- **Prophet** — time-series forecasting
- **XGBoost** — gradient-boosted decision trees
- **LSTM** — a recurrent neural network for sequential data

## Data Source

Weather and solar-radiation data are retrieved from the [NASA POWER project](https://power.larc.nasa.gov/). The script is configured for the coordinates `33.5779, -101.8313` and the period **1 January 2018 to 31 December 2024**.

The downloaded variables include:

- All-sky surface shortwave downward radiation
- Clear-sky surface shortwave downward radiation
- Wind speed at 10 m and 50 m
- Temperature at 2 m
- Relative humidity at 2 m
- Corrected precipitation

You can change the coordinates and dates in `src/data.ipynb` to use another location or period.

## Important Note About Generation Data

The weather data comes from NASA POWER, but the solar and wind energy values are **estimates calculated by this project**, not measured output from an actual power plant. The estimates use simplified assumptions for plant capacity, performance ratio, temperature effects, and a wind-turbine power curve. Results should therefore be treated as an educational forecasting exercise rather than a validated plant-performance assessment.

## Project Structure

```text
renewable-energy-forecasting/
├── README.md
├── requirements.txt
├── .gitignore
├── src/
│   ├── fetch_data.ipynb
│   └── train_models.ipynb
```

`generation_data.csv` is created when the data-fetching script runs and is excluded from Git by `.gitignore`.

## Requirements

- Python 3.10 or a compatible Python version supported by the installed packages
- An internet connection to retrieve NASA POWER data

Install the dependencies from the project directory:

```bash
pip install -r requirements.txt
```

The requirements file should include the packages used by the scripts, such as `requests`, `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `xgboost`, `prophet`, and `tensorflow`.

## How to Run

Run these commands from the project’s root directory.

### 1. Fetch and prepare the data

```bash
python src/data.ipynb
```

This downloads the daily data, cleans missing-value markers, calculates estimated solar and wind generation, adds calendar features, and writes `generation_data.csv`.

### 2. Train and compare forecasting models

```bash
python src/models.ipynb
```

The training script uses historical observations for training and reserves the final 180 days for testing. It compares model performance using regression metrics and creates prediction-comparison outputs.

## Outputs

Depending on the scripts’ successful execution, the project produces:

- `generation_data.csv` — prepared weather features and estimated energy generation
- `model_comparison.csv` — evaluation metrics for the forecasting models
- `forecast_comparison.png` — a plot comparing actual and predicted energy values

Generated data and result files can be kept locally; they do not need to be committed to GitHub unless you specifically want to publish them.

## Evaluation Metrics

The model-comparison script reports:

- **RMSE** — Root Mean Squared Error; penalizes larger errors more strongly
- **MAE** — Mean Absolute Error; average absolute prediction error
- **MAPE** — Mean Absolute Percentage Error; calculated only where actual values are non-zero
- **R²** — coefficient of determination, indicating how well predictions explain variation in the test data

Metrics should be compared on the same test period and interpreted in light of the fact that the target generation values are estimates.

## Limitations

- Generation estimates depend on simplified assumptions and may not represent a real plant.
- Actual solar/wind plant data, plant-specific specifications, and additional operational variables would improve the study.
- Model performance on the selected historical test period does not guarantee future forecasting accuracy.
- Before interpreting results, ensure that input features do not contain information derived from the target energy values, as this can cause data leakage and make scores misleading.

## Future Improvements

- Validate the estimates against measured plant-generation data.
- Tune model hyperparameters and compare additional models.
- Add weather forecasts for operational, forward-looking predictions.
- Include plant-specific technical and operational parameters.
- Add plots for residuals, seasonal performance, and feature importance.

## License

No license is specified yet.
