# Urban-thermal-demand-prediction-in-Montreal

This repository contains a complete pipeline to simulate, train, and visualize thermal energy demand predictions across the city of Montreal. It combines high-resolution deterministic simulations with machine learning models to enable scalable, accurate, and rapid prediction of heating and cooling demand at the urban scale.



## Setup Instructions

To begin, first download this repository to your local machine. It is hosted at:

[Download the project folder](https://nextcloud.tech.beegroup-cimne.com/s/RSD7ciYFeFa9334)  


 Then, open a terminal and navigate to the main project folder:

```bash
cd Urban-thermal-demand-prediction-in-Montreal
```

Each main component of the project (ep_workflow, models, and visualization) contains its own requirements.txt file. To install the dependencies for any module, use:



```bash
pip install -r <folder>/requirements.txt
```

For example:


```bash
pip install -r ep_workflow/requirements.txt
```

---

## Project Structure

### `ep_workflow/`  — **Urban Energy Simulations with HUB**

- `input_files/`:
  - `MS_1_July23_23_40_3_MGB.geojson`: Full building stock of Montreal.
  - `montreal_residential_filtered.geojson`: Filtered dataset containing only residential buildings with valid construction year.
- `cache_files/`: Contains ~48,240 individual files with simulation outputs for each building (e.g., `results_real_14.parquet`).
- `out_files/`:
  - `all_results.parquet`: Aggregated simulation results for all simulated buildings.
- `scripts/`:
  - `ep_workflow.py`: Main simulation workflow that processes the input GeoJSON, builds neighborhood relationships, and runs EnergyPlus simulations via HUB.
  - `utils.py`: Support functions for building selection, geometry handling, and batch processing.

**Note:** The HUB library must be installed locally. It is available via [CERC Gitea](https://ngci.encs.concordia.ca/gitea/CERC/hub).

---

### `models/` — **Machine Learning Modeling and Inference**

This folder contains the data, code, and results related to training and deploying ML models for thermal demand estimation.

- `input_files/`:
  - `city_attributes_simulated.parquet`: Preprocessed attributes for buildings used in model training.
  - `city_attributes_non_simulated.parquet`: Attributes for buildings not simulated, used for city-scale extrapolation.

- `scripts/`:
  - `model_architecture.py`: Contains code to train, evaluate, and compare all ML models.
  - `model_extension.py`: Applies the best model to predict demand for the entire building stock.
  - `utils.py`: Shared helper functions used in both attribute extraction scripts.
 
- `results/`:
  - `results_df.csv`: Evaluation metrics (RMSE, MAE, $R^2$, etc.) for each trained model.
  - `preds_df_results.csv`: Final predictions for all non-simulated buildings in Montreal.
  - `saved_models/`: Trained models and scalers:
    - `linear_regression.pkl`, `random_forest.pkl`, `xgboost.pkl`
    - `mlp.h5`, `mlp_gridsearch.h5`, `mlp_optuna.h5`, `lstm.h5`
    - `preprocessor.pkl`, `target_scaler.pkl`
  - `plots/`: Supplementary figures:
    - Monthly prediction error plots (`*_monthly_scatter_plots.pdf`)
    - Comparison of model performance across different buildings (`comparativa_prediccions.pdf`)

---

### `visualization/` — **Interactive Streamlit App**

- `app.py`: Launches an interactive map to explore thermal demand predictions.
  - Select between predicted, simulated, or all buildings.
  - Filter by month and energy target (heating or cooling).
  - Points are color-coded based on demand intensity.

To run the app:
```bash
streamlit run visualization/app.py
```
