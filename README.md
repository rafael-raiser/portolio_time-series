# Time series analysis of Brazil's average temperature

In this project we are going to analyze Brazil's average surface temperature starting from the end of the 19th century to the beginning of the 21st century. The dataset (available in [Kaggle](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data)) contains monthly measurements of average temperature for different countries and major cities.

Our goal is to study the seasonality of the data using seasonal decomposition, rolling mean analysis and ADF test, and to develop a prediction for the next years using SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors model). We also make a projection for the average temperature in 2050 using linear regression.

## Methods

### Stationarity and ADF test

A time-series is said to be stationary if its properties, like average value and variance, do not vary over time (Figure 1). Determinar se um conjunto de dados é estacionário pode ser difícil sem uma ferramenta estatística objetiva. Aí entram testes como o Augmented Dickey-Fuller Test (ADT).
O teste ADT é um unit root test que verifica se a null hypothesis é verdadeira. Neste caso, esta hipótese é de que o conjunto não é estacionário. Os principais outputs do teste são o p-value e a estatística; a hipótese nula é rejeitada e a série é considerada estacionária se o p-value é menor que um limite (usualmente igual a 0.05) e a estatística do teste é menor que certos valores críticos. Veremos mais detalhadamente o modus operandi do teste ADF em Python nas próximas seções.

![figure1](https://miro.medium.com/v2/resize:fit:1356/1*0LIVYzFe16KexcebBz7fwg.png)

(Figure 1: Example of stationarity in time-series. The first series has non-stationary mean, the second one non-stationary variance and the third one is stationary.




