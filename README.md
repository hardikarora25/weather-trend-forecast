# 🌍 Weather Trend Forecasting — Global Climate Analysis & Prediction

> **Data Science Assessment | PM Accelerator**  
> Empowering the next generation of product managers through hands-on data science and analytics training. Building real-world skills to drive data-informed product decisions at scale.

---

## 📋 Table of Contents
1. [Project Overview](#1-project-overview)
2. [Dataset](#2-dataset)
3. [Project Structure](#3-project-structure)
4. [Methodology](#4-methodology)
5. [Results & Key Findings](#5-results--key-findings)
6. [How to Run](#6-how-to-run)
7. [Dependencies](#7-dependencies)
8. [Author](#8-author)

---

## 1. Project Overview

This project performs a comprehensive analysis of the **Global Weather Repository** dataset — containing over **133,000 daily weather records** across **211 countries** — to uncover climate patterns, detect anomalies, and build predictive models for weather forecasting.

### Objectives
- **Clean & preprocess** multi-dimensional weather data (41 features)
- **Identify outliers and anomalies** using IQR-based detection
- **Explore global climate patterns** across continents, seasons, and regions
- **Build and compare multiple forecasting models** (ARIMA, SARIMA, Ensemble, Random Forest)
- **Visualize geographical weather distributions** interactively
- **Assess feature importance** to understand key drivers of temperature variation

### Assessment Level: Advanced
This project fulfills all **Advanced Assessment** requirements:
- ✅ Advanced EDA with anomaly detection
- ✅ Multiple forecasting models with ensemble approach
- ✅ Climate analysis across regions
- ✅ Environmental impact (air quality) analysis
- ✅ Feature importance analysis
- ✅ Spatial & geographical pattern visualization

---

## 2. Dataset

| Attribute | Details |
|---|---|
| **Source** | [Global Weather Repository (Kaggle)](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository) |
| **Records** | 133,318 rows × 41 columns |
| **Time Range** | May 16, 2024 — April 3, 2026 (687 unique dates) |
| **Geographic Coverage** | 211 countries across 6 continents |
| **Update Frequency** | Daily per city |

### Feature Categories

| Category | Features |
|---|---|
| **Location** | country, location_name, latitude, longitude, timezone |
| **Temperature** | temperature_celsius, temperature_fahrenheit, feels_like_celsius, feels_like_fahrenheit |
| **Wind** | wind_mph, wind_kph, wind_degree, wind_direction, gust_mph, gust_kph |
| **Precipitation** | precip_mm, precip_in |
| **Atmospheric** | pressure_mb, pressure_in, humidity, cloud, visibility_km, visibility_miles |
| **UV & Radiation** | uv_index |
| **Air Quality** | CO, Ozone, NO₂, SO₂, PM2.5, PM10, US-EPA Index, GB-DEFRA Index |
| **Astronomical** | sunrise, sunset, moonrise, moonset, moon_phase, moon_illumination |
| **Temporal** | last_updated_epoch, last_updated |
| **Descriptive** | condition_text |

### Data Quality
- **Missing Values**: 0% — dataset is complete
- **Negative Air Quality Values**: 4 records fixed (clipped to 0)
- **Outliers Detected**: ~30,000 across temperature, wind, pressure (handled via IQR clipping)
- **Duplicates**: 0


## 3. Project Structure
---
```
Data_Science_assessment/
├── data/
│ └── Global Weather Repository.csv # Source dataset (not committed to Git)
├── notebooks/
│ └── weather_analysis.ipynb # Main analysis notebook (all code + visuals)
├── outputs/
│ └── temperature_map.html # Interactive Folium world map
├── requirements.txt # Python dependencies
├── .gitignore # Ignores CSV, cache, venv
└── README.md # This file
```

---

## 4. Methodology

### 4.1 Data Preprocessing
1. **Type Conversion**: Converted `last_updated` string to `datetime` for time-series analysis
2. **Anomaly Handling**: Replaced 4 negative air quality values with 0 (physically impossible)
3. **Outlier Clipping**: Capped wind speed (99th percentile) and pressure (1st–99th percentile) to remove extreme sensor errors
4. **Feature Engineering**: Created temporal features — year, month, day, hour, day_of_week, is_weekend
5. **Aggregation**: Grouped daily per-country averages for time-series modeling (126,479 rows)

### 4.2 Exploratory Data Analysis (EDA)
- **Distribution Analysis**: Histograms for temperature, humidity, precipitation, wind, pressure, UV index
- **Correlation Analysis**: Heatmap of 41 features to identify multicollinearity and relationships
- **Geographic Analysis**: Latitude-longitude scatter with temperature colormap
- **Temporal Patterns**: Monthly temperature curves, seasonal trends
- **Air Quality Correlation**: Scatter plots of pollutants vs temperature

### 4.3 Anomaly Detection
- **Method**: IQR-based outlier detection (1.5× IQR rule)
- **Findings**:
  - Temperature: 2,437 outliers (range: -2°C to 46°C normal)
  - Pressure: 4,082 outliers (range: 998–1030 mb normal)
  - Wind Speed: 2,320 outliers (extreme gusts up to 3000+ kph — sensor errors)
  - Precipitation: 24,780 "outliers" (expected — most days have 0 rain)

### 4.4 Forecasting Models

#### Dataset Split
- **Country**: Afghanistan (685 daily records)
- **Train**: 655 days (May 2024 – Mar 2026)
- **Test**: 30 days (Mar 2026 – Apr 2026)
- **Target Variable**: avg_temp (daily mean temperature)

#### Models Built

| Model | Description | Key Parameters |
|---|---|---|
| **Naive Baseline** | Last observed value repeated | — |
| **ARIMA** | Autoregressive Integrated Moving Average | order=(7,1,0) |
| **SARIMA** | Seasonal ARIMA (weekly seasonality) | order=(1,1,1), seasonal=(1,1,1,7) |
| **Ensemble** | Average of ARIMA + SARIMA forecasts | — |
| **Random Forest** | Tree-based regression with lag features | 100 trees, max_depth=10 |

#### Feature Engineering for ML
- **Lag Features**: temp_lag_1, temp_lag_2, temp_lag_3, temp_lag_7, temp_lag_14, temp_lag_30
- **Rolling Statistics**: 7-day rolling mean, 7-day rolling std
- **Weather Features**: humidity, precipitation, wind, pressure, PM2.5, Ozone

### 4.5 Climate & Geographical Analysis
- **Continent Mapping**: 211 countries mapped to 6 continents
- **Monthly Aggregation**: Continent-wise monthly mean temperatures
- **Precipitation Analysis**: Total and average rainfall by continent
- **Temperature-Humidity Relationship**: Correlation analysis

### 4.6 Feature Importance
- **Method**: Random Forest permutation importance
- **Features**: 14 engineered features including lags, rolling stats, and weather variables

---

## 5. Results & Key Findings

### 5.1 Model Performance

| Model | MAE | RMSE | R² Score |
|-------|-----|------|----------|
| Naive Baseline | 3.65 | 4.89 | -0.65 |
| ARIMA (7,1,0) | 3.14 | 4.26 | -0.25 |
| SARIMA (1,1,1)×7 | 3.01 | 4.08 | -0.15 |
| Ensemble (ARIMA+SARIMA) | 2.98 | 4.05 | -0.12 |
| **Random Forest** | **1.17** | **1.48** | **0.98** ✅ |

**Key Takeaway**: Random Forest with lag features outperforms all time-series models by a massive margin (R² = 0.98 vs ~0 for ARIMA family). This indicates that **temperature is highly autocorrelated** and best predicted using recent history rather than pure time-series decomposition.

### 5.2 Feature Importance (Random Forest)

| Rank | Feature | Importance |
|------|---------|------------|
| 1 | **temp_lag_1** (yesterday's temp) | 67.5% |
| 2 | **temp_rolling_mean_7** (7-day avg) | 29.5% |
| 3 | avg_humidity | 1.2% |
| 4 | avg_pressure | 0.8% |
| 5+ | Others | <1% |

**Insight**: The last 2 features account for **97%** of predictive power. Temperature is overwhelmingly driven by recent history.

### 5.3 Climate Analysis

#### Average Temperature by Continent
| Continent | Mean (°C) | Std | Min | Max |
|---|---|---|---|---|
| Africa | 25.13 | 5.00 | 1.4 | 47.1 |
| Asia | 23.87 | 10.97 | -29.8 | 49.2 |
| North America | 22.48 | 6.44 | -25.0 | 35.3 |
| Oceania | 24.96 | 5.49 | -3.7 | 39.1 |
| South America | 18.52 | 6.30 | -1.7 | 33.3 |
| Europe | 13.17 | 10.19 | -26.9 | 41.4 |

**Insight**: Europe has the coldest average temperature and highest variability (due to northern latitude countries). Asia shows the widest range (-29.8°C to 49.2°C).

#### Seasonal Patterns
- **Asia**: Most extreme seasonality — ~2°C in January to ~32°C in July
- **Europe**: Cold winters (~2°C), warm summers (~25°C)
- **Africa**: Relatively stable year-round (~24-26°C)
- **Oceania**: Inverse pattern (Southern Hemisphere) — warmest in Jan, coolest in Jul
- **North America**: Moderate seasonality (~20°C to ~25°C)

### 5.4 Precipitation Analysis

| Continent | Avg Daily (mm) | Total (mm) | Max Single Day (mm) |
|---|---|---|---|
| Oceania | 0.29 | 2,587 | 26.2 |
| Asia | 0.16 | 4,348 | 32.4 |
| Unknown | 0.20 | 1,409 | 17.3 |
| South America | 0.14 | 1,052 | 11.0 |
| Africa | 0.12 | 3,830 | 27.4 |
| North America | 0.12 | 1,679 | 42.2 |
| Europe | 0.11 | 3,097 | 18.9 |

**Peak Rainy Month**: July (0.175 mm average across all locations)

### 5.5 Air Quality & Environmental Impact

| Pollutant | Correlation with Temperature |
|---|---|
| Ozone (O₃) | **+0.259** (positive — increases in heat) |
| PM10 | +0.107 |
| PM2.5 | +0.053 |
| Sulphur Dioxide | -0.036 |
| Carbon Monoxide | -0.005 |
| Nitrogen Dioxide | -0.143 |

**Key Finding**: Ozone levels increase with temperature (photochemical reaction), while NO₂ decreases (winter heating emissions).

### 5.6 Temperature-Humidity Relationship
- **Correlation**: -0.343 (moderate inverse relationship)
- Warm air holds more moisture, but high temperatures often occur in dry conditions (deserts, summer days)

---

## 6. How to Run

### Prerequisites
- Python 3.11
- Conda (recommended) or pip

### Installation

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/weather-trend-forecast.git
cd weather-trend-forecast

# Create conda environment
conda create -n weather_forecast python=3.11 -y
conda activate weather_forecast

# Install dependencies
pip install -r requirements.txt
Download Dataset
# Download from Kaggle and place in data/ folder
# https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository
Run Analysis
# Launch Jupyter Notebook
jupyter notebook notebooks/weather_analysis.ipynb

# Run all cells (Cell → Run All)
View Interactive Map
Open outputs/temperature_map.html in any web browser for an interactive global temperature visualization.

7. Dependencies
Category	Packages
Data Processing	pandas, numpy, scipy, pyarrow, openpyxl
Visualization	matplotlib, seaborn, plotly, plotly-express, folium
Machine Learning	scikit-learn, statsmodels
Notebook	jupyter, notebook
See requirements.txt for exact versions.

8. Author
Built for PM Accelerator Data Science Assessment.





