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