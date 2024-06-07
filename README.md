# [INTRODUCTION]
In this project, we aim to provide actionable insights for a business stakeholder looking to mitigate risks in a new business venture. By leveraging skills in data cleaning, imputation, analysis, and visualization, we will generate data-driven insights to guide decision-making in the aviation industry. The goal is to determine which aircraft present the lowest risk for purchase and operation, thereby supporting the company's strategic diversification efforts.
# BUSINESS PROBLEM ;
As part of its diversification strategy, the company is exploring new industries to expand its portfolio. Currently, the focus is on entering the aviation industry, specifically in purchasing and operating aircraft for both commercial and private enterprises. The critical business problem is to identify the aircraft that pose the lowest risk, enabling the company to make informed decisions in this new market segment.
## Main Objective
Which aircraft presents the lowest risk for purchase and operation, thereby supporting the company's strategic diversification efforts?

## Specific Objectives
Which Aircraft Category, makes and models is less vulnerable to accidents?
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
## Data Wrangling
>df.shape
(88889, 31)


This are the columns in our dataset:
>'Event.Id', 'Investigation.Type', 'Accident.Number', 'Event.Date',
       'Location', 'Country', 'Latitude', 'Longitude', 'Airport.Code',
       'Airport.Name', 'Injury.Severity', 'Aircraft.damage',
       'Aircraft.Category', 'Registration.Number', 'Make', 'Model',
       'Amateur.Built', 'Number.of.Engines', 'Engine.Type', 'FAR.Description',
       'Schedule', 'Purpose.of.flight', 'Air.carrier', 'Total.Fatal.Injuries',
       'Total.Serious.Injuries', 'Total.Minor.Injuries', 'Total.Uninjured',
       'Weather.Condition', 'Broad.phase.of.flight', 'Report.Status',
       'Publication.Date'
>The Dataset has 88,889 record and 31 columns.

### Relevant Features
- **Event.id**: Unique identifier for each event.
- **RegistrationNumber**: Unique identifier for each airplane.
- **Location**: Unique identifier of a place.
- **Country**: Shows the nations.(e.g., United States)**: Unique identifier for each airplane
- **Injury.Severity**: To determine the severity of the accident. (e.g., Incident, Fatal).
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
- **BroadPhaseOfFlight**: Broad phase of flight whether landing, takeoff,taxi e,t,c .

# 2. DATA CLEANING
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
- 10  Number. of.Engines       82805 non-null  float64
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
To effectively present the analysis and provide actionable insights, we'll focus on identifying aircraft makes that are less involved in accidents and have less damage, indicating they are less risky. We'll create visualizations in three stages: univariate, bivariate, and multivariate analysis. These visualizations will help us to identify patterns and trends in the data.
### : Distribution of Aircraft Damage
Understanding the distribution of aircraft damage helps to identify the most common types of damage and potential risk areas.

![Distribution of Aircraft Damage](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/2679098b-f8d9-460f-bd01-cf0407812d0e)

#### Interpretation:
- Most aircraft damages are substantial

### : Distribution of Aircraft Categories
Different categories of aircraft may have different risk profiles. Analyzing this can help in understanding which types of aircraft are more prone to accidents.

![Distribution of Aircraft Categories](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/4fb620a9-7807-4668-af6c-1d99434b38c7)

#### Interpretation:
- **Airplanes** and **Helicopters** seem to be the most type of aircraft category in our dataset. Due to this reason we shall use airplanes and helicopters for our analysis. An assumption shall be airplanes are used for commercial purposes and Helicopters for private enterprise hence dropping the rest of the categories

**TO ASCERTAIN OUR INTERPRETATION:**
### : Distribution of aircraft types and purpose
![Distribution of aircraft types and purpose](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/38b201f3-922d-4e97-8faa-de7f9a0e6798)
**To be more specific we can create a pivot table that brings out the makes that deal with airplanes and helicopters respectively.**
```
# Pivot table for accidents by aircraft make and type
pivot_accidents_make_type = pd.pivot_table(df_filtered, values='Event.Id', index='Make', columns='Aircraft.Type', aggfunc='count', fill_value=0)

# Sort the pivot table by total accidents
pivot_accidents_make_type['Total'] = pivot_accidents_make_type.sum(axis=1)
pivot_accidents_make_type = pivot_accidents_make_type.sort_values(by='Total', ascending=False)

print(pivot_accidents_make_type.head(20))
```
Aircraft.Type                Commercial  Private  Unknown  Total
Make                                                            
Cessna                             7280        0      445   7725
Piper                              4152        0      184   4336
Beech                              1414        0      114   1528
Bell                                413      147      115    675
Boeing                              139        1      318    458
Mooney                              391        0        2    393
Robinson                            217       98        8    323
Bellanca                            272        0        8    280
Grumman                             205        0       31    236
Aeronca                             226        0        2    228
Maule                               223        0        2    225
Robinson Helicopter                 160       63        2    225
Hughes                              121       71       27    219
Air Tractor Inc                     212        0        6    218
Cirrus Design Corp                  200        0        3    203
Air Tractor                         181        0        8    189
Robinson Helicopter Company          87       86        4    177
Champion                            162        0        3    165
Luscombe                            162        0        1    163
Stinson                             145        0        1    146
## Filtering Data to work with airplanes and helicopters.
```
# Load the cleaned dataset
df_cleaned = pd.read_csv('processed_aviation_data.csv')
# Filter for airplanes and helicopters
df_filtered = df_cleaned[df_cleaned['Aircraft.Category'].isin(['Airplane', 'Helicopter'])]
```
## : Distribution of Makes
![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/2ec5ecb6-f09c-4c26-be7b-bdf82c853127)

### Interpretation:
- **Cessna**, **Piper** and **Beech** seem to be the make for airplanes - 
 and helicopters that encounter major accidents and may be considered as 
   risky makes to commence our aviation expedition with.

## : Distribution of Weather Conditions
Weather conditions can significantly impact the likelihood of accidents. This analysis helps to identify the most hazardous weather conditions.

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/e99ddea3-de0d-4eb5-81a6-96a7f9e399ad)
### Interpretation:
- *Vmc* seems to be a risky weather condition to fly the aircrafts and 
   would recommend flying during *imc* weather conditions

# 4.2 Bivariate Analysis
## : Aircraft Damage vs. Broad Phase of Flight

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/d0cf931e-0455-48a6-bb5a-db211ef1b12c)

### Interpretation:
- Many Aviation accidents happen during landing and the damage level is 
  substantial followed by takeoff.
- If an accident happens during maneuvering phase, the aircraft is likely 
  to be destroyed.

## : Aircraft Damage vs. Weather Condition

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/6f711bf9-d1c6-4326-a1f3-8346dd9c0270)

### Interpratation:
- Accidents occur during VMC weather conditions and when they do, the 
  damage is critical hence it's better to take flights during IMC weather 
  conditions
  
## : Distribution of Aircraft Models Involved in Accidents

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/330f24d3-601a-43f6-898f-d5d52cdf1d1b)


### Interpretation:
- Airplanes and helicopters that are said to be of model 172 , 152 and 
  172N poses much risk in the aviation industry

# 4.3 MULTIVARIATE ANALYSIS
Analysis for airplane and helicopter types and the severity of injuries they bring.

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/f0537c23-4cdb-4860-83b4-430e10cb6327)

### Interpretation
- It seems that Boeing seems to have less fatal injuries and has most numbers of uninjured people during accidents hence can be considered as a less risky airplane.
- On the other hand Cessna seems to have more fatalities and can be considered as a risky airplane.

- To ascertain our interpretation we can create boxplots for aircrafts damage by make and major on minor and uninjured injuries/
```
# Damage by aircraft make
fig, ax = plt.subplots(figsize=(14, 10))
sns.boxplot(data=df_filtered[df_filtered['Make'].isin(top_makes)], y='Make', x='Total.Minor.Injuries', order=top_makes, ax=ax)
ax.set_title('Aircraft Damage by Make (Total Minor Injuries)')
ax.set_xlabel('Total Minor Injuries')
ax.set_ylabel('Aircraft Make')
plt.show()

fig, ax = plt.subplots(figsize=(14, 10))
sns.boxplot(data=df_filtered[df_filtered['Make'].isin(top_makes)], y='Make', x='Total.Uninjured', order=top_makes, ax=ax)
ax.set_title('Aircraft Damage by Make (Total Uninjured)')
ax.set_xlabel('Total Minor Injuries')
ax.set_ylabel('Aircraft Make')
plt.show()
```

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/11a62980-b021-41e8-aeae-0a156dff370c)

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/50e7a681-4267-4e87-9dab-842bab1da914)


### Interpretation:
It can be seen that airplanes and helicopters made by Boeing and Bell are less risky since they seem to pose less severe injuries

# CONCLUSION BY VISUALS
We shall use injury severity to show that our filtered aircrafts i.e airplane and helicopters have more uninjured incidents and less fatal incidents hence more safer aircraft.

![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/d10878b1-2c6a-4bec-af69-1d0a68de9dd7)


**Dealing with uninjured injuries to get more specifics on the make and model.**

### 5.1 Severity of Uninjured Injuries by Aircraft Make

```
# Group by Make and Model, and sum up the Total.Uninjured column
grouped_data = df_filtered.groupby(['Aircraft.Category', 'Make', 'Model'])['Total.Uninjured'].sum().reset_index()

# Sort the grouped data by Total.Uninjured in descending order
grouped_data = grouped_data.sort_values(by='Total.Uninjured', ascending=False)
# Top 10 models for airplanes
top_airplanes = grouped_data[grouped_data['Aircraft.Category'] == 'Airplane'].head(10)

# Top 10 models for helicopters
top_helicopters = grouped_data[grouped_data['Aircraft.Category'] == 'Helicopter'].head(10)

# Display the top 10 models for each category
print("Top 10 Airplane Models with Highest Uninjured Outcomes")
print(top_airplanes)

print("\nTop 10 Helicopter Models with Highest Uninjured Outcomes")
print(top_helicopters)
```

**Top 10 Airplane Models with Highest Uninjured Outcomes**
     Aircraft.Category    Make    Model  Total.Uninjured
998           Airplane  Boeing      737       4404.00000
1070          Airplane  Boeing      767       2372.32544
1054          Airplane  Boeing      757       2052.00000
1088          Airplane  Boeing      777       1757.00000
1005          Airplane  Boeing  737 7H4       1704.00000
1029          Airplane  Boeing  737-7H4       1587.00000
263           Airplane  Airbus     A320       1531.00000
1094          Airplane  Boeing  777-222       1271.00000
1043          Airplane  Boeing      747       1199.00000
270           Airplane  Airbus     A321       1073.00000

**Top 10 Helicopter Models with Highest Uninjured Outcomes**
     Aircraft.Category                         Make     Model  Total.Uninjured
6876        Helicopter                         Bell      206B       188.278077
7375        Helicopter  Robinson Helicopter Company    R44 Ii       176.325440
7355        Helicopter                     Robinson       R44       120.650879
7363        Helicopter          Robinson Helicopter  R22 Beta       116.325440
7348        Helicopter                     Robinson       R22       107.301758
6859        Helicopter                         Bell       206        90.000000
6898        Helicopter                         Bell       407        89.000000
7406        Helicopter                    Schweizer      269C        88.650879
7365        Helicopter          Robinson Helicopter       R44        85.000000
7350        Helicopter                     Robinson  R22 Beta        73.325440


![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/f587fff7-18fa-47e5-981c-fd8ace3d60f1)


![download](https://github.com/BrianGithinji-BMG/First-Project/assets/133196863/9e37ef77-cb8a-4abc-9140-35d887d30252)

# Conclusion

The analysis of the aviation dataset has led to several key insights regarding the safety of different aircraft models, specifically focusing on airplanes and helicopters. By evaluating the uninjured outcomes, we can identify the safest models for both personal and commercial aviation purposes.

## Key Findings

### Top 10 Airplane Models with Highest Uninjured Outcomes

- Boeing models dominate the list, indicating a strong safety record.
- Airbus A320 and A321 also show high safety with substantial uninjured outcomes.

**Top 10 Airplane Models:**
- Boeing 737
- Boeing 767
- Boeing 757
- Boeing 777
- Boeing 737 7H4
- Boeing 737-7H4
- Airbus A320
- Boeing 777-222
- Boeing 747
- Airbus A321

### Top 10 Helicopter Models with Highest Uninjured Outcomes

- Bell and Robinson Helicopter Company models are prevalent in the top 10.
- These models show consistent safety performance.

**Top 10 Helicopter Models:**
- Bell 206B
- Robinson R44 II
- Robinson R44
- Robinson R22 Beta
- Robinson R22
- Bell 206
- Bell 407
- Schweizer 269C
- Robinson R44
- Robinson R22 Beta

# Recommendations

## Focus on High-Safety Models
- For airplane acquisitions, prioritize models such as Boeing 737, 767, 757, 777, and Airbus A320/A321.
- For helicopters, focus on Bell 206B, Robinson R44 II, and other top-performing models.

## Ensure Regular Maintenance and Training
- Continuous maintenance of aircraft to ensure they remain in top safety condition.
- Regular training for pilots to handle various flight scenarios and emergencies effectively.

## Invest in Safety Upgrades
- Upgrade older models with the latest safety technologies.
- Implement advanced monitoring systems for real-time assessment of aircraft health.

## Data-Driven Decision Making
- Use data analytics continuously to monitor the safety performance of the fleet.
- Regularly update the safety protocols based on the latest data insights.

# Final Remarks

This analysis highlights the importance of leveraging data to make informed decisions in the aviation industry. By focusing on the models with the highest uninjured outcomes and adhering to best safety practices, the company can mitigate risks and ensure a successful business venture in aviation. Continuously challenging and reviewing the solutions will help in refining the approach and ensuring robust decision-making.


