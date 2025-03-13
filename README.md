# US Power Outage Trend Data Analysis and Predictions
In this project we analyze trends in data on power outages in the United States over the past fifteen years to draw meaningful insights and make predictions. Final project for DSC 80 at UCSD. 


https://cgaldston.github.io/US-Power-Outage-Trends-Data-Analysis-and-Predictions/

### Introduction

For this project, we decided to examine data on major power outages in the continental U.S. from January 2000 to July 2016. The Department of Energy defines a major power outage as an outage that "impacted atleast 50,000 customers or caused an unplanned firm load loss of atleast 300 MW." Major power outages can have an incredibly detrimental impact on communities, so understanding what factors play a role in the magnitude of these outages. This dataset was curated by Mukherjee, Nateghi, Hastak at Purdue School of Engineering. To determine magnitude we chose to primarily focus on the duration of an outage because the longer that an area is out of power the larger impact that is had on them.  

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
1. We started our data cleaning process by removing columns from our dataset that we knew we were not going to use later, either because they provide redundant infromation or would not be useful in our model. These included columns such as `PCT_LAND`, `PCT_WATER_TOT`, `PCT_WATER_INLAND`, `AREAPCT_UC`, `AREAPCT_URBAN`, `POPDEN_RURAL`, `UTIL.CONTRI`, `UTIL.CONTRI`, `PC.REALGSP.STATE`, `PC.REALGSP.USA`, `RES.PERCEN`, `COM.PERCEN`, `IND.PERCEN`, `HURRICANE.NAMES`, `POSTAL.CODE`

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

### Bivariate Analysis
For our bivariate analysis we decided to look at the relationship between outage duration the other columns in our dataset. We started by creating scatter plots between outage duration and every quantitative column in our dataset, to see if there were any noticeable relationships that we could use to better understand how different variables impact outage duration. Of these scatter plots, the most interesting relationship we found was between outage duration and customers affected. We also added a trendline to the plot so that we could better visualize this trend. 

<iframe
  src="assets/duration_vs_customers_affected.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As we can see from the plot, although the trend is no incredibly strong, there seems to be some positive correlation between the duration of the outage and the number of customers affected. This is important as it gives evidence that the duration of an outage has a result on how the outage impacts the community. We also calculated the correlation coefficient for this relationship so that we have a quantitative measurement of this trend and we found a correlation coefficient of 0.2619. This also indicates that using this feature will be helpful later in this project when we try to predict the duration of an outage. 

Another relationship that we chose to inspect was how the location of the power outage impacts the duration of the outage. To do this, we used Folium to create a map of the United States and heat mapped areas states by outage duration. 

<iframe
  src="assets/outage_map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on this plot, it seems that the state the power outage occured has an impact on the duration of the outage. This can be helpful for us to understand qualities of these areas that may impact outage duration, and also suggests it will be helpful for our prediction task as well. 

### Interesting Aggregates
For our aggreagate functions, we decided to group our dataset by the causes of the outage. We then looked at the average power outage duration and average number of customers affected for each of these groups. This allows us to see how the cause of a power outage affects the impact on the community it has. 

| CAUSE.CATEGORY                |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |\n|:------------------------------|------------------:|---------------------:|\n| equipment failure             |           399.13  |            109223    |\n| fuel supply emergency         |         10911.9   |                 0.2  |\n| intentional attack            |           429.98  |              1865.52 |\n| islanding                     |           200.545 |              6169.09 |\n| public appeal                 |          1468.45  |              7618.76 |\n| severe weather                |          3883.99  |            188490    |\n| system operability disruption |           728.87  |            210562    |

Looking at this table, it seems that there are clearly change in the magnitude of the impact depending on what causes the outage. It also seems that we still may have some outliers, as fuel supply outages almost affect zero customers on average. 

## Assessment of Missingness

# NMAR Analysis
A columns that is most likely  NMAR is `OUTAGE.START`. The paper that this data comes from states that the data was aquired from a variety of public datasets. In this case it might be possible the time `OUTAGE.RESTORATION` was avaiable from a dataset , but its corresponding `OUTAGE.START` was not hence the NA value. Information we could collect in order for the missigness to determine if missingess Mechanism is MAR is to check the sources used for the collection of `OUTAGE.RESTORATION` (`OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`) and Determine whether some sources less likely to have the correspodinng OUTAGE.START for an `OUTAGE.RESTORATION`. 

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

  The observed TVD was about 0.18, corresponding to a p-value of 0.0. Consequently, we reject the null hypothesis that the distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing versus not missing. This suggests that the missingness of `DEMAND.LOSS.MW` may be dependent on `CAUSE.CATEGORY`, but we cannot conclusively state that there is a causal relationship.
     
<iframe
  src="assets/EMP1.html"
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

  The observed TVD was about 0.034, corresponding to a p-value of 0.348. Consequently, we fail to reject the null hypothesis that the distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing versus not missing. This suggests that the missingness of `DEMAND.LOSS.MW` is not dependent on `CAUSE.CATEGORY`, but we cannot conclusively state that there is no causal relationship.

<iframe
  src="assets/EMP2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



## Hypothesis Testing

For our hypothesis test, we wanted to look at whether or not the season that the power outage occured had an affect on how long the power outage lasted. Since we didn't have a season column in our original data set, we had to map each month in the `MONTH` column to the season which that month is in to create a new `SEASON` column. Because we are going to later try to predict `OUTAGE.DURATION` in our project, we thought this test might be helpful to determine if season could be a useful feature in our model. 

**Null Hypothesis:** On average, the duration of power outages is the same regardless of what season the power outage occured. 

**Alternative Hypothesis:** On average, the duration of power outages differ depending on what season the power outage occured. 

**Test Statistic:** We used absolute difference in means as our test statistic. More specifically, we computed the absoluted difference in means for each pair of seasons, and then averaged them. 

**Results:** We performed a permutation test with 10,000 simulations by shuffling the values in the `SEASON` column and recomputing the difference in means for every simulation. We used a signficiance level of 0.05. 

The p-value that we got was 0.0008, which was less that our significance level of 0.05. As a result we rejected the null hypothesis and moved to the alternative: it seems that there is a difference in the average power outage duration for different seasons, however, we cannot conclude absolutely that season has an impact on `OUTAGE.DURATION`. Here is a plot of the absolute difference in means for every simulation. 


<iframe
  src="assests/hypothesis_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>




## Framing a Prediction Problem
Our model aims to predict the duration of an outage, a continuous variable, based on features available at the time of the event. By accurately forecasting outage durations, communities can be better prepared, enabling more efficient response strategies and minimizing disruption to essential services. Since this is a regression problem, we will evaluate our model using two key metrics:

R² : This will help us understand how well our model fits the data. R² measures the proportion of variance in the target variable that is explained by the model.

Mean Absolute Error (MAE): This will be used to quantify the error of the model's predictions. MAE gives us the average absolute difference between predicted and actual values. It is less sensitive to outliers compared to other metrics like Mean Squared Error (MSE) or Root Mean Squared Error (RMSE), making it more robust when there are extreme values in the data.

Feature known at the time of prediciton that will be helpful in our model include YEAR', 'MONTH', 'U.S._STATE', 'NERC.REGION', 'CLIMATE.REGION' ,'ANOMALY.LEVEL', 'CLIMATE.CATEGORY', 'CAUSE.CATEGORY.DETAIL','OUTAGE.DURATION', 'CUSTOMERS.AFFECTED', 'TOTAL.PRICE', 'TSALES','TOTAL.CUSTOMERS', 'POPULATION', and 'SEASON'.

## Baseline Model
Our baseline model incorporates the month of the outage and the cause category of the power outage to predict its duration and severity. By accurately forecasting these factors, communities can better anticipate and prepare for outages. We selected the month as a feature based on the hypothesis that the duration of power outages varies significantly across different seasons. This relationship, highlighted by the statistical significance of seasonality, suggests that including the month may help capture important seasonal patterns affecting outage duration. As for cause cateogry, certain causes of power outages are more likey to result in longer lasting outages than other cause (i.e Hurricane vs Vandalism).

We first onehotencoded the cause category and then fit the model.

The MAE was 2586 and R^2 was 0.16 indicating that our baseline model did not perform the best. 

## Final Model

## Fairness Analysis
