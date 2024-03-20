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

<iframe src="figures/duration_dist.html" width=800 height=600 frameBorder=0></iframe>

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

|   YEAR |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|-------:|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
|   2000 |             0       |                  0      |              0       |    0        |         0       |          63.0556 |                       12.125    |
|   2001 |             8.23333 |                  0      |              0       |    0        |         4.27222 |         169      |                       11.863    |
|   2002 |             0       |                  0      |              0       |    0        |         0       |          99.85   |                        3.40556  |
|   2003 |             8.66944 |                  0      |             22.1583  |    0        |        25.8     |         104.991  |                       42.1429   |
|   2004 |             4.47333 |                  0      |              0       |    0        |        59.3214  |          84.4161 |                        1.60556  |
|   2005 |             2.41667 |                  0      |              0       |    0        |         0       |         100.799  |                        3.8125   |
|   2006 |             2.65    |                  0      |              0       |    0        |         5.95    |          66.9182 |                        3.81667  |
|   2007 |             5.29722 |                 27.9333 |              0       |    0.783333 |        40.5083  |          48.2717 |                        7.65417  |
|   2008 |             6.0037  |                293.306  |              0       |    5.14167  |       100.958   |          84.4318 |                        6.36905  |
|   2009 |           143.847   |                109.5    |              0       |    6.3375   |         6.4     |          65.087  |                        3.82222  |
|   2010 |             5.51    |                161      |              0       |    1.85     |        22.5262  |          62.8272 |                       35.2141   |
|   2011 |             0.225   |                137.917  |              8.91336 |    5.77778  |        27.4771  |          52.2852 |                        9.65     |
|   2012 |             1.75    |                108.125  |              4.66873 |    0.975    |        17.0111  |          68.6244 |                        5.50714  |
|   2013 |             4.875   |                127.072  |              5.68521 |    3.99762  |         0       |          44.6282 |                        2.67222  |
|   2014 |             0       |                377.262  |             12.3841  |    0.933333 |         9.50667 |          23.7729 |                        0.866667 |
|   2015 |             0       |                  4.25   |              3.55488 |    2.44583  |         5.6125  |          35.359  |                        6.25     |
|   2016 |             0       |                568.367  |              7.85449 |    3.71667  |         0       |          27.34   |                       15.7854   |

*Severe weather* tends to be a consistent cause of outages over the years with a generally high duration, peaking particularly in 2008 and 2011. This suggests that severe weather events are a major and consistent threat to power supply stability.

Also, a sample grouped table is shown below. By looking at the sum of outage durations per year per state, we can analyze trends over time, which could reveal whether there has been an improvement in utility.

|   YEAR |   Alabama |   Alaska |    Arizona |   Arkansas |   California |   Colorado |   Connecticut |   Delaware |   District of Columbia |    Florida |   Georgia |   Hawaii |    Idaho |   Illinois |    Indiana |     Iowa |   Kansas |    Kentucky |   Louisiana |       Maine |   Maryland |   Massachusetts |   Michigan |   Minnesota |   Mississippi |   Missouri |   Montana |    Nebraska |     Nevada |   New Hampshire |   New Jersey |   New Mexico |   New York |   North Carolina |   North Dakota |       Ohio |   Oklahoma |    Oregon |   Pennsylvania |   South Carolina |   South Dakota |   Tennessee |     Texas |      Utah |   Vermont |   Virginia |   Washington |   West Virginia |   Wisconsin |   Wyoming |
|-------:|----------:|---------:|-----------:|-----------:|-------------:|-----------:|--------------:|-----------:|-----------------------:|-----------:|----------:|---------:|---------:|-----------:|-----------:|---------:|---------:|------------:|------------:|------------:|-----------:|----------------:|-----------:|------------:|--------------:|-----------:|----------:|------------:|-----------:|----------------:|-------------:|-------------:|-----------:|-----------------:|---------------:|-----------:|-----------:|----------:|---------------:|-----------------:|---------------:|------------:|----------:|----------:|----------:|-----------:|-------------:|----------------:|------------:|----------:|
|   2000 |  74.9     |        0 |    1.1     |    0       |       0      |    0       |       0       |   0        |                  0     |    0       |    0      |   0      |  0       |   20       |   0        |   0      |   0      |   0         |    0        |   0         |    0       |         0       |     0      |     0       |     0         |   0        |  0        |   0         |  0         |         0       |      0       |      0       |   11.35    |         229.5    |              0 |   0        |     0      |   0       |        0       |         234      |              0 |      0      |   45.15   |   0       | 0         |    0       |      0       |       0         |      0      |   0       |
|   2001 |   0       |        0 |    0       |    0       |      87.0667 |    0       |       0       |   0        |                  0     |    0       |    0      |   0      |  0       |    0       |   0        |   0      |   0      |   0         |    0        |   0         |    0       |         1.71667 |     0      |     0       |     0         |   0        |  0        |   0         |  0         |         0       |      0       |      0       |    8.23333 |           0      |              0 |   0        |     0      |   0       |        0       |           0      |              0 |      0      |  195.783  |   0       | 0         |    4.01667 |      0       |       0         |      0      |   0       |
|   2002 |   0       |        0 |    0       |  136       |     252.383  |    0       |      98       |   0        |                  0     |    3.83333 |    0      |   0      |  0       |    0       |   0        |   0      |   0      |   0         |    0        |   0         |    0       |         0       |    60      |     0       |     0         | 257        |  0        |   0         |  0         |         0       |      0       |      0       |    0       |           0      |              0 |   0        |   198      |   0       |       58.5     |           0      |              0 |      0      |    0      |   0       | 0         |   44.85    |      0       |       0         |      0      |   0       |
|   2003 |   0       |        0 |    2.25    |    0       |     717.667  |    0       |       0       |   0        |                241.667 |   13.6     |    0      |   0      | 25.8     |   24       |   0        |   0      |   0      |   0         |    0        |   0         |  518.967   |         1.91667 |  1193.43   |     0       |     0         |   0        |  0        |   0         |  0         |         0       |      0       |      0       |  190.567   |          82.1833 |              0 | 160.283    |     0      |   0       |       72.3     |           0      |              0 |      0      |  173.817  |   0       | 0         |    4.1     |    108       |       0         |     36.3167 |   0       |
|   2004 |   0       |        0 | 1650.97    |    0       |     181.883  |    0       |       0       |   0        |                  0     | 1275.43    |   81.5    |   0      |  1.58333 |   47.5     |  87.5      |   0      |   0      |   0         |  198.483    |   0         |   65.8333  |         6.23333 |   454.333  |     0       |     0         |   0        |  0        |   0.0666667 |  0         |         0       |      0       |      0       |  170       |          31.5    |              0 | 164        |   111.417  |   0       |       38.65    |          66.1333 |              0 |      0      |  365.9    |   0       | 0         |   10.4     |    160.417   |       0         |      0      |   0       |
|   2005 |   0       |        0 |    0       |    2.1     |     234.367  |    0       |       0       |   0        |                  0     | 1498.33    |   54.0833 |   0      |  0       |    2.48333 | 265.833    |   0      | 234      |   0         |  488.933    |   0         |  215.083   |         0       |   597.983  |   214       |     0         |   0        |  0        |   0         |  0         |         0       |      0       |      0       |   54       |          28      |              0 | 434        |     0      |   0       |       57.85    |           0      |              0 |      0      |  255.5    |   0       | 0         |    0       |      0       |       0         |    123.5    |   0       |
|   2006 |   0       |        0 |    0       |    6.9     |     335.717  |   42.9833  |       2.41667 |   0        |                  0     |    0       |    2      |  44.7667 |  0       |   91.5     |  67.4167   |   0      |   0      |   0         |    0        |   3.3       |  183.133   |         0       |   225      |     0       |     0         |   0        |  0        | 160         |  0         |         0       |    131       |      0       |  672.933   |          19.5    |              0 |  30.6667   |     5      | 162.383   |      357.1     |           0      |              0 |      0      |   42.5167 |   0       | 0         |   28.75    |   1047.5     |       0         |      0      |   0       |
|   2007 |   0       |        0 |    0       |    0       |     246.783  |    0       |      71       |   0        |                  0     |    0       |    0      |   0      |  0       |  166.817   |   0.783333 |  23.1833 | 227.5    |   0         |    0        | 103.833     |  147.5     |         0       |   516.917  |     0       |     0         |  34.5      |  0        |   0         |  0         |        11       |      0       |      0       |   56.6167  |           0      |              0 |   0        |   232.867  |   0       |        1.3     |           0      |              0 |      0      |   99.0667 |   4.71667 | 0         |   20.0167  |    138.6     |       0         |      0      |   0       |
|   2008 |   0       |        0 |    0       |    0       |     686.917  |    0       |      45.8333  |   0        |                  0     |  196.9     |   32.5    |  22.7833 |  0       |  109       | 246.933    | 144.5    |   0      | 592.983     | 1339.8      |  88.7       |   96.5     |         0       |   755.7    |     0       |     0         |  14.5      |  0        |   1         |  0         |         0       |    189.783   |      0       |  407.833   |          36.7167 |              0 | 502.433    |     0      |   3.33333 |      371.217   |           0      |              0 |     14      | 1707.18   |  14.5167  | 0         |   47       |      4.13333 |       0         |      0      |   0       |
|   2009 |   0       |        0 |   70       |  216.233   |      90.2833 |   45.1667  |      76       |   0        |                  0     |   23.65    |   20      |   0      |  0       |  164.817   | 805.3      |   0      |   0      | 459.567     |   35.7      |  49.8167    |   47.5833  |         0       |  1845.23   |     0       |     0         | 161.667    |  0        |   0         |  0         |         0       |      0       |      0       |    0       |          24.3333 |              0 | 104.883    |     0      |   0       |       86       |           0      |              4 |     15.5    |  264.567  |   0.25    | 0         |   66.2833  |     20.0667  |       0         |      0      |   1.76667 |
|   2010 |   0       |        0 |    0       |    3.43333 |     603.517  |    6.51667 |      57.35    |   0.833333 |                185.517 |    6.2     |    0      |   0      |  0       |  174.667   | 428        |   0      |   0      |   0         |  269.317    |  11.2167    |  179       |         0       |   503.983  |   130.5     |     0         |   0        |  0.216667 |   2.65      |  0         |         0       |    528.983   |      0       |  892.35    |           0.7    |              0 |  30.7167   |   122.467  |   0       |      681.4     |           0      |              0 |      0      |  173.783  |  37.9167  | 0         |   62.9167  |     86       |       0         |     14.4667 |   1.01667 |
|   2011 |   0       |        0 |   12.8333  |  174.567   |     697.717  |   38.7333  |      27.5167  |   7.61667  |                151.417 |   43.35    |   69.1833 |   2.9    | 20.5     |  151.417   | 153.367    |   0      |  15.2167 |  50.3167    |   28.2833   |   3.25      |  365.983   |         8.01667 |   877.533  |    73.0333  |     5         | 260.867    |  0        |   0         | 57.15      |         7.83333 |    608.417   |      0       | 1580.83    |         164.1    |             12 | 210.55     |   138.383  |  32.7333  |     1000.18    |          40.25   |              0 |    223.783  |  406.617  | 100.667   | 0.85      |   53.3     |    197.933   |       0         |      0      |   0       |
|   2012 |   0       |        0 |    0       |   19.8333  |     239.767  |    5.16667 |       1       |   0.783333 |                134.617 |    4       |    1.8    |   0      |  0       |   92.6667  |  41.5167   |  36.0167 |   0      |   0         |  168.717    |  19.7833    |  313.333   |       118.15    |   282.75   |    42.5     |     0         |   0        |  0        |   0         |  1.81667   |        45.1333  |    735.267   |      5.23333 | 1005.28    |          27.0833 |              0 | 330.533    |    41.2    |   0       |      689       |           0      |              0 |     66      |  154.933  |   0       | 4.43333   |  188.067   |    139.867   |     447.583     |      0      |   0       |
|   2013 |   1.28333 |        0 |  139.2     |    7.35    |     163.167  |    2.95    |       5.53333 |   2        |                  3.15  |    4.31667 |    0      |   0      |  0       |   36.25    | 305.783    |  27.2833 |   0      |   0         |    3.78333  |  49.2       |   19.5     |       129.417   |   668.133  |   189.35    |     0         |  93.5      |  1.55     |   0         |  4.56667   |         1.28333 |      0       |     13.4833  |  227.4     |          60.55   |              0 |  27.75     |   167.367  |  53.0167  |        6.33333 |           0      |              0 |     57.2833 |  851.15   |   1.85    | 0.0166667 |   60.4333  |     58.35    |       0         |     23.4667 |   0.55    |
|   2014 |   6.5     |        0 |   14.8833  |    4.75    |      30.8833 |    0       |       0       |   1.38333  |                  0.9   |    0       |   86.8167 |   0      |  0       |   92.5167  |  60.7167   | 408.183  |   0      |   0.0166667 |    0.866667 |   0         |   50.6167  |         0       |   156.25   |     1.01667 |     0.516667  |   0.416667 |  0.933333 |   0         |  0         |         0       |      5.88333 |      0       | 1441.02    |         185.2    |              0 |  10.5667   |     0      |  16.05    |      162.05    |          77.6167 |              0 |    134.6    |  111.617  |   0.3     | 0.0166667 |    0       |     28.8667  |      16.6667    |   2280.47   |   0       |
|   2015 |  13.3833  |        0 |    2.03333 |   59.8167  |     102.883  |    1.75    |       0       |  70.9      |                  0     |    0       |   33.3167 |   0      |  0       |    1.5     |   2        |   0      |  33.85   |   1.8       |    0        |   0.0166667 |    5.06667 |         0       |   229.917  |    31.5833  |     0.0833333 |   1.08333  |  0        |   0         |  0.0166667 |         0       |    170.2     |      0       |    7.01667 |          32.5    |              0 |   0.116667 |    77.0333 |   6.05    |       28.8667  |           0      |              0 |     27.1833 |  457.8    |   4.76667 | 0         |   40.5833  |    209.133   |       0.0166667 |      0      |   0       |
|   2016 |   0       |        0 |    3.78333 |    0       |     828.917  |   66.9833  |       0       |  13.1      |                  0     |    1.38333 |    0      |   0      |  7.4     |    0       |   0        |   0      |   0      |   0         |   53        |   0         |   27.8833  |        17.8     |    30.2167 |     0       |     0         |  19.9833   |  0        |   0         |  1         |         0       |      1.28333 |      0       |  316.35    |          25.3667 |              0 |   0        |    13.2667 |  45.8833  |        7.36667 |           0      |              0 |      0      |  193.417  |   6       | 0         |    0       |     37.2167  |       0         |     26.75   |   0       |

 These states show notable figures in certain years (e.g., Florida in 2005 and Arizona in 2004), which might correspond to specific large-scale outages or reporting changes.