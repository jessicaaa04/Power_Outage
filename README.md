# Predictive Analysis for Uncovering the Causes of Major Power Outages

**Authors**: Jessica Zhang, Stephanie Wang

---

## Project Overview

This is a data science project on predicting the causes of a major power outage. The dataset used to explore the topic can be find [here](https://engineering.purdue.edu/LASCI/research-data/outages). This project is for DSC80 at UCSD.

---

## Introduction

Power exists everywhere in our lives, woven into the fabric of our daily routines and essential activities. We use power to brighten our homes, to charge the devices that keep us connected to the world, and to preserve the food that nourishes us. Imagine this: You're about to sit down for a family dinner or dive into a critical work project when suddenly, everything goes dark. The power's out, again. Frustration kicks in, plans get disrupted, and the uncertainty of when things will return to normal looms over you. This scenario is far too common for many of us and highlights a pressing issue in our modern lives—the vulnerability of our power systems to outages. *In order to minimize the inconveniences associated with power outages, we will uncover the cause of a major power outage in U.S.*

### Introduction to the Datasets in the Study

The dataset for power outages contains *1534* rows, each representing information on major outages experienced by different states from 2016 to 2020, with *55* columns regarding to the following information.

| Column Name | Description |
| --- | --- |
| `YEAR` | Year when the outage occurred |
| `MONTH` | Month when the outage occurred |
| `U.S._STATE` | States in U.S. |
| `POSTAL.CODE` | Postal code of the states |
| `NERC.REGION` | North American Electric Reliability Corporation regions involved in the outage |
| `CLIMATE.REGION` | U.S. climate regions |
| `ANOMALY.LEVEL` | Oceanic El Niño/La Niña (ONI) index |
| `CLIMATE.CATEGORY` | Climate episodes |
| `OUTAGE.START.DATE` | Day of the year when the outage started |
| `OUTAGE.START.TIME` | Time of the day when the outage started |
| `OUTAGE.RESTORATION.DATE` | Day of the year when power was restored |
| `OUTAGE.RESTORATION.TIME` | Time of the day when power was restored |
| `CAUSE.CATEGORY` | Category causing major power outages |
| `CAUSE.CATEGORY.DETAIL` | Description of categories |
| `HURRICANE.NAMES` | Hurricane name |
| `OUTAGE.DURATION` | Duration of outage |
| `DEMAND.LOSS.MW` | Amount of peak demand lost |
| `CUSTOMERS.AFFECTED` | Number of customers affected by the power outages |
| `RES.PRICE` | Monthly electricity price in the residential sector |
| `COM.PRICE` | Monthly electricity price in the commercial sector |
| `IND.PRICE` | Monthly electricity price in the industrial sector |
| `TOTAL.PRICE` | Average monthly electricity price |
| `RES.SALES` | Residential electricity consumption |
| `COM.SALES` | Commercial electricity consumption |
| `IND.SALES` | Industrial electricity consumption |
| `TOTAL.SALES` | Total electricity consumption |
| `RES.PERCEN` | Percentage of residential electricity consumption |
| `COM.PERCEN` | Percentage of commercial electricity consumption |
| `IND.PERCEN` | Percentage of industrial electricity consumption |
| `RES.CUSTOMERS` | Annual number of customers served in the residential electricity sector |
| `COM.CUSTOMERS` | Annual number of customers served in the commercial electricity sector |
| `IND.CUSTOMERS` |Annual number of customers served in the industrial electricity sector |
| `TOTAL.CUSTOMERS` | Annual number of total customers |
| `RES.CUST.PCT` | Percent of residential customers |
| `COM.CUST.PCT` | Percent of commerical customers |
| `IND.CUST.PCT` | Percent of industrial customers |
| `PC.REALGSP.STATE` | Per capita real gross state product in the states |
| `PC.REALGSP.USA` | Per capita real gross state product in U.S. |
| `PC.REALGSP.REL` | Relative per capita real gross state product |
| `PC.REALGSP.CHANGE` | Percentage change of per capita real GSP from the previous year |
| `UTIL.REALGSP` | Real capita real gross state product contributed by Utility industry |
| `TOTAL.REALGSP` | Real capita real gross state product contributed by all industries |
| `UTL.CONTRI` | Utility industry's contribution to the total capita real gross state product in the state |
| `PI.UTIL.OFUSA` | State utility sector׳s income as a percentage of the total earnings of the utility sector's income |
| `POPULATION` | Population in the state |
| `POPPCT_URBAN` |Percentage of the total population of the state represented by the urban population |
| `POPPCT_UC` | Percentage of the total population of the state represented by the population of the urban clusters |
| `POPDEN_URBAN` | Population density of the urban areas |
| `POPDEN_UC` | Population density of the urban clusters |
| `POPDEN_RURAL` | Population density of the rural areas |
| `AREAPCT_URBAN` | Percentage of the land area of state represented by the land area of the urban areas |
| `AREAPCT_UC` | Percentage of the land area of state represented by the land area of the urban clusters |
| `PCT_LAND` | Percentage of land area in state as compared to the overall land area |
| `PCT_WATER_TOT` | Percentage of water area in the state as compared to the overall water area |
| `PCT_WATER_INLAND` | Percentage of inland water area in the state as compared to the overall inland water area |

In the project, we used several key columns from the dataset to understand the factors leading to power outages, particularly focusing on those caused by severe weather. The columns of interest spanned various dimensions, including electricity pricing (e.g., `RES.PRICE` for residential, `COM.PRICE` for commercial, `IND.PRICE` for industrial sectors), sales volumes (`RES.SALES`, `COM.SALES`, `IND.SALES`), customer demographics (`RES.CUSTOMERS`, `COM.CUSTOMERS`, `IND.CUSTOMERS`), and broader economic indicators such as state and national GDP (`PC.REALGSP.STATE`, `PC.REALGSP.USA`). 

---

## Cleaning and Exploratory Data Analysis

### Data Cleaning

In order to increase the readability of the data and performance of our model, we conducted data cleaning through the following steps:

- **Drop meaningless columns and reset the index starting from 0**: The original Excel file includes a row that illustrates the unit of measurement for some columns, as well as a column labeled OBS indicating the sequence of cases as 1, 2, 3, 4, and so on.
	
- **Replace missing values with np.nan**: It  ensures a consistent representation of missing values across different columns and data types, making it easier to identify, count, and handle these missing entries.

- **Check the datatypes of each column, especially the columns related to datetime**: The `OUTAGE.START.DATE`, `OUTAGE.START.DATE`, `OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME` columns in the dataframe are stored as objects. We converted it into datetime data type so that we can apply basic operations later. Convert numerical columns such as `ANOMALY.LEVEL`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, etc., into float or int types.

- **Merge date and time into a single column**: This process creates a more comprehensive and accurate representation of specific events or records in a dataset. The resulting singular datetime column then allows for more efficient sorting, filtering, and time series analysis within the data.

- **Add time duration in hours**: By extracting the day of the week from the outage start and restoration times, we can observe and analyze patterns in outage occurrences and recovery times across different days.

After cleaning the dataframe, the dataframe looks like ths following (only showing the first 5 rows and part of the crucial columns for illustration):

|   RES.PRICE |   COM.PRICE |   IND.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   RES.CUSTOMERS |
|------------:|------------:|------------:|------------:|------------:|------------:|----------------:|
|       11.6  |        9.18 |        6.81 |     2332915 |     2114774 |     2113291 |     2.30874e+06 |
|       12.12 |        9.71 |        6.49 |     1586986 |     1807756 |     1887927 |     2.34586e+06 |
|       10.87 |        8.19 |        6.07 |     1467293 |     1801683 |     1951295 |     2.30029e+06 |
|       11.79 |        9.25 |        6.71 |     1851519 |     1941174 |     1993026 |     2.31734e+06 |
|       13.07 |       10.16 |        7.74 |     2028875 |     2161612 |     1777937 |     2.37467e+06 |

### Univariate Analysis

After cleaning the dataframe, we analyzed the distribution of outage duration in hours and anomaly level.

#### Distribution of Duration in Hours

Here is a histogram on the distribution of the duration in hours of power outages in U.S.

Since the data is right-skewed, most outages are short-lived, but there are a few that last much longer and can skew averages. For a thorough risk assessment, an energy company should use median or a range of percentiles for more representatie comparison, and analyze outliers separately to understand the causes behind prolonged outages and to identify any patters in these extreme cases.

<iframe src="figures/duration_dist.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Anomaly Level

Here is a histogram on the distribution of the anomaly level in our dataset.

This figure suggests that the distribution could be approximated as a Gaussian distribution, slightly skewed to the right, with a break between 1.7 and 2. We would say that the figure centered around -0.35, meaning that most power outages have anomaly level at -0.35.

<iframe src="figures/anomaly_dist.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis
