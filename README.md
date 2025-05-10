<h1 align="center"> 🚦Forecasting Daily Traffic Volumes </h1>

This project aims to forecast **daily traffic volume** on State Highway 1 (SH1) at the **Silverdale Interchange (Southbound)** in Auckland, New Zealand. The goal is to understand how past traffic patterns, weather conditions, and public holidays affect daily vehicle flows, and use that information to build accurate predictive models.

## 🚗Project Motivation
Understanding traffic patterns is critical for transport agencies to plan road infrastructure, manage congestion, and improve road safety. This project was inspired by Waka Kotahi NZ Transport Agency's focus on evidence-based planning and the growing need for data-driven traffic forecasting that adapts to seasonal, behavioral, and environmental factors.

## 📂Data Collection
- **Traffic Data**  
  From NZTA's TMS system — daily vehicle counts for SH1 Silverdale Interchange SB (2018–2022)

- **Weather Data**  
  Collected from Open-Meteo API which includes temperature, rainfall, and cloud cover using latitude: -36.721472 and longitude: 174.713806

- **Public Holiday Data**  
  From the `holidays` Python package, capturing official NZ public holidays

  

