<h1 align="center"> 🚦Forecasting Traffic Volume </h1>

This project forecasts daily traffic volumes for New South Head Road in Sydney, using historical traffic data from January 2012 to January 2025. The goal is to model and predict traffic flow using machine learning while accounting for holidays, weather, and irregular events like road closures or festivals.

## 🚗Project Motivation
Understanding traffic patterns is critical for transport agencies to plan road infrastructure, manage congestion, and improve road safety. Recent advancements in machine learning, including deep learning techniques like Graph Neural Networks (GNNs) and Transformer-based models, have significantly improved traffic prediction capabilities by capturing complex spatial-temporal patterns. While these advanced models offer high accuracy, they often come with increased computational demands. To balance performance and practicality, this project explores the effectiveness of simpler, yet powerful, machine learning models for traffic volume prediction:
- **Random Forest Regressor**
- **XGBoost (with hyperparameter tuning)**
- **LightGBM**
  
 By applying these models to real-world traffic data, the project aims to gain practical insights into their applicability in traffic forecasting scenarios.

## 📂Data Collection
### Traffic Data

Traffic data was sourced from the NSW Traffic Volume Viewer, focusing on New South Head Road (Westbound direction) to monitor traffic entering the city. The data spans from 01/01/2012 to 31/01/2025, filtered for light vehicles only. Each day contains 24 hourly traffic volume columns, representing the total number of vehicles passing through each hour.

The dataset also includes indicators for public holidays and school holidays, where:
- 1 denotes a holiday.
- 0 indicates a regular day.

### Weather Data

Weather data was collected using the Open-Meteo API, retrieving daily values for:
- Temperature (°C)
- Rainfall (mm)
- Cloud cover (%)
- Wind speed (km/h)

These weather features were merged with the traffic data by date to analyze how weather conditions influence traffic patterns.



  

