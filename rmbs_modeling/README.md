How do we predict prepayment which along with expected loss (from a default prediction model) would allow us to price an MBS?

## Notebooks
- `rmbs_data_eda.ipynb`: exploratory data analysis and feature engineering
- `prepayment_model.ipynb`: XGB regression, hyperparameter tuning, evaluation + RCA post-results

In the first notebook, I do a lot of EDA to get a sense of what these features are.

In the second notebook, I implement an XGB regression model using random search CV. The model performance is summarized below:

Metric | Train | Validation | Test |
| --- | --- | --- | --- |
| **MAE** | 0.0051 | 0.0057 | 0.0091 |
| **R-squared** | 0.89 | 0.86 | 0.74 |

The test metrics at first glance show a loss in predictive power of the model. However, I did some RCA by looking at the MAPE (between predicted and actuals) as a function of time. The diagnosis is clear: around month 132, a sudden event caused borrowers to flock toward re-financing options that raised the CPR. It was accompanied by the following observations:
- cumulative incentive goes up. But notice how it was moving slowly and then accelerated! 
- incentive goes up
- burnout goes down. It depends on cum incentive anyway, but this means the pool has tons of borrowers who can still re-finance
- hpa goes up: rising home equity makes refinancing easier to qualify for
- ltv goes down. A sign of low risk for borrowers, which accompanies low interest rates

Stay tuned for the next part: using HMM to detect regime changes, followed by post-hoc adjustments on XGB model predictions
