# Predicting Superconducting Critical Temperature Using Machine Learning

## Project Overview

This project investigates the prediction of superconducting critical temperatures (\(T_c\)) using machine learning. It uses the UCI Superconductivity dataset and compares ordinary least squares (OLS) linear regression with XGBoost, a nonlinear gradient-boosted decision-tree model.

The project includes data inspection, preprocessing, model fitting, hyperparameter tuning, model evaluation, and visualisation of the predictions.

## Dataset

The dataset is obtained from the [UCI Machine Learning Repository – Superconductivity Data](https://archive.ics.uci.edu/dataset/464/superconductivty+data).

It contains 21,263 superconducting materials and includes two files:

- `train.csv`: 81 input features and the critical temperature (`critical_temp`) for each material.
- `unique_m.csv`: Elemental composition information, critical temperature, and chemical formula for each material.

### Obtaining the data

1. Download the dataset from the UCI Machine Learning Repository using the link above.
2. Extract the downloaded archive.
3. Place `train.csv` and `unique_m.csv` in the root directory of this project repository, alongside the Jupyter notebook.

The dataset files are not included in this repository. Please obtain them directly from the UCI source.

## Models and Results

The final model comparison is performed on the held-out test set. The reported results are:

| Model | Test RMSE (K) | Test MAE (K) | Test R² |
|---|---:|---:|---:|
| OLS | 17.7733 | 13.2669 | 0.7255 |
| XGBoost | 9.5605 | 5.1096 | 0.9206 |

XGBoost achieves lower prediction errors and a higher coefficient of determination than OLS on the test set.

Ridge regression is also explored during model development, but is not included in the final test-set comparison.

## Running the Notebook

1. Clone or download this repository.
2. Obtain the dataset files as described above and place both CSV files in the repository root.
3. Install the required Python packages:

   ```bash
   pip install numpy pandas matplotlib scikit-learn xgboost jupyter
   ```

4. Open `superconductor_prediction.ipynb` in Jupyter Notebook or JupyterLab.
5. Execute the notebook cells in order.

Additional details about the data split, preprocessing, model parameters, hyperparameter search, and final evaluation are provided in `REPRODUCIBILITY.md`.

## Dataset Citation

Hamidieh, K. (2018). *Superconductivty Data* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C53P47.

The dataset is distributed under the Creative Commons Attribution 4.0 International (CC BY 4.0) licence.
