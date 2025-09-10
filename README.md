# Cloud & Local Data Pipeline Project

This project demonstrates a hybrid data pipeline that automates data collection, processing, and storage using **Python**, **SQL**, and **Google Cloud Services**. It handles both static and dynamic data including city info, weather forecasts, and flight schedules.

## Table of Contents

- [Project Overview](#project-overview)  
- [Project Structure](#project-structure)  
- [Local Pipeline](#local-pipeline)  
- [Cloud Pipeline](#cloud-pipeline)  
- [Requirements](#requirements)  
- [Setup & Usage](#setup--usage)  
- [Database Schema](#database-schema)  
- [Further Reading](#further-reading)  

---

## Project Overview

In modern data engineering, automation is essential. This project:  

- Scrapes static data (city info, population, coordinates).  
- Fetches dynamic data from APIs (**OpenWeather** and **Aerodatabox**) for weather and flight information.  
- Stores data locally in a **MySQL database**.  
- Pushes processed data to **Google Cloud SQL** using a cloud pipeline for automated daily updates.

---

## Project Structure

```
project/
│
├── src/
│   ├── weather_api.py       # Fetch weather data
│   ├── flights_api.py       # Fetch flight data
│   ├── local_pipeline.py    # Push data to local SQL database
│   └── cloud_pipeline.py    # Cloud Function to push data to Cloud SQL
│
└── sql/
    └── schema.sql           # SQL scripts to create database & tables
```

---

## Local Pipeline

The local pipeline uses Python scripts to fetch data and store it in a local MySQL instance.

Example usage:

```python
from src.local_pipeline import data_to_sql
from src.weather_api import weather_df
from src.flights_api import departures_df, arrivals_df

# Push data to local database
data_to_sql(weather_df, departures_df, arrivals_df)
```

---

## Cloud Pipeline

The cloud pipeline deploys a **Python Cloud Function** that automatically updates a **Google Cloud SQL** database.

Example usage locally (for testing):

```python
from src.cloud_pipeline import retrieve_send_data

# Trigger cloud function locally
retrieve_send_data(None)
```

The cloud function workflow:

1. Connects to the Cloud SQL database  
2. Fetches cities and airport info  
3. Calls weather and flight APIs  
4. Processes the data into DataFrames  
5. Inserts results into Cloud SQL  



## Requirements

- Python 3.9+  
- `pandas`  
- `requests`  
- `sqlalchemy`  
- `pymysql`  
- Google Cloud SDK  

```bash
pip install pandas requests sqlalchemy pymysql
```

---

## Setup & Usage

1. Create a local MySQL database using `sql/schema.sql`.  
2. Configure database credentials in `local_pipeline.py` and `cloud_pipeline.py`.  
3. Run `weather_api.py` and `flights_api.py` to fetch data.  
4. Use `local_pipeline.py` to store data locally.  
5. Deploy `cloud_pipeline.py` as a **Google Cloud Function** connected to Cloud SQL.  
6. Schedule automatic updates using **Google Cloud Scheduler**.

---

## Database Schema

The schema includes:  

- `city_df` – City names  
- `population_df` – City populations  
- `coordinates_df` – City coordinates and country  
- `weather_df` – Weather data  
- `airports` – Airport info  
- `flight_departures` & `flight_arrivals` – Flight schedules  

Full schema in `sql/schema.sql`.

---

## Further Reading

Full documentation and project explanation are available in my [Medium article](https://medium.com/@minwoobfd/code-connect-automate-crafting-data-pipelines-for-the-cloud-and-beyond-7163761d831d).
