# Reproducibility Notes

## 1. Data Split

The UCI Superconductivity dataset contains 21,263 samples and 81 input features. The target variable is `critical_temp`.

The dataset is divided into training, validation, and test sets using `train_test_split` from scikit-learn, with a random seed of 42 for both splits.

| Dataset | Number of samples | Proportion |
|---|---:|---:|
| Training | 14,884 | 70% |
| Validation | 3,189 | 15% |
| Test | 3,190 | 15% |

The validation set is used for model selection and hyperparameter tuning. The test set is reserved for final evaluation and is not used during model development.

## 2. Preprocessing and Models

### Ordinary Least Squares (OLS)

OLS regression is implemented using scikit-learn's `LinearRegression`. The model is initially fitted on the training set and evaluated on the validation set. It is subsequently refit using the combined training and validation sets for final evaluation.

### Ridge Regression

Ridge regression is explored using scikit-learn's `Ridge`. Input features are standardised using `StandardScaler`, fitted exclusively on the training set and applied to the validation and test sets.

The regularisation parameter, $\alpha$, is selected from 50 logarithmically spaced values between $10^{-4}$ and $10^4$, based on validation RMSE.

### XGBoost

XGBoost regression is implemented using `XGBRegressor` with the squared-error objective (`reg:squarederror`).

The baseline configuration is:

| Parameter | Value |
|---|---:|
| Learning rate | 0.02 |
| Maximum depth | 16 |
| Minimum child weight | 1 |
| Column subsampling | 0.5 |
| Row subsampling | 0.5 |
| Number of estimators | 750 |
| Random seed | 42 |

The optimal number of trees for the baseline model is determined using validation RMSE.

## 3. XGBoost Hyperparameter Search

A random search is conducted over 50 configurations sampled using scikit-learn's `ParameterSampler`, with random seed 42. The search space consists of:

| Parameter | Values |
|---|---|
| Learning rate | 0.010, 0.015, 0.020 |
| Maximum depth | 15–25 |
| Minimum child weight | 1, 10 |
| Column subsampling | 0.25, 0.50, 0.75 |
| Row subsampling | 0.5 (fixed) |

The initial search evaluates each configuration with up to 750 estimators. The five best configurations are then extended to 1,500 estimators and evaluated using validation RMSE.

The selected configuration is:

| Parameter | Value |
|---|---:|
| Learning rate | 0.015 |
| Maximum depth | 19 |
| Minimum child weight | 10 |
| Column subsampling | 0.75 |
| Row subsampling | 0.5 |
| Number of estimators | 1,069 |

This configuration yields the lowest validation RMSE among the five extended configurations.

## 4. Final Evaluation

After hyperparameter selection, the training and validation sets are combined to form the final training dataset. The selected XGBoost and OLS models are refitted on this combined dataset.

Both models are then evaluated on the held-out test set using root mean squared error (RMSE), mean absolute error (MAE), and the coefficient of determination ($R^2$).

The final test-set performance metrics are reported in the README.

## AI-Use Disclosure

ChatGPT (OpenAI) was used for conceptual explanations, Python code assistance, troubleshooting, data-analysis guidance, and documentation. AI-assisted code was executed and its outputs, including the data split, hyperparameter selection, evaluation metrics, and plots, were independently checked by the author. The final analysis and interpretation were reviewed by the author.

## References

1. Hamidieh, K. (2018). A Data-Driven Statistical Model for Predicting the Critical Temperature of a Superconductor. *Computational Materials Science*, 154, 346–354.

2. Hamidieh, K. (2018). Superconductivity Data. UCI Machine Learning Repository. https://doi.org/10.24432/C53P47.

3. Chen, T. and Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 785–794.

4. Scikit-learn documentation: https://scikit-learn.org/stable/
