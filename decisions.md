# Decisions

This file is the history of the project. Append an entry when a choice changes the work: the quantity, the data, the split, the model, the loss, or the action. Leave old entries in place. A later entry can reverse an earlier one. If a result depends on a random draw, record the seed under Why. The split entry records the cutoff date, or the seed and the fraction. The pull request that merges the code names this entry.

## YYYY-MM-DD: short title

- Decision. What you chose.
- Why. The evidence.
- Alternative. What you rejected.
- Revisit. What would change the choice.

## 2026-10-28: Select NYC Taxi Demand Forecasting Project

- **Decision.** Use the NYC TLC Yellow Taxi Trip Records from January through August 2026 to forecast the number of Manhattan taxi pickups per hour. Aggregate individual trip records into 5,832 expected hourly observations. Use January–June for training, July for validation, and August for testing. The model will support decisions about hourly taxi allocation in Manhattan.

- **Why.** The eight monthly datasets contain 29,703,355 trip records with pickup timestamps and location identifiers needed for hourly forecasting. Eight months provide more observations and seasonal variation than a three-month dataset. A chronological split preserves the order of observations and allows evaluation on future data. Training ends June 30, validation ends July 31, and testing covers August 1–31, 2026. No random seed is required.

- **Alternative.** Use only January–March 2026 or randomly divide observations into training and testing sets. The shorter period was rejected because it provides less seasonal variation. Random splitting was rejected because it does not reflect how the model would be used to forecast future demand.

- **Revisit.** Reconsider the time period or split if the datasets contain substantial missing or invalid records, if the model performs poorly on later months, or if additional historical data is needed to account for seasonal patterns.

## 2026-11-5: Chronological train, validation, and test split

- **Decision.** Divide the hourly Manhattan taxi pickup dataset chronologically, using January 1 through June 30, 2026, for training; July 1 through July 31, 2026, for validation; and August 1 through August 31, 2026, for testing.
- **Why.** The project forecasts future hourly taxi demand using historical pickup activity. A chronological split preserves the order of observations and prevents future periods from being included in the training data. July provides a separate period for model selection, while August provides an independent final evaluation.
- **Alternative.** A random train-test split was considered but rejected because it would mix earlier and later observations and could produce overly optimistic estimates of forecasting performance.
- **Revisit.** Reconsider the split if more months of data become available or if the model requires evaluation across multiple historical forecast periods.


## 2026-11-7: Seasonal Naive Forecasting Baseline

- **Decision.** Use a seasonal naive forecasting method that predicts hourly Manhattan yellow taxi pickups based on the observed pickup count during the same hour seven days earlier.
- **Why.** Taxi demand is expected to follow recurring daily and weekly patterns. A seasonal naive baseline provides a simple benchmark for evaluating whether more complex machine learning models improve forecasting performance.
- **Alternative.** Predicting the overall average hourly demand was considered but rejected because it does not account for variations associated with different hours and weekdays.
- **Revisit.** Reconsider the baseline if exploratory analysis shows weak weekly patterns or if alternative simple forecasting methods produce lower prediction errors.


## 2026-11-8: Random Forest Regression Model

- **Decision.** Use a Random Forest Regressor to forecast hourly Manhattan yellow taxi pickups. The model uses calendar features and historical pickup counts from one hour, 24 hours, and 168 hours earlier. Train on January–June 2026, select hyperparameters using July validation performance, and reserve August for final testing.
- **Why.** Random Forest can model nonlinear relationships between previous pickup activity, time of day, and weekly demand patterns. It can capture interactions among features without requiring a predefined linear relationship. Mean Absolute Error (MAE), measured in pickups per hour, is used for initial model selection. A fixed random seed of 42 is used for reproducibility.
- **Alternative.** Linear Regression was considered as a simpler model but was not selected for the initial experiment because it would require explicit feature transformations and interactions to represent complex demand patterns.
- **Revisit.** Reconsider the model if it fails to outperform the seasonal naive baseline on validation data, if it performs poorly on August test data, or if another approach provides better forecasting accuracy or lower operational decision costs.

## 2026-11-8: Asymmetric Taxi Allocation Decision Loss

- **Decision.** Evaluate the Seasonal Naive Baseline and Random Forest using an asymmetric taxi-allocation loss function. Assume each taxi can serve two pickups per hour, each pickup exceeding available capacity costs \$15, and unused taxi capacity costs $8 per taxi-hour. Use July validation residuals to calibrate an allocation adjustment and evaluate the resulting decisions on August test data.
- **Why.** Underestimating taxi demand is assumed to be more expensive than overestimating demand because insufficient capacity may cause missed trips and lost revenue. The asymmetric loss function incorporates both shortage costs and excess capacity costs. The values are illustrative rather than estimates of actual NYC fleet expenses. Using the same assumptions for both forecasting methods allows a consistent comparison of operational decisions.
- **Alternative.** Evaluate the models only using forecasting metrics such as MAE and RMSE, or allocate taxis directly from the predicted demand without a safety adjustment. These alternatives do not fully account for the assumed difference between shortage and excess-capacity costs.
- **Revisit.** Reconsider the loss assumptions if actual taxi operating costs, vehicle utilization rates, or passenger demand information become available. Reevaluate the allocation rule if different cost ratios substantially change the results.