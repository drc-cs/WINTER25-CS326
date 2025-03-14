---
title: COMP_SCI 326
separator: <!--s-->
verticalSeparator: <!--v-->
theme: serif
revealOptions:
  transition: 'none'
---

<div class = "col-wrapper">
  <div class="c1 col-centered">
  <div style="font-size: 0.8em; left: 0; width: 70%; position: absolute;">

  # Introduction to Data Science Pipelines
  ## L.14 Time Series Analysis

  </div>
  </div>
  <div class="c2 col-centered" style = "bottom: 0; right: 0; width: 80%; padding-top: 30%">
  <iframe src="https://lottie.host/embed/bd6c5b65-d724-4f97-882c-40f58367ea38/BIKhZdSeqW.json" height="100%" width = "100%"></iframe>
  </div>
</div>

<!--s-->

## Announcements

- H.02 is due 02.25 @ 11:59PM.

- Added [silhouette score pseudocode](https://drc-cs.github.io/WINTER25-CS326/lectures/L10_clustering/#/18) to clustering lecture.

<!--s-->

<div class="header-slide">

# L.14 Time Series Modeling

</div>

<!--s-->

<div class = "col-wrapper">
  <div class="c1 col-centered">
    <div style="font-size: 0.8em; left: 0; width: 60%; position: absolute;">

# Intro Poll
## On a scale of 1-5, how confident are you with the **time series** methods such as:

1. Seasonal Decomposition
2. Stationarity & Differencing
3. Autocorrelation
4. ARIMA

  </div>
  </div>
  <div class="c2" style="width: 50%; height: 100%;">
  <iframe src="https://drc-cs-9a3f6.firebaseapp.com/?label=Intro Poll" width="100%" height="100%" style="border-radius: 10px"></iframe>
  </div>

</div>

<!--s-->

## Agenda

Today we're going to talk about time series analysis, specifically building an intution for forecasting models. We'll cover the following topics:

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

### Understanding Time Series Data
- Seasonal Decomposition
- Stationarity & Differencing
- Autocorrelation

### Time Series Forecasting
- Moving Average
- Autoregressive Models
- ARIMA (Theory)
- ARIMA (Practice)

### Evaluation
- Walk-Forward Validation
- Evaluation Metrics

</div>
<div class="c2 col-centered" style = "width: 50%">

<div>
<img src="https://storage.googleapis.com/slide_assets/forecast.jpg" style="margin: 0 auto; display: block;">
<span style="font-size: 0.6em; padding-top: 0.5em; text-align: center; display: block; color: grey;">Lokad 2016</span>
</div>
</div>
</div>

<!--s-->

<div class="header-slide">

# Understanding Time Series Data

</div>

<!--s-->

## Understanding Time Series | Seasonal Decomposition

<div class = "col-wrapper">
<div class="c1" style = "width: 50%; margin: 0; padding: 0;">

### Trend
The long-term movement of a time series. That represents the general direction in which the data is moving over time.

### Seasonality
The periodic fluctuations in a time series that occur at regular intervals. For example, sales data may exhibit seasonality if sales increase during the holiday season.

### Residuals
Noise in a time series that cannot be explained by the trend or seasonality.

</div>
<div class="c2 col-centered" style = "width: 50%">
<div>

<img src="https://sthalles.github.io/assets/time-series-decomposition/complete-seasonality-plot-additive.png" width="400" style="margin: 0; padding: 0; display: block;">
<span style="font-size: 0.6em; padding-top: 0.5em; text-align: center; display: block; color: grey;">Thalles 2019</span>

</div>
</div>`
</div>


<!--s-->

## Understanding Time Series | Seasonal Decomposition

Seasonal Decomposition is a technique used to separate a time series into its trend, seasonal, and residual components. Seasonal decomposition can help identify patterns in the time series data and make it easier to model. It can be viewed as a form of feature engineering.

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

### Additive Seasonal Decomposition
The seasonal component is added to the trend and residual components.

$$ X_t = T_t + S_t + R_t $$

### Multiplicative Seasonal Decomposition

The seasonal component is multiplied by the trend and residual components.
$$ X_t = T_t \times S_t \times R_t $$

</div>

<div class="c2" style = "width: 50%; margin-top: 8%;">
<div>
<img src="https://sthalles.github.io/assets/time-series-decomposition/complete-seasonality-plot-additive.png" width="400" style="margin: 0 auto; display: block;">
<span style="font-size: 0.6em; padding-top: 0.5em; text-align: center; display: block; color: grey;">Thalles 2019</span>
</div>
</div>
</div>

<!--s-->

## Understanding Time Series | Seasonal Decomposition


Seasonal Decomposition is a technique used to separate a time series into its trend, seasonal, and residual components. Seasonal decomposition can help identify patterns in the time series data and make it easier to model. It can be viewed as a form of feature engineering.

<div class = "col-wrapper">
<div class="c1" style = "width: 50%; height: 100%;">

### **Question**: Does this figure represent additive or multiplicative decomposition?

*Answer -- additive.

<img src="https://sthalles.github.io/assets/time-series-decomposition/complete-seasonality-plot-additive.png" width="400" style="margin: 0 auto; display: block;">
<span style="font-size: 0.6em; padding-top: 0.5em; text-align: center; display: block; color: grey;">Thalles 2019</span>

</div>

<div class="c2" style = "width: 50%; height: 100%;">
<iframe src="https://drc-cs-9a3f6.firebaseapp.com/?label=L.12 | Q.01" width="100%" height="100%" style="border-radius: 10px;"></iframe>
</div>

<!--s-->

## Understanding Time Series | Stationarity

A time series is said to be **stationary** if its statistical properties such as mean, variance, and autocorrelation do not change over time. Many forecasting methods assume that the time series is stationary. The **Augmented Dickey-Fuller Test (ADF)** is a statistical test that can be used to test for stationarity.

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

### Strict Stationarity
The joint distribution of any subset of time series observations is independent of time. This is a strong assumption that is rarely met in practice.

### Trend Stationarity
The mean of the time series is constant over time. This is a weaker form of stationarity that is more commonly used in practice.

</div>
<div class="c2" style = "width: 50%">

<div>
<img src="https://upload.wikimedia.org/wikipedia/commons/e/e1/Stationarycomparison.png" style="margin: 0; display: block;">
<span style="font-size: 0.6em; padding-top: 0.5em; text-align: center; display: block; color: grey;">Wikipedia 2024</span>
</div>
</div>
</div>

<!--s-->

## Understanding Time Series | Differencing

**Differencing** is a technique used to make a time series **stationary** by computing the difference between consecutive observations. Differencing can help remove trends and seasonality from a time series.

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

$$ Y_t' = Y_t - Y_{t-1} $$

Where:
- $Y_t$ is the observation at time $t$.
- $Y_t'$ is the differenced observation at time $t$.

</div>
<div class="c2" style = "width: 50%">

<img src="https://storage.googleapis.com/blogs-images-new/ciscoblogs/1/2020/03/0e3efdd8-differencing.png" width="400" style="margin: 0 auto; display: block; border-radius: 10px;">
<span style="font-size: 0.6em; text-align: center; display: block; color: grey;">Wise, 2020</span>

</div>
</div>

<!--s-->

## Understanding Time Series | Autocorrelation

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

### Autocorrelation
A measure of the correlation between a time series and a lagged version of itself. 

$$ \text{Corr}(X_t, X_{t-k}) $$


### Partial Autocorrelation
A measure of the correlation between a time series and a lagged version of itself, controlling for the values of the time series at all shorter lags.

$$ \text{Corr}(X_t, X_{t-k} | X_{t-1}, X_{t-2}, \ldots, X_{t-k+1}) $$

</div>
<div class="c2 col-centered" style = "width: 50%">
<div>
<img src = "https://i.makeagif.com/media/3-17-2017/CYdNJ7.gif" width="100%" style="margin: 0 auto; display: block; border-radius: 10px;">
<span style="font-size: 0.5em; text-align: center; display: block; color: grey; padding-top: 0.5em;">@osama063, 2016</span>
</div>
</div>
</div>

<!--s-->

## Understanding Time Series | Autocorrelation

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

### Autocorrelation
A measure of the correlation between a time series and a lagged version of itself. 

$$ \text{Corr}(X_t, X_{t-k}) $$


### Partial Autocorrelation
A measure of the correlation between a time series and a lagged version of itself, controlling for the values of the time series at all shorter lags.

$$ \text{Corr}(X_t, X_{t-k} | X_{t-1}, X_{t-2}, \ldots, X_{t-k+1}) $$

</div>
<div class="c2 col-centered" style = "width: 50%">
<div>
<img src="https://storage.googleapis.com/cs326-bucket/lecture_13/observed.png" width="100%" style="margin: 0 auto; display: block;">
<img src="https://storage.googleapis.com/cs326-bucket/lecture_13/auto2.png" width="100%" style="margin: 0 auto; display: block;">
</div>
</div>
</div>


<!--s-->

## Understanding Time Series | Checkpoint TLDR;

### Seasonal Decomposition
A technique used to separate a time series into its trend, seasonal, and residual components.

### Stationarity
A time series is said to be stationary if its basic properties do not change over time.

### Differencing
A technique used to make a time series stationary by computing the difference between consecutive observations.

### Autocorrelation
A measure of the correlation between a time series and a lagged version of itself. Partial autocorrelation controls for the values of the time series at all shorter lags.


<!--s-->

<div class="header-slide">

# Time Series Forecasting

</div>

<!--s-->

## Time Series Forecasting | Introduction

Time series forecasting is the process of predicting future values based on past observations. Time series forecasting is used in a wide range of applications, such as sales forecasting, weather forecasting, and stock price prediction. 

The **ARIMA** (Autoregressive Integrated Moving Average) model is a popular time series forecasting model that combines autoregressive, moving average, and differencing components.

Before we dive into ARIMA, let's first discuss two simpler time series forecasting models to build intuition for the components of ARIMA: **Moving Average (MA)** and **Autoregressive (AR)** Models.

<!--s-->

## Time Series Forecasting | Autoregressive Models

**Autoregressive Models (AR)**: A type of time series model that predicts future values based on past observations. The AR model is based on the assumption that the time series is a linear combination of its past values. It's primarily used to capture the periodic structure of the time series.

AR(1) $$ X_t = \phi_1 X_{t-1} + c + \epsilon_t $$

Where:

- $X_t$ is the observed value at time $t$.
- $\phi_1$ is a learnable parameter of the model.
- $c$ is a constant term (intercept).
- $\epsilon_t$ is the white noise at time $t$.

<!--s-->

## Time Series Forecasting | Autoregressive Models

**Autoregressive Models (AR)**: A type of time series model that predicts future values based on past observations. The AR model is based on the assumption that the time series is a linear combination of its past values. It's primarily used to capture the periodic structure of the time series.

AR(p) $$ X_t = \phi_1 X_{t-1} + \phi_2 X_{t-2} + \ldots + \phi_p X_{t-p} + c + \epsilon_t $$

Where:

- $X_t$ is the observed value at time $t$.
- $p$ is the number of lag observations included in the model.
- $\phi_1, \phi_2, \ldots, \phi_p$ are the parameters of the model.
- $c$ is a constant term (intercept).
- $\epsilon_t$ is the white noise at time $t$.

<!--s-->

## Time Series Forecasting | Autoregressive Models

**Autoregressive Models (AR)**: A type of time series model that predicts future values based on past observations. The AR model is based on the assumption that the time series is a linear combination of its past values. It's primarily used for capturing the periodic structure of the time series.

$$ X_t = \phi_1 X_{t-1} + \phi_2 X_{t-2} + \ldots + \phi_p X_{t-p} + c + \epsilon_t $$

<iframe width = "100%" height = "70%" src="https://storage.googleapis.com/cs326-bucket/lecture_13/ARIMA_1_2.html" title="scatter_plot"></iframe>

<!--s-->

## Time Series Forecasting | Moving Average

**Moving Average (MA) Models**: A type of time series model that predicts future values based on the past prediction errors. A MA model's primary utility is to smooth out noise and short-term discrepancies from the mean.

MA(1) $$ X_t = \theta_1 \epsilon_{t-1} + \mu + \epsilon_t$$

<div class = "col-wrapper" style="font-size: 0.8em;">
<div class="c1" style = "width: 50%">

Where: 

- $X_t$ is the observed value at time $t$.
- $\theta_1$ is a learnable parameter of the model.
- $\mu$ is the mean of the time series.
- $\epsilon_t$ is the white noise at time $t$.

</div>
<div class="c2" style = "width: 50%">

Example with a $\mu = 10 $ and $\theta_1 = 0.5$:

| t | $\widehat{X}_t$ | $\epsilon_t$ | $X_t$ |
|---|------------|--------------|-------|
| 1 | 10         | -2            | 8    |
| 2 | 9         | 1           | 10    |
| 3 | 10.5         | 0            | 10.5    |
| 4 | 10         | 2           | 12     |
| 5 | 11         | -1           | 10    |


</div>
</div>

<!--s-->

## Time Series Forecasting | Moving Average

**Moving Average (MA) Models**: A type of time series model that predicts future values based on the past prediction errors. A MA model's primary utility is to smooth out noise and short-term discrepancies from the mean.

MA(1) $$ X_t = \theta_1 \epsilon_{t-1} + \mu + \epsilon_t$$

<iframe width = "100%" height = "70%" src="https://storage.googleapis.com/cs326-bucket/lecture_13/MA2.html" title="scatter_plot";></iframe>

<!--s-->

## Time Series Forecasting | Moving Average

**Moving Average (MA) Models**: A type of time series model that predicts future values based on the past prediction errors. A MA model's primary utility is to smooth out noise and short-term discrepancies from the mean.

MA(q) $$ X_t = \theta_1 \epsilon_{t-1} + \theta_2 \epsilon_{t-2} + \ldots + \theta_q \epsilon_{t-q} + \mu + \epsilon_t$$

Where: 

- $X_t$ is the observed value at time $t$.
- $q$ is the number of lag prediction errors included in the model.
- $\theta_1, \theta_2, \ldots, \theta_q$ are the learnable parameters.
- $\mu$ is the mean of the time series.
- $\epsilon_t$ is the white noise at time $t$.

<!--s-->

## Time Series Forecasting | ARMA

**Autoregressive Models with Moving Average (ARMA)**: A type of time series model that combines autoregressive and moving average components.

The ARMA model is defined as:

$$ X_t = \phi_1 X_{t-1} + \phi_2 X_{t-2} + \ldots + \phi_p X_{t-p} + \theta_1 \epsilon_{t-1} + \theta_2 \epsilon_{t-2} + \ldots + \theta_q \epsilon_{t-q} + c + \epsilon_t $$


Where:

- $X_t$ is the observed value at time $t$.
- $\phi_1, \phi_2, \ldots, \phi_p$ are the autoregressive parameters.
- $\theta_1, \theta_2, \ldots, \theta_q$ are the moving average parameters.
- $c$ is a constant term (intercept).
- $\epsilon_t$ is the white noise at time $t$.


<!--s-->

## Time Series Forecasting | ARIMA

**Autoregressive Integrated Moving Average (ARIMA)**: A type of time series model that combines autoregressive, moving average, and differencing components.

The ARIMA model is defined as: 

$$ y_t' = \phi_1 y_{t-1}' + \phi_2 y_{t-2}' + \ldots + \phi_p y_{t-p}' + \theta_1 \epsilon_{t-1} + \theta_2 \epsilon_{t-2} + \ldots + \theta_q \epsilon_{t-q} + c + \epsilon_t $$

Where:

- $y_t'$ is the differenced observation at time $t$.
- $\phi_1, \phi_2, \ldots, \phi_p$ are the autoregressive parameters.
- $\theta_1, \theta_2, \ldots, \theta_q$ are the moving average parameters.
- $c$ is a constant term (intercept).
- $\epsilon_t$ is the white noise at time $t$.

<!--s-->

## Time Series Forecasting | Practical ARIMA

ARIMA takes three parameters when fitting a model ($p$, $d$, $q$). 

<div style="font-size: 0.7em;">

| Parameter | Description | Estimation |
|-----------|-------------|------------|
| $p$ | The number of lag observations included in the model (lag order for autoregression). | Where there is a dropoff in Partial Autocorrelation Function (PACF) (with gradual decline in ACF). |
| $d$ | The number of times that the raw observations are differenced (degree of differencing). | Minimum amount of differencing required to achieve a significant Augmented Dickey-Fuller Test (ADF). |
| $q$ | The number of prediction errors included in the model (order of moving average). | Where there is a dropoff in the Autocorrelation Function (ACF) (with gradual decline in PACF). |

</div>


<!--s-->

## Time Series Forecasting | Practical ARIMA

ARIMA takes three parameters when fitting a model ($p$, $d$, $q$). 

<div style="font-size: 0.7em;">

| Parameter | Description | Estimation |
|-----------|-------------|------------|
| $p$ | The number of lag observations included in the model (lag order for autoregression). | Where there is a dropoff in Partial Autocorrelation Function (PACF) (with gradual decline in ACF). |
| $d$ | The number of times that the raw observations are differenced (degree of differencing). | Minimum amount of differencing required to achieve a significant Augmented Dickey-Fuller Test (ADF). |
| $q$ | The number of prediction errors included in the model (order of moving average). | Where there is a dropoff in the Autocorrelation Function (ACF) (with gradual decline in PACF). |

</div>

**Question**: What is a reasonable value of $p$ based on the following? 
Answer: 2

<img src = "https://storage.googleapis.com/cs326-bucket/lecture_13/ar.png" width="60%" style="margin: 0 auto; display: block;">
<span style="font-size: 0.5em; text-align: center; display: block; color: grey; ">Spur Economics 2022</span>

<!--s-->

## Time Series Forecasting | Practical ARIMA

ARIMA takes three parameters when fitting a model ($p$, $d$, $q$). 

<div style="font-size: 0.7em;">

| Parameter | Description | Estimation |
|-----------|-------------|------------|
| $p$ | The number of lag observations included in the model (lag order for autoregression). | Where there is a dropoff in Partial Autocorrelation Function (PACF) (with gradual decline in ACF). |
| $d$ | The number of times that the raw observations are differenced (degree of differencing). | Minimum amount of differencing required to achieve a significant Augmented Dickey-Fuller Test (ADF). |
| $q$ | The number of prediction errors included in the model (order of moving average). | Where there is a dropoff in the Autocorrelation Function (ACF) (with gradual decline in PACF). |

</div>

**Question**: What is a reasonable value of $d$ based on the following? 
Answer: 1

```python
import numpy as np
from statsmodels.tsa.stattools import adfuller

timeseries = ...
for d in range(0, 3):
    diffed = np.diff(timeseries, n=d)
    result = adfuller(diffed)
    print(f"ADF Statistic for d={d}: {result[0]} p-value: {result[1]}")
```
```text
ADF Statistic for d=0: -2.5 p-value: 0.1
ADF Statistic for d=1: -3.2 p-value: 0.04
ADF Statistic for d=2: -4.1 p-value: 0.01
``` 

<!--s-->

## Time Series Forecasting | Practical ARIMA

ARIMA takes three parameters when fitting a model ($p$, $d$, $q$). 

<div style="font-size: 0.7em;">

| Parameter | Description | Estimation |
|-----------|-------------|------------|
| $p$ | The number of lag observations included in the model (lag order for autoregression). | Where there is a dropoff in Partial Autocorrelation Function (PACF) (with gradual decline in ACF). |
| $d$ | The number of times that the raw observations are differenced (degree of differencing). | Minimum amount of differencing required to achieve a significant Augmented Dickey-Fuller Test (ADF). |
| $q$ | The number of prediction errors included in the model (order of moving average). | Where there is a dropoff in the Autocorrelation Function (ACF) (with gradual decline in PACF). |

</div>

**Question**: What is a reasonable value of $q$ based on the following? 
Answer: 2

<img src = "https://storage.googleapis.com/cs326-bucket/lecture_13/ma.png" width="60%" style="margin: 0 auto; display: block;">
<span style="font-size: 0.5em; text-align: center; display: block; color: grey; ">Spur Economics 2022</span>

<!--s-->

<div class="header-slide">

# Forecast Evaluation

</div>

<!--s-->

## Walk-Forward Validation

In walk-forward validation, the model is trained on historical data and then used to make predictions on future data. The model is then retrained on the updated historical data and used to make predictions on the next future data point. This process is repeated until all future data points have been predicted.

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

### Train / Validate Period
The historical data used to train and validate the time series model.

### Test Period
The future data used to evaluate the generalization performance of the time series model.

</div>
<div class="c2" style = "width: 50%">

<img src = "https://storage.googleapis.com/slide_assets/walk-forward.png" width="100%" style="margin: 0 auto; display: block;">
<span style="font-size: 0.5em; text-align: center; display: block; color: grey;">Peeratiyuth, 2018</span>

</div>
</div>

<!--s-->

## Walk-Forward Validation

In walk-forward validation, the model is trained on historical data and then used to make predictions on future data. The model is then retrained on the updated historical data and used to make predictions on the next future data point. This process is repeated until all future data points have been predicted.

<div class = "col-wrapper">
<div class="c1" style = "width: 50%">

### Train / Validate Period
The historical data used to train and validate the time series model.

### Test Period
The future data used to evaluate the generalization performance of the time series model.

</div>
<div class="c2" style = "width: 50%">

<img src = "https://storage.googleapis.com/slide_assets/holdout-walk-forward.png" width="100%" style="margin: 0 auto; display: block;"> 
<span style="font-size: 0.5em; text-align: center; display: block; color: grey;">Karaman, 2005</span>

</div>
</div>


<!--s-->

## Evaluation Metrics

<div style = "font-size: 0.8em">

**Mean Absolute Error (MAE)**: The average of the absolute errors between the predicted and actual values.

$$ MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i| $$

**Mean Squared Error (MSE)**: The average of the squared errors between the predicted and actual values.

$$ MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 $$

**Root Mean Squared Error (RMSE)**: The square root of the average of the squared errors between the predicted and actual values.

$$ RMSE = \sqrt{ \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 } $$

**Mean Absolute Percentage Error (MAPE)**: The average of the absolute percentage errors between the predicted and actual values.

$$ MAPE = \frac{1}{n} \sum_{i=1}^{n} \left| \frac{y_i - \hat{y}_i}{y_i} \right| \times 100\% $$

</div>

<!--s-->

<div class="header-slide">

# Wrapping Up

</div>

<!--s-->

## Summary | What We Covered

<div style="font-size: 0.75em;">

| Term | Description |
|------|-------------|
| **Seasonal Decomposition** | A technique used to separate a time series into its trend, seasonal, and residual components. |
| **Stationarity** | A time series is said to be stationary if its basic properties do not change over time. |
| **Differencing** | A technique used to make a time series stationary by computing the difference between consecutive observations. |
| **Autocorrelation** | A measure of the correlation between a time series and a lagged version of itself. Partial autocorrelation controls for the values of the time series at all shorter lags. |
| **ARIMA** | A type of time series model that combines autoregressive, moving average, and differencing components. |
| **Walk-Forward Validation** | A method for evaluating the generalization performance of a time series model. |
| **Evaluation Metrics** | Regression metrics used to evaluate the performance of a time series model. |
</div>

<!--s-->

## Summary | What We Didn't Cover

<div style="font-size: 0.75em;">

| Topic | Description |
|-------|-------------|
| **Seasonal ARIMA** | An [extension of ARIMA](https://www.statsmodels.org/dev/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html) that includes seasonal components. |
| **SARIMAX** | An [extension of ARIMA](https://www.statsmodels.org/dev/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html) that includes seasonal components and exogenous variables. |
| **Box-Jenkins Approach** | A [systematic method](https://en.wikipedia.org/wiki/Box%E2%80%93Jenkins_method) for identifying, estimating, and diagnosing ARIMA models. |
| **Maximum Likelihood Estimation** | A [method](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation) for estimating the parameters of a statistical model by maximizing the likelihood function. |
| **Prophet** | A [forecasting tool](https://facebook.github.io/prophet/) developed by Facebook that is designed for forecasting time series data with strong seasonal patterns. |
| **SOTA Models** | Models like [TiDE](https://arxiv.org/abs/2304.08424) and [TSMixer](https://arxiv.org/abs/2303.06053) are state-of-the-art models for time series forecasting (2023+). |

</div>
<!--s-->

<div class = "col-wrapper">
  <div class="c1 col-centered">
    <div style="font-size: 0.8em; left: 0; width: 60%; position: absolute;">

# Exit Poll
## On a scale of 1-5, how confident are you with the **time series** methods such as:

1. Seasonal Decomposition
2. Stationarity & Differencing
3. Autocorrelation
4. ARIMA

  </div>
  </div>
  <div class="c2" style="width: 50%; height: 100%;">
  <iframe src="https://drc-cs-9a3f6.firebaseapp.com/?label=Exit Poll" width="100%" height="100%" style="border-radius: 10px"></iframe>
  </div>

</div>

<!--s-->