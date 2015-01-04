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
| Log Linear | log(y(i)) = b1 * log(x(i)) + b0 |   b1 * y / x  | b1 |

Regression with time series data
---------------------------------
  - Biggest problem with time series data is that we don't have forecast for predictor varaibles
  - Scenario based forecasting (e.g. based on 1% increase in x, we will have following forecast)
    - We are assuming predictor variable for future
  - Ex-Ante Forecast
    - These forecast are made using only the informaton that is available in advance
    - e.g. the forecast for 2015 will only use the data which is available till today( dec 23, 2014)
    - These are genuine forecast
  - Ex-Post Forecast
    - They make use of later information about the predictor variables
    - These are not genuine forecast 
  - Residual Auto correlation
    - With time series data it is highly likely that the value of a variable observed in the current time period will be influenced by its value in the previous period, or even the period before that, and so on.
    - Therefore when fitting a regression model to time series data, it is very common to find autocorrelation in the residuals
    - Plot ACF for the residuals
  - Spurious Regressions
    - More often than not, time series data are “non-stationary”; that is, the values of the time series do not fluctuate around a constant mean or with a constant variance.
    - Regressing non-stationary time series can lead to spurious regressions. High R^2 and high residual autocorrelation can be signs of spurious regression.
    

Multiple Linear Regression
--------------------------
  - In multiple linear regression we have multiple predictor variables and one 
  - y(i) = b0 + b1*x1(i) + b2 * x2(i) + b3 * x3(i) + error(i)
    - y(i) is the forecast variable 
    - x1, x2, x3 are predictor variables
    - measures (b1,b2,b3) show the effect of predictor variable on forecast variable taking into account of other predictor variables. 
    - error is the error term in the regression
  - Errors shouldd have following properties
    - Errors should have mean zero
    - Errors should be uncorrelated with each other
    - Errors should be un-correlated with predictor variable
  - Estimation of model
    - We choose the values of b1, b2, bn so that sum of the squares of error is minimized
    - error is defined as difference between original value and fitted value.
```R
Coefficients:
             Estimate Std. Error t value  P value
(Intercept)    -0.219      5.231   -0.04   0.9667
log.savings    10.353      0.612   16.90  < 2e-16
log.income      5.052      1.258    4.02  6.8e-05
log.address     2.667      0.434    6.14  1.7e-09
log.employed    1.314      0.409    3.21   0.0014

Residual standard error: 10.16 on 495 degrees of freedom
Multiple R-squared: 0.4701, Adjusted R-squared: 0.4658
F-statistic: 109.8 on 4 and 495 DF,  p-value: < 2.2e-16 
```
    - In the above examole b1 = 10.353, b2 = 5.052, b3 = 2.667, b4 = 1.314
    - Standard Error gives the measure of uncertainity in the estimation of b(i)
    - "t value" is the ratio of a b coefficient to its standard error
    - "p-value": the probability of the estimated b coefficient being as large as it is if there was no real relationship between the forecast and the predictor 
  - R Square
    - R square is the square of correlation between acutal and fitted values
    - R square can also be interpreted as the varition in the forecast values explained by the model

- Useful Predictors
  - Dummy variables
    - Catagorical variables take eithe value 1 or 0 
    - for example, when forecasting credit scores and you want to take account of whether the customer is in full-type employment. So the predictor takes value "yes" when the customer is in full-time employment, and "no" otherwise.
    - A dummy variable is also known as an "indicator variable
  - Seasonal Dummy Variable
    - For example, suppose we are forecasting daily electricity demand and we want to account for the day of the week as a predictor
    - The interpretation of each of the coefficients associated with the dummy variables is that it is a measure of the effect of that category relative to the omitted category. 
    - In the above example, the coefficient associated with Monday will measure the effect of Monday compared to Sunday on the forecast variable.
  - Outliers
    - If there is an outlier in the data, rather than omit it, you can use a dummy variable to remove its effect.
    - In this case, the dummy variable takes value one for that observation and zero everywhere else.
  - Public Holidays
    - For daily data, the effect of public holidays can be accounted for by including a dummy variable predictor taking value one on public holidays and zero elsewhere.
  - Easter
    - Easter is different from most holidays because it is not held on the same date each year and the effect can last for several days. 
    - In this case, a dummy variable can be used with value one where any part of the holiday falls in the particular time period and zero otherwise.
    - For example, with monthly data, when Easter falls in March then the dummy variable takes value 1 in March, when it falls in April, the dummy variable takes value 1 in April, and when it starts in March and finishes in April, the dummy variable takes value 1 for both months.
- Austrialian Beer Forecast
  - We can model the Australian beer production data using a regression model with a linear trend and quarterly dummy variables:
  - yt=β0+β1t+β2d2,t+β3d3,t+β4d4,t+et,
    - here di,t=1 if t is in quarter i and 0 otherwise. 
    - The first quarter variable has been omitted
    - the coefficients associated with the other quarters are measures of the difference between those quarters and the first quarter.
  ```R
    Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 441.8141     4.5338  97.449  < 2e-16
trend        -0.3820     0.1078  -3.544 0.000854
season2     -34.0466     4.9174  -6.924 7.18e-09
season3     -18.0931     4.9209  -3.677 0.000568
season4      76.0746     4.9268  15.441  < 2e-16

Residual standard error: 13.01 on 51 degrees of freedom
Multiple R-squared: 0.921,  Adjusted R-squared: 0.9149
  ```
    - there is a strong downward trend of 0.382 megalitres per quarter
    - On average, the second quarter has production of 34.0 megalitres lower than the first quarter
    - the third quarter has production of 18.1 megalitres lower than the first quarter,
    - fourth quarter has production 76.1 megalitres higher than the first quarter.
    - The model explains 92.1% of the variation in the beer production data.
- Intervention variables
  - When the effect lasts only for one period, we use a spike variable. This is a dummy variable taking value one in the period of the intervention and zero elsewhere.
  - Spike variable is similar to dummy variable for outlier
  - A step variable takes value zero before the intervention and one from the time of intervention onwards.
  - Another form of permanent effect is a change of slope. Here the intervention is handled using a piecewise linear trend 
- Selecting Predictor
  - It is hard to choose between various predictor variable
  - One common wrong approach is to plot the predictor variable with forecast variable and see the effect on scatterplot but this does't take into account the effect of other predictor variable.
  - Other common wrong approach is to do a multiple linear regression on discard the predictor with disregard all variables whose p-values are greater than 0.05.
  - statistical significance does not always indicate predictive value. Even if forecasting is not the goal, this is not a good strategy because the p-values can be misleading when two or more predictors are correlated with each other
  - Adjusted R^2
    - Imagine a model which produces forecasts that are exactly 20% of the actual values. In that case, the R^2 value would be 1 (indicating perfect correlation), but the forecasts are not very close to the actual values.
    - R^2 does not allow for "degrees of freedom''. Adding any variable tends to increase the value of R2, even if that variable is irrelevant
    - An equivalent idea is to select the model which gives the minimum sum of squared errors (SSE),
    - Minimizing the SSE is equivalent to maximizing R2 and will always choose the model with the most variables, and so is not a valid way of selecting predictors.
    - Adjusted R^2 is SSE/(n-k-1)  
      - n  no of observations
      - k  no of predictors
    - Adjusted R^2 no longer increase with each added variable 
- Cross Validation
  - Cross-validation is a very useful way of determining the predictive ability of a model.
  - Leave-one-out cross-validation
    - Remove observation i from the data set, and fit the model using the remaining data. 
    - Compute the error (e∗i=yi−y^i) for the omitted observation. (This is not the same as the residual because the ith observation was not used in estimating the value of y^i.)
    - Repeat step 1 for i=1,…,N.
    - Compute the MSE from e∗1,…,e∗N. We shall call this the CV.
    - Under this criterion, the best model is the one with the smallest value of CV.
 - Other criteria's 
    - Akaike's Information Criterion
    - Corrected Akaike's Information Criterion
    - Schwarz Bayesian Information Criterion
 - The model with all four predictors has the lowest CV, AIC, AICc and BIC values and the highest R^2 value
 - Residual Diagnostic 
  - The residuals from a regression model are calculated as the difference between the actual values and the fitted values: ei=yi−y^i. 
  - Each residual is the unpredictable component of the associated observation.
  - Scatterplots of residuals against predictors
  - Scatterplot of residuals against fitted values
  - Autocorrelation in the residuals
    - you should look at an ACF plot of the residuals. This will reveal if there is any autocorrelation in the residuals (suggesting that there is information that has not been accounted for in the model).
    - Durbin-Watson test is used to test the hypothesis that there is no lag one autocorrelation in the residuals.
    - If there is no autocorrelation, the Durbin-Watson distribution is symmetric around 2.
    - A small p-value indicates there is significant autocorrelation remaining in the residuals. 
  - Histogram of residuals
 


