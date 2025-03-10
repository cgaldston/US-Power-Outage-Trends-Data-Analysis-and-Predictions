# US Power Outage Trend Data Analysis and Predictions
In this project we analyze trends in data on power outages in the United States over the past fifteen years to draw meaningful insights and make predictions. Final project for DSC 80 at UCSD. 


https://cgaldston.github.io/US-Power-Outage-Trends-Data-Analysis-and-Predictions/

### Introduction

For this project, we decided to examine data on major power outages in the continental U.S. from January 2000 to July 2016. The Department of Energy defines a major power outage as an outage that "impacted atleast 50,000 customers or caused an unplanned firm load loss of atleast 300 MW." Major power outages can have an incredibly detrimental impact on communities, so understanding what factors play a role in the magnitude of these outages. This dataset was curated by Mukherjee, Nateghi, Hastak at Purdue School of Engineering. To determine magnitude we chose to primarily focus on the duration of an outage because the longer that an area is out of power can have a large impact on 

Our key question for this project is **What are the main factors that impact the duration of major power outages in the continental U.S and how can we accurately predict how long an outage will last?** This is an important question as if we can understand what is contributing the most to power outages we may be able to help local governments and utility companies respond more effectively to future incidents. 

Our dataset consists of 1534 rows and 56 columns, but the main ones that we are going to look at our project are the following:

| Feature        | Description     |
|:-------------|:------------------|
| `YEAR`       | The year when the outage occurred| 
| `MONTH` | The month when the outage occurred |
| `U.S._STATE` | State the outage occured in| 
| `OUTAGE.START.DATE`| Day of the year when the outage started |
| `OUTAGE.START.TIME`| Time of the day when the outage started |
| `RESTORATION.START.DATE`| Day of the year when energy was restored for outage|
| `RESTORATION.START.TIME`| Time of the day when energy was restored for outage|
| `CLMATE.CATEGORY`| Climate episode during outage (Warm/Normal/Cold) |
| `OUTAGE.DURATION`| Duration of outage (minutes) |
| `POPULATION`| Duration of outage (minutes) |
| `DEMAND.LOSS.MW`|  Amount of peak demand lost during an outage event (Megawatt) |
| `CUSTOMERS.AFFECTED`| Number of customers affected by a power outage event |
| `RES.SALES`| Electricity consumption in the residential sector (megawatt-hour)|
| `COM.SALES`| Electricity consumption in the commercial sector (megawatt-hour)|
| `IND.SALES`| Electricity consumption in the industrial sector (megawatt-hour)|
| `TOTAL.SALES`| Total electricity consumption in the U.S. state (megawatt-hour)|
| `UTIL.CONTRI`| % of Utility industry׳s contribution to the total GSP in the State|
| `PC.REALGSP.STATE`| Per capita real gross state product (GSP) in the U.S. state (measured in 2009 chained U.S. dollars)|
| `CAUSE.REGION`| U.S. Continental Region|
| `CAUSE.CATEGORY`| Categories of all the events causing the major power outages|
| `CAUSE.CATEGORY.DETAIL`| Detailed description of the event categories causing the major power outages|
| `DEMAND.LOSS.MW`| Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]|




## Data Cleaning and Exploratory Data Analysis

### Cleaning our dataset
1. We started our data cleaning process by removing columns from our dataset that we definitively knew we were not going to use later. These included columns such as `PCT_LAND`, `PCT_WATER_TOT`, `PCT_WATER_INLAND`, `AREAPCT_UC`, `AREAPCT_URBAN`, `POPDEN_RURAL`, `UTIL.CONTRI`, `UTIL.CONTRI`, `PC.REALGSP.STATE`, `PC.REALGSP.USA`, `RES.PERCEN`, `COM.PERCEN`, `IND.PERCEN`, `HURRICANE.NAMES`, `POSTAL.CODE`

2. Next we noticed that our dataset had a few columns that contained dates that were formatted as strings. These columns included `OUTAGE.START.DATE`, `OUTAGE.START.TIME`, `OUTAGE.RESTORATION.DATE`, and `OUTAGE.RESTORATION.TIME`. We combined the dates and times and then converted these strings to pd.DateTime objects so that we could use them in our plots and model. 

3. 


### Head of cleaned dataset
| YEAR | MONTH | U.S._STATE | POSTAL.CODE | CLIMATE.REGION | ANOMALY.LEVEL | CLIMATE.CATEGORY | CAUSE.CATEGORY | CAUSE.CATEGORY.DETAIL | OUTAGE.DURATION | DEMAND.LOSS.MW | CUSTOMERS.AFFECTED | RES.PRICE | COM.PRICE | IND.PRICE | TOTAL.PRICE | RES.SALES | COM.SALES | IND.SALES | TOTAL.SALES | RES.CUSTOMERS | COM.CUSTOMERS | IND.CUSTOMERS | TOTAL.CUSTOMERS | RES.CUST.PCT | COM.CUST.PCT | IND.CUST.PCT | PC.REALGSP.REL | PC.REALGSP.CHANGE | UTIL.REALGSP | TOTAL.REALGSP | PI.UTIL.OFUSA | POPULATION | POPPCT_URBAN | POPPCT_UC | POPDEN_URBAN | POPDEN_UC | OUTAGE.START | OUTAGE.RESTORATION |
|------|-------|-----------|-------------|----------------|---------------|------------------|----------------|------------------------|----------------|---------------|------------------|-----------|-----------|-----------|-------------|-----------|-----------|-----------|-------------|---------------|---------------|---------------|----------------|-------------|-------------|-------------|---------------|----------------|-------------|-------------|-------------|------------|-------------|------------|-------------|------------|---------------------|---------------------|
| 2011 | 7 | Minnesota | MN | East North Central | -0.3 | normal | severe weather | nan | 3060 | nan | 70000 | 11.6 | 9.18 | 6.81 | 9.28 | 2332915 | 2114774 | 2113291 | 6562520 | 2.30874e+06 | 276286 | 10673 | 2.5957e+06 | 88.9448 | 10.644 | 0.411181 | 1.07738 | 1.6 | 4802 | 274182 | 2.2 | 5.34812e+06 | 73.27 | 15.28 | 2279 | 1700.5 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
| 2014 | 5 | Minnesota | MN | East North Central | -0.1 | normal | intentional attack | vandalism | 1 | nan | nan | 12.12 | 9.71 | 6.49 | 9.28 | 1586986 | 1807756 | 1887927 | 5284231 | 2.34586e+06 | 284978 | 9898 | 2.64074e+06 | 88.8335 | 10.7916 | 0.37482 | 1.08979 | 1.9 | 5226 | 291955 | 2.2 | 5.45712e+06 | 73.27 | 15.28 | 2279 | 1700.5 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
| 2010 | 10 | Minnesota | MN | East North Central | -1.5 | cold | severe weather | heavy wind | 3000 | nan | 70000 | 10.87 | 8.19 | 6.07 | 8.15 | 1467293 | 1801683 | 1951295 | 5222116 | 2.30029e+06 | 276463 | 10150 | 2.5869e+06 | 88.9206 | 10.687 | 0.392361 | 1.06683 | 2.7 | 4571 | 267895 | 2.1 | 5.3109e+06 | 73.27 | 15.28 | 2279 | 1700.5 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
| 2012 | 6 | Minnesota | MN | East North Central | -0.1 | normal | severe weather | thunderstorm | 2550 | nan | 68200 | 11.79 | 9.25 | 6.71 | 9.19 | 1851519 | 1941174 | 1993026 | 5787064 | 2.31734e+06 | 278466 | 11010 | 2.60681e+06 | 88.8954 | 10.6822 | 0.422355 | 1.07148 | 0.6 | 5364 | 277627 | 2.2 | 5.38044e+06 | 73.27 | 15.28 | 2279 | 1700.5 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
| 2015 | 7 | Minnesota | MN | East North Central | 1.2 | warm | severe weather | nan | 1740 | 250 | 250000 | 13.07 | 10.16 | 7.74 | 10.43 | 2028875 | 2161612 | 1777937 | 5970339 | 2.37467e+06 | 289044 | 9812 | 2.67353e+06 | 88.8216 | 10.8113 | 0.367005 | 1.09203 | 1.7 | 4873 | 292023 | 2.2 | 5.48959e+06 | 73.27 | 15.28 | 2279 | 1700.5 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |



### Univariate Analysis
For our univariate analysis it first makes sense to look at the distribution of the `OUTAGE.DURATION` looks as this is the focus of our predictive task. 

<iframe
  src="assets/duration_univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on this plot, we can tell that most of the power outages are in the dataset last less than ten thousand total minutes, however there are some outliers that lasted much longer. After doing some research into the data, we found that there were a few outliers that seemed to be incorrect as upon researching these outages we were unable to find references of these outages online, so we decided to remove them from our data set. 

## Assessment of Missingness

# NMAR Analysis
A columns that is most likely  NMAR is `OUTAGE.START`. The paper that this data comes from states that the data was aquired from a variety of public datasets. In this case it might be possible the time `OUTAGE.RESTORATION` was avaiable from a dataset , but its correspodning `OUTAGE.START` was not hence the NA value. Information we could collect in order for the missigness to determine if missingess Mechanism is MAR is to check the sources used for the collection of `OUTAGE.RESTORATION` (`OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`) and Determine whether some sources less likely to have the correspodinng OUTAGE.START for an `OUTAGE.RESTORATION`. 

# Missingness Dependency
We will be testing the missingness dependency for `DEMAND.LOSS.MW` with respect to `CAUSE.CATEGORY` and `CLIMATE.REGION`.
- **CAUSE.CATEGORY**:
We will first examine the distribution of `CAUSE.CATEGORY` when `DEMAND.LOSS.MW` is missing vs not missing.

Null Hypothesis: The distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing vs not missing.

Alternate Hypothesis: The distribution of `CAUSE.CATEGORY` is different when `DEMAND.LOSS.MW` is missing vs not missing.

<iframe
  src="assets/demand_loss_missingness_Cause_cat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


- **CLIMATE.REGION**:
Now we will examine the distribution of `CLIMATE.REGION` when `DEMAND.LOSS.MW` is missing vs not missing.

Null Hypothesis: The distribution of `CLIMATE.REGION` is the same when `DEMAND.LOSS.MW` is missing vs not missing.

Alternate Hypothesis: The distribution of `CLIMATE.REGION` is different when `DEMAND.LOSS.MW` is missing vs not missing.

<iframe
  src="assets/demand_loss_missingness_CLIMATE_cat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing


## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
