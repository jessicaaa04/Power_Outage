# Predictive Analysis for Uncovering the Causes of Major Power Outages

**Authors**: Jessica Zhang, Stephanie Wang

---

## Project Overview

This is a data science project on predicting the causes of a major power outage. The dataset used to explore the topic can be find [here](https://engineering.purdue.edu/LASCI/research-data/outages). This project is for DSC80 at UCSD.

---

## Introduction

Power exists everywhere in our lives, woven into the fabric of our daily routines and essential activities. We use power to brighten our homes, to charge the devices that keep us connected to the world, and to preserve the food that nourishes us. Imagine this: You're about to sit down for a family dinner or dive into a critical work project when suddenly, everything goes dark. The power's out, again. Frustration kicks in, plans get disrupted, and the uncertainty of when things will return to normal looms over you. This scenario is far too common for many of us and highlights a pressing issue in our modern lives—the vulnerability of our power systems to outages. *In order to minimize the inconveniences associated with power outages, we will uncover the cause of a major power outage in U.S.*

### Introduction to the Dataset in the Study

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

First, we read the Excel file and loaded into notebook. In order to increase the readability of the data and performance of our model, we conducted data cleaning through the following steps:

- **Drop meaningless columns and reset the index starting from 0**: The original Excel file includes a row that illustrates the unit of measurement for some columns, as well as a column labeled OBS indicating the sequence of cases as 1, 2, 3, 4, and so on.
	
- **Replace missing values with np.nan or 0**: It  ensures a consistent representation of missing values across different columns and data types, making it easier to identify, count, and handle these missing entries. For columns `MONTHS` and `CUSTOMERS.AFFECTED`, we filled null values with 0.

- **Check the datatypes of each column, especially the columns related to datetime**: The `OUTAGE.START.DATE`, `OUTAGE.START.DATE`, `OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME` columns in the dataframe are stored as objects. We converted it into datetime data type so that we can apply basic operations later. Convert numerical columns such as `ANOMALY.LEVEL`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, etc., into float or int types.

- **Merging date and time into a single column**: This process creates a more comprehensive and accurate representation of specific events or records in a dataset. The resulting singular datetime column then allows for more efficient sorting, filtering, and time series analysis within the data.

- **Adding time duration in hours**: By extracting the day of the week from the outage start and restoration times, we can observe and analyze patterns in outage occurrences and recovery times across different days.

After cleaning the dataframe, the dataframe looks like ths following (only showing the first 5 rows and part of the crucial columns for illustration):

|   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.DURATION.HOUR | OUTAGE.START.DAY   | OUTAGE.RESTORATION.DAY   |
|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|-----------------------:|:-------------------|:-------------------------|
|   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 |     2332915 |     2114774 |     2113291 |       6562520 |      35.5491 |      32.225  |      32.2024 |     2.30874e+06 |          276286 |           10673 |       2.5957e+06  |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |  5.34812e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |             51         | Friday             | Sunday                   |
|   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 |     1586986 |     1807756 |     1887927 |       5284231 |      30.0325 |      34.2104 |      35.7276 |     2.34586e+06 |          284978 |            9898 |       2.64074e+06 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |  5.45712e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |              0.0166667 | Sunday             | Sunday                   |
|   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 |     1467293 |     1801683 |     1951295 |       5222116 |      28.0977 |      34.501  |      37.366  |     2.30029e+06 |          276463 |           10150 |       2.58690e+06 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |  5.3109e+06  |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |             50         | Tuesday            | Thursday                 |
|   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 |     1851519 |     1941174 |     1993026 |       5787064 |      31.9941 |      33.5433 |      34.4393 |     2.31734e+06 |          278466 |           11010 |       2.60681e+06 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |  5.38044e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |             42.5       | Tuesday            | Wednesday                |
|   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 |     2028875 |     2161612 |     1777937 |       5970339 |      33.9826 |      36.2059 |      29.7795 |     2.37467e+06 |          289044 |            9812 |       2.67353e+06 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |  5.48959e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |             29         | Saturday           | Sunday                   |


### Univariate Analysis

After cleaning the dataframe, we analyzed the distribution of outage duration in hours and anomaly level.

#### Distribution of Duration in Hours

Here is a histogram on the distribution of the duration in hours of power outages in U.S.

Since the data is right-skewed, most outages are short-lived, but there are a few that last much longer and can skew averages. For a thorough risk assessment, an energy company should use median or a range of percentiles for more representatie comparison, and analyze outliers separately to understand the causes behind prolonged outages and to identify any patters in these extreme cases.

<iframe src="figures/duration_by_state.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Anomaly Level

Here is a histogram on the distribution of the anomaly level in our dataset.

This figure suggests that the distribution could be approximated as a Gaussian distribution, slightly skewed to the right, with a break between 1.7 and 2. We would say that the figure centered around -0.35, meaning that most power outages have anomaly level at -0.35.

<iframe src="figures/anomaly_dist.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

To identify potential correlations within the data, bivariate analysis is essential for exploring and understanding the relationship between two variables. In the first figure, we used boxplot to illustrate the relationship between cause category and outrage duration in hours.

<iframe src="figures/cause_duration.html" width=800 height=600 frameBorder=0></iframe>

From the plot, ***fuel supply emergency*** has the widest range of outage durations, indicating high variablity, with some outages lasting significantly longer than those caused by other factors. ***Severe weather*** is another major cause, with a higher median outage duration than most categories but fewer outliers that *fuel supply emergency*. Overall, the plot indicates that the cause of an outage is a strong indicator of its potential duration and variability.

### Interesting Aggregates

To construct a more reliable analysis, we created a pivot table illustrating the average outage duration in hourse by cause category for different years.

<iframe src="figures/cause_year.html" width=800 height=600 frameBorder=0></iframe>

*Severe weather* tends to be a consistent cause of outages over the years with a generally high duration, peaking particularly in 2008 and 2011. This suggests that severe weather events are a major and consistent threat to power supply stability.

Also, a sample grouped table is shown below. By looking at the sum of outage durations per year per state, we can analyze trends over time, which could reveal whether there has been an improvement in utility.

<iframe src="figures/cause_year.html" width=800 height=600 frameBorder=0></iframe>

 These states show notable figures in certain years (e.g., Florida in 2005 and Arizona in 2004), which might correspond to specific large-scale outages or reporting changes.