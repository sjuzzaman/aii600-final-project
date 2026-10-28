# Proposal

At most one page. Replace the prompts under each heading. Leave the headings.

## Hypothesis

Historical taxi trip patterns, including time of day, day of the week, and 
previous pickup activity, can be used to predict hourly taxi demand in New York City. 
I hypothesize that taxi demand follows predictable daily and weekly patterns, with higher 
demand during commuting hours and weekends. The model will forecast the number of yellow 
taxi pickups in Manhattan during the next hour, measured in trips per hour.

## Data

This project will use the New York City Taxi and Limousine Commission (TLC) Yellow Taxi Trip Records 
from January through August 2026. The eight monthly Parquet files contain 29,703,355 individual taxi
trip records. Each record represents a completed taxi trip and includes pickup and drop-off timestamps, 
pickup and drop-off location identifiers, trip distance, and fare information. The records are collected 
through taxi technology systems and submitted to the TLC.

The project will use pickup timestamps and location identifiers to aggregate Manhattan taxi pickups into 
hourly totals, producing 5,832 expected hourly observations across 243 days. Data from January through June 
will be used for training, July for validation, and August for testing.

The dataset is publicly available through the New York City Taxi and Limousine Commission (TLC). NYC's open-data 
policy generally permits public reuse and republication, subject to applicable terms and source attribution. The 
project will credit the TLC as the original data provider. To avoid unnecessary duplication of large datasets, the 
original Parquet files will not be uploaded to GitHub. Instead, `data/README.md` will provide the official download
links and instructions for obtaining the files.

## Decision affected by the model

The model will help a New York City taxi fleet manager decide how many taxis to allocate to Manhattan during each hour. 
By predicting the number of taxi pickups in the next hour, the manager can deploy additional vehicles when demand is 
expected to be high and reduce unnecessary vehicle deployment when demand is expected to be low.

Underestimating demand is assumed to be more costly because insufficient taxi availability can result in longer passenger 
wait times, missed trips, and lost revenue. Overestimating demand can lead to excess idle vehicles and unnecessary operating 
costs. The model will help the manager balance these costs and make more efficient vehicle allocation decisions.