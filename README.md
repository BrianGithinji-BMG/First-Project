# [INTRODUCTION]
In this project, we aim to provide actionable insights for a business stakeholder looking to mitigate risks in a new business venture. By leveraging skills in data cleaning, imputation, analysis, and visualization, we will generate data-driven insights to guide decision-making in the aviation industry. The goal is to determine which aircraft present the lowest risk for purchase and operation, thereby supporting the company's strategic diversification efforts.
# BUSINESS PROBLEM ;
As part of its diversification strategy, the company is exploring new industries to expand its portfolio. Currently, the focus is on entering the aviation industry, specifically in purchasing and operating aircraft for both commercial and private enterprises. The critical business problem is to identify the aircraft that pose the lowest risk, enabling the company to make informed decisions in this new market segment.
## Main Objective
Which aircraft present the lowest risk for purchase and operation, thereby supporting the company's strategic diversification efforts?

## Specific Objectives
Which Aircraft Category , makes and models is less vulnerable to accidents?
Which Aircraft to be used for commercial enterprise and which to be used for private enterprises?
# Data Understanding

### Dataset Overview
The dataset used in this project is sourced from Kaggle, provided by the National Transportation Safety Board (NTSB). It contains aviation accident data spanning from 1962 to 2023.
### Data & Libraries Importation

```
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import numpy as np
```

### Loading the data
To import the dataset from the CSV file and store it in a variable df
> Importing the aviation data
```
df = pd.read_csv('C:/Users/ADMIN/Documents/Moringa/Phase1/Project1/First-Project/Aviation Data/AviationData.csv', encoding='ISO-8859-1', low_memory=False)
df.head()
```
>Importing the US state codes
```
df_code = pd.read_csv('Aviation Data/USState_Codes.csv')
df_code.head()
```
5.0#00000	699.000000`2022.000`000	3.000000
## Data Wrangling
>df.shape
(88889, 31)

The da
This are the columns in our dataset:
>'Event.Id', 'Investigation.Type', 'Accident.Number', 'Event.Date',
       'Location', 'Country', 'Latitude', 'Longitude', 'Airport.Code',
       'Airport.Name', 'Injury.Severity', 'Aircraft.damage',
       'Aircraft.Category', 'Registration.Number', 'Make', 'Model',
       'Amateur.Built', 'Number.of.Engines', 'Engine.Type', 'FAR.Description',
       'Schedule', 'Purpose.of.flight', 'Air.carrier', 'Total.Fatal.Injuries',
       'Total.Serious.Injuries', 'Total.Minor.Injuries', 'Total.Uninjured',
       'Weather.Condition', 'Broad.phase.of.flight', 'Report.Status',
       'Publication.Date',taset has 88,889 record and columns to use.
- **Event.id**: Unique identifier for each event.s and 31 columns.

### Relevant Features
- **RegistrationNumber
- **Location**: Unique identifier of a place.
- **Country**: Shows the nations.(e.g., United States)**: Unique identifier for each airplane
- **Injury.Severity**: To determine the severity of the accident.(e.g., Incident,Fatal).
- **EventDate**: Date of the accident.
- **Aircraft.Damage**: Description of aircraft damage (e.g., Substantial, Minor).
- **AircraftCategory**: Category of aircraft involved.
- **Make**: Manufacturer of the aircraft.
- **Model**: Model of the aircraft.
- **NumberOfEngines**: Number of engines on the aircraft.
- **EngineType**: Type of engine(s) on the aircraft.
- **PurposeOfFlight**: Purpose of 
- **Total.Fatal.Injuries**: N.o of fatal accidents.
- **Total.Serious.Injuries**: N.o of serious injuries.
- **Total.Minor.Injuries**: N.o of Injuries that are minor.
- **Total.Uninjured**: N.o of uninjured people.the flight (e.g., Personal, Commercial).
- **WeatherCondition**: Weather condition at the time of the accident (e.g., VMC, IMC).
- **BroadPhaseOfFlight**: Broad ph .# 2. DATA CLEANING
As seen above the following feaures are of importance and therefore the rest of the columns that are in our dataset are irrelevant hence need to be dropped.
>Dropping of unnecessary columns for our analysis
```
df.drop(labels = ['Investigation.Type','Accident.Number','Latitude', 'Longitude', 'Airport.Code',
                  'Airport.Name', 'Amateur.Built','FAR.Description','Air.carrier', 'Publication.Date','Report.Status',
                  'Schedule',],inplace = True , axis = 1)
```
>Checking the general overview of our data

`df.info()`
Data columns (total 19 columns):
- #   Column                  Non-Null Count  Dtype  
---  ------                  --------------  -----  
- 0   Event.Id                88889 non-null  object 
- 1   Event.Date              88889 non-null  object 
- 2   Location                88837 non-null  object 
- 3   Country                 88663 non-null  object 
- 4   Injury.Severity         87889 non-null  object 
- 5   Aircraft.damage         85695 non-null  object 
- 6   Aircraft.Category       32287 non-null  object 
- 7   Registration.Number     87507 non-null  object 
- 8   Make                    88826 non-null  object 
- 9   Model                   88797 non-null  object 
- 10  Number.of.Engines       82805 non-null  float64
- 11  Engine.Type             81793 non-null  object 
- 12  Purpose.of.flight       82697 non-null  object 
- 13  Total.Fatal.Injuries    77488 non-null  float64
- 14  Total.Serious.Injuries  76379 non-null  float64
- 15  Total.Minor.Injuries    76956 non-null  float64
- 16  Total.Uninjured         82977 non-null  float64
- 17  Weather.Condition       84397 non-null  object 
- 18  Broad.phase.of.flight   61724 non-null  object 
dtypes: float64(5), object(14)

### Key Takeaways
1.We note that in our data the data type for Event.Date is an object instead of date.

2.There are several missing values in some columns

# 3. DATA COMPLETENESS
### Missing Values
First is to identify the unique values in each column.
Afterwards identify the number of missing values per column using;

`df.isna().sum()`

Deal with the *missing values* in each column;
* For *Categorical columns* we shall use *unknown* to fill in the values for the missing values.
* For *Numerical columns* we shall use the *mean* to fill in the missing values.

## 3.1 Data Validity
This done by making the data valid by converting *Event.Date* to *datetime* format.

## 3.2 Data Uniformity
This done by making the data uniform. One of the ways is standardize categorical columns and make the in a title format.
For numerical columns the *Number.Of.Engines* is converted from a float to an integer.

## 3.2 Data Consistency
This is done by checking the duplicated rows and droping duplicates if they exist.

### NOTE
- United States is overrepresented in Country column more than 90% of the Country column is the United States. As the focus is on the US aviation accidents       create a new dataframe `df_USA` to focus on the United States.
- We may want to create a column for year alone to help in future analysis.
- We may want to categorise the purpose of flight as either 'Private Enterprise' or 'Commercial'

# Summary of cleaned dataframe
`df_USA.info()`

> Data columns (total 21 columns):
- #   Column                  Non-Null Count  Dtype  
---  ------                  --------------  -----  
- 0   Event.Id                82248 non-null  object 
- 1   Country                 82248 non-null  object 
- 2   Injury.Severity         82248 non-null  object 
- 3   Aircraft.damage         82248 non-null  object 
- 4   Aircraft.Category       82248 non-null  object 
- 5   Registration.Number     82248 non-null  object 
- 6   Make                    82248 non-null  object 
- 7   Model                   82248 non-null  object 
- 8   Number.of.Engines       82248 non-null  int32  
- 9   Engine.Type             82248 non-null  object 
- 10  Total.Fatal.Injuries    82248 non-null  float64
- 11  Total.Serious.Injuries  82248 non-null  float64
- 12  Total.Minor.Injuries    82248 non-null  float64
- 13  Total.Uninjured         82248 non-null  float64
- 14  Weather.Condition       82248 non-null  object 
- 15  Broad.phase.of.flight   82248 non-null  object 
- 16  City                    82248 non-null  object 
- 17  State                   82248 non-null  object 
- 18  Year                    82248 non-null  int32  
- 19  Flight.Category         82248 non-null  float64
- 20  Aircraft.Type           82248 non-null  object 
dtypes: float64(5), int32(2), object(14)



# [Exploratory Data Analysis]
## 4.1 Univariate Analysis
### : Distribution of Aircraft Damage
Understanding the distribution of aircraft damage helps to identify the most common types of damage and potential risk areas.


