# Time series analysis of Brazil's average temperature

## Abstract

In this project we are going to analyze Brazil's average surface temperature starting from the end of the 19th century to the beginning of the 21st century. The dataset (available in [Kaggle](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data)) contains monthly measurements of average temperature for different countries and major cities.

Our goal is to study the seasonality of the data using seasonal decomposition, rolling mean analysis and ADF test, and to develop a prediction for the next years using SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors model). We also make a projection for the average temperature in 2050 using linear regression.

**timeSeriesAnalysis.ipynb**: contains the code for all the analysis performed.

## Methods

### Stationarity and ADF test

A time-series is said to be stationary if its properties, like average value, variance and covariance, do not vary over time (Figure 1). Determinar se um conjunto de dados é estacionário pode ser difícil sem uma ferramenta estatística objetiva. Aí entram testes como o Augmented Dickey-Fuller Test (ADT).
O teste ADT é um unit root test que verifica se a null hypothesis é verdadeira. Neste caso, esta hipótese é de que o conjunto não é estacionário. Os principais outputs do teste são o p-value e a estatística; a hipótese nula é rejeitada e a série é considerada estacionária se o p-value é menor que um limite (usualmente igual a 0.05) e a estatística do teste é menor que certos valores críticos. Veremos mais detalhadamente o modus operandi do teste ADF em Python nas próximas seções.

![figure1](https://miro.medium.com/v2/resize:fit:1400/1*0vp3L0WZV_HkqS53H6skjg.png)

(Figure 1: Example of stationarity in time-series)

A powerfull tool to study the time evolution of a time-series is the seasonal decomposition. In general, this kind of study decomposes the data, y(t), into differents components: the trend T(t) representing the growing or decreasing of the series, the cyclical component C(t) representing repeated but non-periodic variations, the seasonal component S(t) representing periodic variations and the irregular component I(t) or the noise. In its additive formulation, the decomposition takes the following form:

$$ y(t)=T(t)+C(t)+S(t)+I(t) $$

### SARIMAX

ARIMA (AutoRegressive Integrated Moving Average) is a model which takes as input a non-seasonal time-series alongside with parameters p, d, q, integers greater or equal to 0, to make predictions. The parameter p represents the number of lagged observations, d the number of differentiations needed to turn the series stationary and q the moving average order. While ARIMA delivers good results, it's limited to non-periodic data.

In other hand, SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors) is considered an improvement of ARIMA used to fit and make predictions for non-stationary series. Alongside to the (p,d,q) parameters, it takes also (P,D,Q,m) parameters related to the seasonality, where m stands for the number of periods in each season.

The optimal parameters for an individual model can be found by using statistical estimators, such as the AIC (Akaike Information Criterion). When a model is used to represent the process that generated the observed data, the representation will never be exact and some information will be lost; the AIC estimator is proportional to this loss and minimizing it means to find the model that best represents the dataset and thus is able to make the best predictions.

We are going to use the AIC estimator to obtain the parameters (p,d,q)x(P,D,Q,m) that optimizes the forecasting ability of our SARIMAX model.

## The dataset

The dataset we're going to analyze is a compilation of data put together by the project Berkeley Earth, which is affiliated with Lawrence Berkeley National Laboratory. It combines 1.6 billion temperature reports from 16 pre=existing archives and contains average global land temperatures for the whole planet, countries and cities.
We study here only data for Brazil's land temperature starting from 1832 and going to 2013 (Figure 2).

![figure2](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/raw_plot.png)

(Figure 2: Brazil's average land temperature)

## Results

Após limpar o conjunto de dados e eliminar os valores nulos, iniciamos a análise determinando se a série temporal é estacionária através da visualização da rolling mean/std and ADF test (Figure 3). O resultado do teste ADF foi:

Test statistics: -3.439203

p-value: 0.009693

Number of lags used: 26

Number of observations used: 2154

Criticality at 1% : -3.4333895125643408

Criticality at 5% : -2.86288274571734

Criticality at 10% : -2.567484811553121

![figure3](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/test_stationarity.png)

(Figure 3: Rolling mean and standard deviation)

Embora seja possível identificar uma sutil tendência de aumento da temperatura média com o tempo, o que poderia indicar uma série com média não estacionária, o teste não foi capaz de reconhecê-la e seu resultado indica uma série estacionária, com p-value menor que o critério de 0.05 e estatística aproximadamente igual ao valor crítico a 1% de tolerância.

Esta tendência de crescimento da média também fica visível na seasonal decomposition (Figure 4).

![figure4](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/seasonal_decomp.png)

(Figure 4: Seasonal decomposition)

A próxima etapa foi a aplicação do método SARIMAX. Inicialmente, buscamos uma combinação de parâmetros (p,d,q)x(P,D,Q,m=12) que otimizassem o resultado, minimizando o índice AIC. Encontramos os parâmetros (1, 0, 1)x(0, 1, 1, 12) com AIC=2162.6.

Figure 5 shows a good agreement between SARIMAX forecast and the observed value for years after 2010.

![figure5](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/sarimax_observed_vs_results.png)

(Figure 6: Comparison between SARIMAX forecast and observed values)

The next step was to optain a long-term estimation of averages temperatures. We applied our model up to year 2030 (200 months after 2013) and the results along with the confidence intervals are plotted in Figure 6.

![figure6](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/sarimax_forecast.png)

(Figure 6: SARIMAX forecast for 200 months)

Finally, in order to catch the growing in average temperatures starting from the middle of the 20st century (as we can see in the trend evolution of Figure 4) and to make a prediction for year 2050, we performed a linear regression over. This resulted in an average temperature of 26.1 °C for that year (Figure 7).

![figure7](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/average_linearfit.png)

(Figure 7: Linear regression for data after 1990 vs rolling mean)














