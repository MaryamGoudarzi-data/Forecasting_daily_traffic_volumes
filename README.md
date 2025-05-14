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

## Residual Analysis
To better understand the traffic variability, time series decomposition was applied to analyse the residuals (unexplained variation). The residual plot shows large deviations from the baseline (zero line), confirming spikes and drops in traffic volume. These deviations suggest unusual events such as road closures, crashes, parades, or public disruptions, factors not captured by trend or seasonality.
This insight justified creating a new binary feature **(is_unusual)** to flag such anomalous days and improve model performance on unpredictable patterns.

## Models and Methods
To forecast daily traffic volume on Sydney's New South Head Road (westbound), this study implemented and evaluated three machine learning models:
- **Random Forest Regressor**
- **XGBoost (with hyperparameter tuning)**
- **LightGBM**

The target variable was daily traffic volume, while input features included lag variables (1-day and 7-day lag), a 7-day rolling average, day of the week, month, public holiday, school holiday, weather indicators (temperature, rainfall, cloud cover, wind), and a custom feature named 'is_unusual' was created to flag days with unexpected traffic spikes or drops more than 30% deviation from a 7-day rolling average.

The data was split into two parts chronologically: 
- Training set: 80% of the data (earliest dates)
- Test set: 20% (most recent dates)

This approach allows the model to be built and refined using the training set, followed by an evaluation of its performance on the test set to ensure it generalizes well to new, unseen data. To maintain consistency across different code runs, the random_state was fixed to 42, ensuring that the data is evenly distributed each time, gauranteeing reproducibility. 

The **random forest regressor model** is a robust ensemble machine learning algorithm designed for regression tasks, especially when dealing with complex, non-linear data. It works by combining the predictions of multiple decision trees, each trained on random subsets of the dataset (a technique known as bootstrapping). The final prediction is calculated as the average of the outputs from all trees, making it less prone to overfitting compared to individual trees. In this project, this model was trained on all input features without hyperparameter tuning.

**XGBoost** short for eXtreme Gradient Boosting, is a gradient boosting framework that uses decision trees as its base learners combining them sequentially to improve the model’s performance. Each new tree is trained to correct the errors made by the previous tree (a process called boosting). It has built-in parallel processing to train models on large datasets quickly.

Hyperparameter tuning is the process of optimizing the performance of machine learning models by evaluating a range of predefined values for key parameters. For this project, I applied hyperparameter tuning to XGBoost using RandomizedSearchCV. The **n_estimators** parameter, which controls the number of trees in the model, was tested with values ranging from 100 to 500. The **max_depth** parameter, determines how deep each tree can grow, tested from 3 to 10, the deeper the tree, the more complex the model.  The learning_rate parameter controls the speed at which the model learns, tested with small values between 0.01 and 0.2 to balance learning speed and accuracy. The subsample parameter controls the fraction of observations for each tree, with values between 0.6 and 1.0 to prevent overfitting.The **colsample_bytree** specifies the fraction of features used in each tree, varied between 0.6 and 1.0 to increase model diversity.

After evaluating 20 combinations, the best performing set of parameters was used to train the final XGBoost model.

**LightGBM** is another gradient boosting framework developed by Microsoft known for its efficiency especially for large datasets. It uses leaf-wise tree growth which leads to faster training speed compared to XGBoost. Unlike XGBoost, LightGBM was not tuned and was trained with fixed parameters: n_estimators: 300, learning_rate: 0.05 and max_depth: 6. 

## Model Comparison 


| Model              | MAE       | RMSE      | Relative MAE (%)|
|-------------------|-----------|-----------|-----------------|
| Random Forest      | 1945.854  | 3926.882  |  5.787  |
| XGBoost (Tuned)    | 1901.334  | 3813.914  |  5.655  |
| LightGBM           | 1960.274  | 3978.401  |  5.787  |













In this project, XGBoost achieved lower MAE likely because:
- It was hyperparameter-tuned using cross-validation, while LightGBM was trained with default settings.
- The data was noisy, with irregular spikes due to events like road closures or holidays. XGBoost’s conservative level-wise approach and tuning likely made it better suited to handle this variability


