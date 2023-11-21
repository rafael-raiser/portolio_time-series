# Time series analysis of Brazil's average temperature

## Abstract

In this project we are going to analyze Brazil's average surface temperature starting from the end of the 19th century to the beginning of the 21st century. The dataset (available in [Kaggle](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data)) contains monthly measurements of average temperature for different countries and major cities.

Our goal is to study the seasonality of the data using seasonal decomposition, rolling mean analysis and ADF test, and to develop a prediction for the next years using SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors model). We also make a projection for the average temperature in 2050 using linear regression.

**timeSeriesAnalysis.ipynb**: contains the code for all the analysis performed.

## Methods

### Stationarity and ADF test

A time-series is said to be stationary if its properties, like average value, variance and covariance, do not vary over time (Figure 1). Determining if the dataset is stationary can be a hard task without a statistical objective tool. That's where tests like the Augmented Dickey-Fuller test (ADF) comes in.

THE ADT test is a unit root test to verify if the null hypothesis (which stands that the set is **not** stationary) is true. The test returns two important outputs: the p-value and the test statistic; the null hypothesis is rejected and the series is considered to be stationary if the p-value is smaller than a given criterium (usually 0.05) and the statistic smaller than the critical value.

![figure1](https://miro.medium.com/v2/resize:fit:1400/1*0vp3L0WZV_HkqS53H6skjg.png)

(Figure 1: Example of stationarity in time-series)

Another powerfull tool to study the time evolution of a time-series is the seasonal decomposition. In general, this kind of study decomposes the data, y(t), into differents components: the trend T(t) representing the growing or decreasing of the series, the cyclical component C(t) representing repeated but non-periodic variations, the seasonal component S(t) representing periodic variations and the irregular component I(t) or the noise. In its additive formulation, the decomposition takes the following form:

$$ y(t)=T(t)+C(t)+S(t)+I(t) $$

### SARIMAX

ARIMA (AutoRegressive Integrated Moving Average) is a model which takes as input a non-seasonal time-series alongside with parameters p, d, q, integers greater or equal to 0, to make predictions. The parameter p represents the number of lagged observations, d the number of differentiations needed to make the series stationary and q the moving average order. While ARIMA delivers good results, it's limited to non-periodic data, so SARIMAX comes in.

SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors) is considered to be an improvement of ARIMA and it's used to fit and make predictions for non-stationary series. Alongside to the (p,d,q) parameters, it takes also (P,D,Q,m) parameters related to the seasonality, where m stands for the number of periods in each season.

The optimal parameters for an individual model can be found by using statistical estimators, such as the AIC (Akaike Information Criterion). When a model is used to represent the process that generates the observed data, the representation will never be exact and some information will be lost; the AIC estimator is proportional to this loss and minimizing it means to find the model that best represents the dataset and thus is able to make the best predictions. Detailed informations about SARIMAX and AIC estimator can be found [here](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average) and [here](https://en.wikipedia.org/wiki/Akaike_information_criterion).

We are going to use the AIC estimator to obtain the parameters (p,d,q)x(P,D,Q,m) that optimizes the forecasting ability of our SARIMAX model.

## The dataset

The dataset we're going to analyze (available at [Kaggle](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data)) is a compilation of data put together by the project Berkeley Earth, which is affiliated with Lawrence Berkeley National Laboratory. It combines 1.6 billion temperature reports from 16 pre-existing archives and contains average global land temperatures for the whole planet, countries and cities.
We study here only data for Brazil's land temperature starting from 1832 and going up to 2013 (Figure 2).

![figure2](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/raw_plot.png)

(Figure 2: Brazil's average land temperature)

## Results

After cleaning the dataset and filling null values, we start the analysis by determining if the time-series (land average temperature vs date) is stationary. This is done by visualizing the rolling mean/standard deviation and by performing the ADF test (Figure 3). The test results are:

Test statistics: -3.439203

p-value: 0.009693

Number of lags used: 26

Number of observations used: 2154

Criticality at 1% : -3.4333895125643408

Criticality at 5% : -2.86288274571734

Criticality at 10% : -2.567484811553121

![figure3](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/test_stationarity.png)

(Figure 3: Rolling mean and standard deviation)

While it's possible to verify a very subtle growth trend that could mean that the series is not stationary, the ADF test wasn't able to recognize it and its results indicate a stationary series: p-value is smaller than 0.05 and the test statistics are very close to the critical value at 1% of tolerance.

This growing trend is also verified in the seasonal decomposition plots (Figure 4).

![figure4](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/seasonal_decomp.png)

(Figure 4: Seasonal decomposition)

The next step was to apply the SARIMAX model. Initially we searched for the combination of parameters (p,d,q)x(P,D,Q,m=12) that optimizes the result by minimizing the AIC estimator. We found the values (1, 0, 1)x(0, 1, 1, 12) for which AIC=2162.6.

Figure 5 shows a good agreement between SARIMAX forecast using these parameters and the observed values for years after 2010.

![figure5](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/sarimax_observed_vs_results.png)

(Figure 6: Comparison between SARIMAX forecast and observed values)

Then we performed a long-term estimation of averages temperatures. We applied our model up to year 2030 (200 periods of 1 month each after 2013) and the results along with the confidence intervals are plotted in Figure 6.

![figure6](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/sarimax_forecast.png)

(Figure 6: SARIMAX forecast for 200 months)

Finally, in order to catch the growing trend in average temperatures starting from the middle of the 20st century (as we saw in Figure 4) and to make a prediction for year 2050, we performed a linear regression over all the data after 1990. This resulted in an average temperature of 26.1 °C for that year (Figure 7).

![figure7](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/average_linearfit.png)

(Figure 7: Linear regression for data after 1990 vs rolling mean)











