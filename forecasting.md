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
  - Autocorrelation 
    - Show the correlation of of time series with its lag (k) e.g r(k) will be the correlation between t(i) and t(i-k)
    - You can use correlogram (using acf in R) to see the auto-correlation
  - White Noise
    - Series with no auto correlation is white noise. 
    - We expect 95% of the spikes in the ACF to lie within ±2/sqrt(t) where T is the length of the time series.
    - for examples T=50 and the bounds are at ±2/sqrt(50) = ±0.28. All autocorrelation coefficients lie within these limits to confirm that data is a white noise.
    
Simple Forecasting Methods
--------------------------
  - Average Method
    - Forecast for all future values is equal to the average/mean of historical data
    - y(t+h|t) = (y(1) + y(2) + .. + y(n) ) / n
  - Naive Method
    - Use the last observed value as forecast
  - Seasonal Naive Method
    - Use the last observed value in the last year of the same month
  - Drift Method
    - Method is similar to naive method except the values are increased/decreased by drift factor.
    - The amount of change over time (called the drift) is set to be the average change seen in the historical data. 
    - y(n+h) = y(n) + h * (y(t) - y(1))  

Simple Linear Regression
------------------------
  - Forecast and predictor variables are related using linear model
  - y = b0 + b1*x + E
    - bo - Intercept (Value of y when x is zero)
    - b1 - Slope (Change in Y for unit change in X)
    - E - Error Term (Dievation from underlying model)
  - Error term should have couple of properties
    - have mean zero; otherwise the forecasts will be systematically biased.
    - are not autocorrelated; otherwise the forecasts will be inefficient as there is more information to be exploited in the data.
    - are unrelated to the predictor variable; otherwise there would be more information that should be included in the systematic part of the model.
  - Another important assumption in the simple linear model is that x is not a random variable.
  - With observational data (including most data in business and economics) it is not possible to control the value of x, and hence we make this an assumption.
  - There are many ways of finding b0 and b1 each giving out a different line. We try to find the line which minimizes square of dievation from actual values. This process is called least square estimation. 
  - Minimize {y(i) - b0 - b1*x(i)} ^ 2 for least squre estimation
  - The forecast values of y obtained from the models are called fitted values 
  - The difference between forecasted values and actual value is called is Residuals.
  - error(i) = y(i) - yfcst(i)
  - Properties:  sum(error(i)) = 0 and sum(error(i) * x(i) ) = 0

Regression and Correlation
--------------------------
  - Correlation cofficents measure the strength and direction (positive and nagative) between two variables.
  - Slope b1 = r * s(x) / s(y)
    - b1 - slope in regression
    - s(x) - standard dievation of x
    - s(y) - standard dieevation of y
    - r    - correlation cofficents
  - Correlation and regression are strongly linked
  - Regression assertion a predictive relationship between variables (e.g. x predicts y) but correlation doesn't.

Interpreting R Output
---------------------
```R
fit <- lm(Carbon ~ City, data=fuel)
abline(fit)
> summary(fit)
Coefficients:
             Estimate Std. Error t value Pr(>|t|)
(Intercept) 12.525647   0.199232   62.87   <2e-16 ***
City        -0.220970   0.008878  -24.89   <2e-16 ***
---
Signif. codes:  0 *** 0.001 ** 0.01 * 0.05 . 0.1   1

Residual standard error: 0.4703 on 132 degrees of freedom
Multiple R-squared: 0.8244,     Adjusted R-squared: 0.823
F-statistic: 619.5 on 1 and 132 DF,  p-value: < 2.2e-16
```
  - In the above output, we can write Carbon = -0.23 * City + 12.53
  - b0 = 12.53 indicates that if miles per gallon is zero (ie. x is zero) the value of Carbon emission is 12.53. This doesn't make sense in this case as car can't have zero miles per gallon.
  - b1 = -0.23 indicates that if the mpg (x) increase by 1 unit than carbon content decreases by -0.22 which means that as the mpg for car increases, carbon emission comes down.
  
Evaluating Regression Model
---------------------------
  - Residuals actual(y) - observed(y) 
  - It might be good to look at the plot of residuals and preditor variable(x).
  - We shouldn't note any pattern in the scatterplot i.e. the values should be randomly distributed.
  - Non Linear pattern may indicate
      - Non-linear relationship
      - heteroscedasticity (Residuals show non-constant variance)
      - there is some left over serial correlation 
  - Observations that take on extreme values compared to the majority of the data are called “outliers”. 
  - Observations that have a large influence on the estimation results of a regression model are called “influential observations
  - Goodness of fit (R^2) : R^2 refers to the variation of forecast variable explained by the model. 
  - R^2 is square of the cofficent of correlation between observed y values and predicted y values.
  - R^2 has value between 0 and 1.
  - Validating a model’s out-of-sample forecasting performance is much better than measuring the in-sample R2 value.
  - Standard error of regression is the standard dievation the residuals. 
  
Forecasting with Linear Regression
----------------------------------
  - We have following equation y = b0*x + b1 
      - To obtain the forecast for a new value of x, we put the value of x in the equation
      - The newly obtained value of y is called fitted value.
      - You have error in the forecast which can be calcualted by assuming that forecast are normally distributed (1.96 sd - 95% 1.28 sd - 80% )
Hypothesis Testing
------------------
  - Explore whether there is enough evidence that x and y are related.
  - Hyposthesis testing is used to explore the above fact e.g. x and y are related
  - We do a test to see if it is possible to have b1 = 0 for the observed data
  - We start with null hypostheis (thing which we want to disprove) that x and y are unrelated ie H0 => b1 = 0
  - Evidence against the null hypothesis is provided by value of b1
  - If b1 is different from zero than we have evidence against null hypothesis.
  - We calculate the probability of obtaining a value of b1 as large as we have calculated if the null hypothesis were true. This probability is known as the “P-value”.
```R
Coefficients:
             Estimate Std. Error t value Pr(>|t|)
(Intercept) 12.525647   0.199232   62.87   <2e-16 ***
City        -0.220970   0.008878  -24.89   <2e-16 ***
---
Signif. codes:  0 *** 0.001 ** 0.01 * 0.05 . 0.1   1

Residual standard error: 0.4703 on 132 degrees of freedom
Multiple R-squared: 0.8244,     Adjusted R-squared: 0.823
F-statistic: 619.5 on 1 and 132 DF,  p-value: < 2.2e-16
```
  - From above P-value is 2 X 10^-16 which indicates that probablity of null hypothesis being true is very low and hence we can reject the null hypothesis. 
  - To provide the interval for b1, we can use confidence levels
```R
 confint(fit,level=0.95)
                 2.5 %     97.5 %
(Intercept) 12.1315464 12.9197478
City        -0.2385315 -0.2034092
```
  - In this case b1 is in [-0.238 , -0.203] range for 95% confidence interval

Non Linear Functional Form
---------------------------
  - We assume that data is related linearly related
  - Tranformation of x or y series  leads to linear relationship with better fit
  - log-log transformation
    - log(y(i)) = b0+ b1* log(x(i)) + error(i)
    - the slope b1 here refers to  elasticity i.e. average percentage change in y from 1% change in x
    - Why log difference are equal to percentage change ?
      - Let's say we have x and it change by r percentage so new value is x(1+r) 
      - log(x(1+r)) = log(x) + log(1+r) = log(x) + r (becasue log(1+r) = r by taylor series)
      - log(x(1+r)) - log(x) = r ie. percentage difference
    -  the log transformation converts the exponential growth pattern to a linear growth pattern, and it simultaneously converts the multiplicative (proportional-variance) seasonal pattern to an additive (constant-variance) seasonal pattern

  
| Model         | Functional Form |   Slope          | Elasticity  |
| ------------- |:-------------:| -----:| ---------: | 
| Linear  | y(i) = b1 * x(i) + b0 | b1 | b1 * x / y | 
| Log Linear | log(y(i)) = b1 * log(x(i)) + b0 |     | b1* y / x | b1 |  
