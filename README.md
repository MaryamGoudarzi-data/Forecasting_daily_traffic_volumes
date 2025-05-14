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

**Figure 1** shows traffic volume by hour of the day. Traffic rises sharply from 5 AM, peaking between 7–9 AM during the morning commute, and again between 3–6 PM in the evening rush hour. Volumes are lowest between 12–4 AM, with steady declines after 6 PM. Wider boxes during peak hours indicate greater variability in traffic during commuting times.


**Figure 2** illustrates daily traffic volume from January 2012 to January 2025. While overall patterns remain stable, several key fluctuations stand out:
- 2014–2015: Data is missing from January to June in both years. Interpolation was used for continuity, but trends during these gaps may be less reliable.
- Late 2014: Noticeable drops in November–December 2014 reflect disruptions from events such as the Sydney Lindt Cafe hostage crises on December 15.
- 2020–2021: Sharp declines align with COVID-19 lockdowns, which significantly reduced traffic.
- Post-2023: A gradual decline may reflect people moving away from Sydney due to rising living costs and housing affordability pressures.

**Figure 3** displays the average monthly traffic volume. Traffic tends to peak in February, March, May, and November, while January, July, and December show the lowest volumes. These dips likely correspond to school holidays, summer vacations, and seasonal factors such as winter weather or major public holidays.

**Figure 4** shows the average traffic volume by day of the week. Traffic increases steadily from Monday to Friday, peaking on Friday, likely due to early weekend getaways as people head out for long weekends. Saturday maintains high volumes, likely driven by recreational activities and shopping, while Sunday sees the lowest traffic, as people tend to stay home and relax

**Figure 5** compares the average traffic volume on public holidays (1) versus non-holidays (0). The chart shows that traffic volume is significantly lower on public holidays. This pattern highlights the importance of including holiday indicators as features in forecasting models.

**Figure 6** displays scatter plots exploring how weather conditions may influence traffic volume:
- **Traffic vs Rain**: Most datapoints cluster below 20 mm of rain, heavier rain may reduce traffic.
- **Traffic vs Temperature**: Traffic remains stable across temperature ranges, showing minimal impact.
- **Traffic vs Cloud Cover**: Even distribution suggests cloudiness has little effect on traffic.
- **Traffic vs Wind**: The scatter is dense and consistent across the wind range, suggesting minimal impact.
