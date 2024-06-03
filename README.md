# INTRODUCTION
In this project, we aim to provide actionable insights for a business stakeholder looking to mitigate risks in a new business venture. By leveraging skills in data cleaning, imputation, analysis, and visualization, we will generate data-driven insights to guide decision-making in the aviation industry. The goal is to determine which aircraft present the lowest risk for purchase and operation, thereby supporting the company's strategic diversification efforts.
# BUSINESS PROBLEM ;
As part of its diversification strategy, the company is exploring new industries to expand its portfolio. Currently, the focus is on entering the aviation industry, specifically in purchasing and operating aircraft for both commercial and private enterprises. The critical business problem is to identify the aircraft that pose the lowest risk, enabling the company to make informed decisions in this new market segment.
## Main Objective
Which aircraft present the lowest risk for purchase and operation, thereby supporting the company's strategic diversification efforts?

## Specific Objectives
Which Aircraft is less vulnerable to accidents?
Which Aircraft to be used for commercial enterprise and which to be used for private enterprises?
## Data Understanding

### Dataset Overview
The dataset used in this project is sourced from Kaggle, provided by the National Transportation Safety Board (NTSB). It contains aviation accident data spanning from 1962 to 2023.
### Data & Libraries Importation
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import numpy as np
## Loading the data
To import the dataset from the CSV file and store it in a variable df
## Data Wrangling
df.shape
(88889, 31)
The dataset has 88,889 records and 31 columns.



### Relevant Features
- **EventId**: Unique identifier for each accident.
- **EventDate**: Date of the accident.
- **AircraftDamage**: Description of aircraft damage (e.g., Substantial, Minor).
- **AircraftCategory**: Category of aircraft involved.
- **Make**: Manufacturer of the aircraft.
- **Model**: Model of the aircraft.
- **NumberOfEngines**: Number of engines on the aircraft.
- **EngineType**: Type of engine(s) on the aircraft.
- **PurposeOfFlight**: Purpose of the flight (e.g., Personal, Commercial).
- **WeatherCondition**: Weather condition at the time of the accident (e.g., VMC, IMC).
- **BroadPhaseOfFlight**: Broad phase of flight (e.g., Takeoff, Landing).
- **ReportStatus**: Status of the report (e.g., Factual, Probable Cause).


