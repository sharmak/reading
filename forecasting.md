Time series forecasting
-----------------------
Explainatory Models 
-------------------
Predictor variables can be used in time series models e.g forecasting the electricity hourly demand can be modelled like following function:

  ED = f(temperature, time of work, weekday, hour, error)
  error - relationship might not be exact
Time Series Models
------------------
  ED(t+1) = f(ED(t), ED(t-1), ED(t-2)
  
  
  t - Hour of trading
  
  t+1 - Next hour of trading
  
  t-1 - Previous hour of trading

Problem with time series models is that they don't take into account the external factors e.g. temperature etc.
Dynamic Regression Models
---------------------------
Combination of time series and explainotory models


  ED(t+1) = f(current temperature, time of work, weekday, hour, ED(t), error)

Explainatory vs Time series Models
----------------------------------
  - Explainatory variables take into account external factors while time series model depend only on historical values
  - Some time it is hard to find the exact relationship 
  - It is necessary for forecast predictor variable in explainatory models
  - We might have predict the value not explain why ?
  - Time series models might be accurate than explainatory models.

Time series patterns
--------------------
  - Trend - Long term increase or decrease in data
  - Seasonality - time series is effected by day of the year or particular season
  - Cycle - A cycle occurs when the data exhibit rises and falls that are not of a fixed period.
Plots
------
  - Seasonal plots
  - Scatter plots
  - Scatter plot matrices

Univariate Statistics
----------------------
  - Mean 
  - Median
  - Mode
  - Percentiles
Bivariate Statistics
---------------------
  - Correlation cofficents (r) - Values range between +1 and  -1
  - Autocorrelation - Show the correlation of of time series with its lag (k) e.g r(k) will be the correlation between t(i) and t(i-k) 

  
