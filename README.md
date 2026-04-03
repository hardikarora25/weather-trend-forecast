# 🌍 Weather Trend Forecasting - Data Science Assessment

## 📋 Project Overview
This project analyzes the **Global Weather Repository** dataset to forecast weather trends and uncover climate patterns across 211 countries worldwide. It includes data cleaning, exploratory data analysis (EDA), anomaly detection, multiple forecasting models, and geographical visualizations.

## 🎯 Objectives
- Clean and preprocess weather data (133K+ records, 41 features)
- Perform advanced EDA with anomaly detection
- Build and compare multiple forecasting models (ARIMA, SARIMA, Ensemble, Random Forest)
- Analyze climate patterns across continents and regions
- Visualize geographical weather patterns interactively

## 📊 Dataset
- **Source**: [Global Weather Repository (Kaggle)](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository)
- **Size**: 133,318 rows × 41 columns
- **Time Range**: May 2024 – April 2026 (687 unique dates)
- **Coverage**: 211 countries across 6 continents
- **Features**: Temperature, humidity, precipitation, wind, pressure, UV index, air quality (CO, O₃, NO₂, SO₂, PM2.5, PM10), moon phases

## 📁 Project Structure
Data_Science_assessment/
├── data/
│ └── Global Weather Repository.csv
├── notebooks/
│ └── weather_analysis.ipynb # Main analysis notebook
├── outputs/
│ └── temperature_map.html # Interactive Folium map
├── requirements.txt
└── README.md


## 🔧 How to Run

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd Data_Science_assessment
2. Create Environment
conda create -n weather_forecast python=3.11 -y
conda activate weather_forecast
pip install -r requirements.txt
3. Run the Notebook
jupyter notebook notebooks/weather_analysis.ipynb
📈 Key Findings
Model Performance
Model	MAE	RMSE	R²
Naive Baseline	3.65	4.89	-0.65
ARIMA	3.14	4.26	-0.25
SARIMA	3.01	4.08	-0.15
Random Forest	1.17	1.48	0.98 ✅
Top Insights
Random Forest with lag features outperforms all time-series models (R² = 0.98)
temp_lag_1 (yesterday’s temperature) is the strongest predictor (68% feature importance)
Europe has the lowest avg temperature (13°C), Africa the highest (25°C)
Asia shows the most extreme seasonal variation (2.4°C winter → 32°C summer)
Temperature-Humidity correlation: -0.34 (inverse relationship)
Air Quality: Ozone has positive correlation with temperature (0.26), others are weak
Partly Cloudy & Sunny conditions dominate ~60% of all observations
️ Interactive Map
Open outputs/temperature_map.html in a browser for an interactive global temperature visualization.

📦 Dependencies
See requirements.txt for full list:

pandas, numpy, matplotlib, seaborn
scikit-learn, statsmodels
xgboost, lightgbm
plotly, folium
scipy, jupyter
👤 Author
Built for PM Accelerator Data Science Assessment.



