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

### SARIMAX

## The dataset

The dataset we're going to analyze is a compilation of data put together by the project Berkeley Earth, which is affiliated with Lawrence Berkeley National Laboratory. It combines 1.6 billion temperature reports from 16 pre=existing archives and contains average global land temperatures for the whole planet, countries and cities.
We study here only data for Brazil's land temperature starting from 1832 and going to 2013 (Figure 2).

![figure2](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/raw_plot.png)

(Figure 2: Brazil's average land temperature)

## Results

Após limpar o conjunto de dados e eliminar os valores nulos, iniciamos a análise determinando se a série temporal é estacionária através da visualização da rolling mean/std and ADF test (Figure 3). O resultado do teste ADF foi:

Test stat               -3.439203
p-value                  0.009693
#lags used              26.000000
#observations used    2154.000000
dtype: float64
criticality at 1% : -3.4333895125643408
criticality at 5% : -2.86288274571734
criticality at 10% : -2.567484811553121

![figure3](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/test_stationarity.png)

(Figure 3: Rolling mean and standard deviation)

Embora seja possível identificar uma sutil tendência de aumento da temperatura média com o tempo, o que poderia indicar uma série com média não estacionária, o teste não foi capaz de reconhecê-la e seu resultado indica uma série estacionária, com p-value=0.009<0.05 e estatística aproximadamente igual ao valor crítico a 1% de tolerância.

Esta tendência de crescimento da média também fica visível na seasonal decomposition (Figure 4).

![figure4](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/seasonal_decomp.png)

(Figure 4: Seasonal decomposition)

A próxima etapa foi a aplicação do método SARIMAX. Inicialmente, buscamos uma combinação de parâmetros pdq e seasonal-pdq que otimizassem o resultado, minimizando o índice AIC. Encontramos os parâmetros (1, 0, 1)x(0, 1, 1, 12) com AIC=2162.6.

Utilizando estes parâmetros, obtivemos os seguintes resultados:

==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
ar.L1          0.8297      0.017     47.704      0.000       0.796       0.864
ma.L1         -0.4401      0.028    -15.709      0.000      -0.495      -0.385
ma.S.L12      -0.9199      0.009    -99.934      0.000      -0.938      -0.902
sigma2         0.1580      0.004     42.899      0.000       0.151       0.165
==============================================================================

Figure 5 shows a good agreement between SARIMAX forecast and the observed value for years after 2010.

![figure5](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/sarimax_observed_vs_results.png)

(Figure 5: Comparison between SARIMAX forecast and observed values)

![figure6](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/sarimax_forecast.png)

(Figure 6: SARIMAX forecast for 200 months)

We performed simple linear regression on data after 1990 in order to capture the growing tendency of the average temperature and to make a prediction for year 2050. We obtain this way that 

![figure7](https://github.com/rafael-raiser/portolio_time-series/blob/main/images/average_linearfit.png)

(Figure 7: Linear regression for data after 1990 vs rolling mean)














