<h1 align="center"> 🚦Forecasting Traffic Volume🚦 </h1>

This project forecasts daily traffic volumes for New South Head Road in Sydney, using historical traffic data from January 2012 to January 2025. The goal is to model and predict traffic flow using machine learning while accounting for holidays, weather, and irregular events like road closures or festivals.

## Project Motivation
Understanding traffic patterns is critical for transport agencies to plan road infrastructure, manage congestion, and improve road safety. Recent advancements in machine learning, including deep learning techniques like Graph Neural Networks (GNNs) and Transformer-based models, have significantly improved traffic prediction capabilities by capturing complex spatial-temporal patterns. While these advanced models offer high accuracy, they often come with increased computational demands. To balance performance and practicality, this project explores the effectiveness of simpler, yet powerful, machine learning models for traffic volume prediction:
- **Random Forest Regressor**
- **XGBoost (with hyperparameter tuning)**
- **LightGBM**
  
 By applying these models to real-world traffic data, the project aims to gain practical insights into their applicability in traffic forecasting scenarios.

## Data Collection
### 🚗Traffic Data

Traffic data was sourced from the NSW Traffic Volume Viewer, focusing on New South Head Road (Westbound direction) to monitor traffic entering the city. The data spans from 01/01/2012 to 31/01/2025, filtered for light vehicles only. Each day contains 24 hourly traffic volume columns, representing the total number of vehicles passing through each hour.

The dataset also includes indicators for public holidays and school holidays, where:
- 1 denotes a holiday.
- 0 indicates a regular day.

### 🌦️Weather Data

Weather data was collected using the Open-Meteo API, retrieving daily values for:
- Temperature (°C)
- Rainfall (mm)
- Cloud cover (%)
- Wind speed (km/h)

These weather features were merged with the traffic data by date to analyze how weather conditions influence traffic patterns.

## Exploratory Data Analysis (EDA)
This section explores the dataset to uncover patterns, trends, and potential relationships between variables. Before conducting EDA, several key data preparation steps were carried out to ensure the dataset was clean and consistent:

- **Datetime processing**: Converted the date column to datetime format and set it as the index.
- **Handling missing data**: Interpolated missing hourly traffic values across 24 columns to fill in gaps.
- **Traffic volume aggregation**: Created a new 'traffic_volume' column by summing hourly counts for each day.
- **Row filtering**: Dropped any rows where total daily volume was still missing.
- **Feature engineering**:
   - Extracted day_of_week and month from the date.
   - Applied **cyclical encoding** to 'day_of_week' using sine and cosine transformations to help the models understand the proximity between the days (e.g., Monday is closer to Tuesday than to Friday).
- **Column cleanup**: The holiday indicators were standardized by renaming them to 'is_public_holiday' and 'is_school_holiday'.




  

