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


### Head of cleaned dataset
|    | U.S._STATE   |   POPULATION | NERC.REGION   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED | OUTAGE.START        |
|---:|:-------------|-------------:|:--------------|:-------------------|------------------:|---------------------:|:--------------------|
|  0 | Minnesota    |  5.34812e+06 | MRO           | severe weather     |              3060 |                70000 | 2011-07-01 17:00:00 |
|  1 | Minnesota    |  5.45712e+06 | MRO           | intentional attack |                 1 |                  nan | 2014-05-11 18:38:00 |
|  2 | Minnesota    |  5.3109e+06  | MRO           | severe weather     |              3000 |                70000 | 2010-10-26 20:00:00 |
|  3 | Minnesota    |  5.38044e+06 | MRO           | severe weather     |              2550 |                68200 | 2012-06-19 04:30:00 |
|  4 | Minnesota    |  5.48959e+06 | MRO           | severe weather     |              1740 |               250000 | 2015-07-18 02:00:00 |



### Univariate Analysis
For our univariate analysis it first makes sense to look at the distribution of the `OUTAGE.DURATION` looks as this is the focus of our predictive task. 

<iframe 
  src="assets/duration_univariate.html" 
  width="800" 
  height="600" 
  frameborder="0" 
  style="margin: 0; padding: 0;"
  ></iframe>

Based on this plot, we can tell that most of the power outages are in the dataset last less than ten thousand total minutes, however there are some outliers that lasted much longer. After doing some research into the data, we found that there were a few outliers that seemed to be incorrect as upon researching these outages we were unable to find references of these outages online, so we decided to remove them from our data set. 

### Bivariate Analysis
For our bivariate analysis we decided to look at the relationship between outage duration the other columns in our dataset. We started by creating scatter plots between outage duration and every quantitative column in our dataset, to see if there were any noticeable relationships that we could use to better understand how different variables impact outage duration. Of these scatter plots, the most interesting relationship we found was between outage duration and customers affected. We also added a trendline to the plot so that we could better visualize this trend. 

<iframe
  src="assets/duration_vs_customers_affected.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin: 0; padding: 0;"
></iframe>

As we can see from the plot, although the trend is not incredibly strong, there seems to be some positive correlation between the duration of the outage and the number of customers affected. This is important as it gives evidence that the duration of an outage has a result on how the outage impacts the community. We also calculated the correlation coefficient for this relationship so that we have a quantitative measurement of this trend and we found a correlation coefficient of 0.2619. This also indicates that using this feature will be helpful later  when we try to predict the duration of an outage. 

Another relationship that we chose to inspect was how the location of the power outage impacts the duration of the outage. To do this, we used Folium to create a map of the United States and heat mapped areas states by outage duration. 

<iframe
  src="assets/outage_map.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin: 0; padding: 0;"
></iframe>

Based on this plot, it seems that the state the power outage occured has an impact on the duration of the outage. This can be helpful for us to understand qualities of these areas that may impact outage duration, and also suggests it will be helpful for our prediction task as well. 

### Interesting Aggregates
To further investigate the geographic distribution of power outage impacts, we grouped our dataset by CLIMATE.REGION and calculated the average outage duration and the average number of customers affected in each region. This provides insight into how regional factors might influence both the duration of outages and the scale of their impact on customers.

| CLIMATE.REGION     |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |
|:-------------------|------------------:|---------------------:|
| Central            |          2701.13  |             126810   |
| East North Central |          5352.04  |             138389   |
| Northeast          |          2991.66  |             121960   |
| Northwest          |          1284.5   |              81420   |
| South              |          2846.1   |             183501   |
| Southeast          |          2217.69  |             180540   |
| Southwest          |          1566.14  |              39028.9 |
| West               |          1628.33  |             194580   |
| West North Central |           696.562 |              47316   |

By analyzing these regional aggregates, we can better understand which areas are more vulnerable to prolonged outages and which regions experience more widespread customer disruption. Further investigation into the causes of these regional differences can help improve outage management strategies tailored to each area's unique needs.


To analyze the impact of different outage causes on power outage duration and the number of customers affected, we grouped our dataset by the CAUSE.CATEGORY. For each category, we calculated the average outage duration and the average number of customers affected. This approach helps us understand how various causes of power outages influence both the length of the outages and their scale in terms of customer impact

| CAUSE.CATEGORY                |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |
|:------------------------------|------------------:|---------------------:|
| equipment failure             |          1816.91  |        101936        |
| fuel supply emergency         |         13484     |             0.142857 |
| intentional attack            |           429.98  |          1790.53     |
| islanding                     |           200.545 |          6169.09     |
| public appeal                 |          1468.45  |          7618.76     |
| severe weather                |          3883.99  |        188575        |
| system operability disruption |           728.87  |        211066        |

By examining these averages, we gain a clearer picture of how different causes of outages impact communities. Some causes, like severe weather, not only result in longer outages but also affect a significantly larger number of people. In contrast, other causes, such as fuel supply emergencies, appear to be less impactful in terms of customer reach but may still need further validation to address potential data inconsistencies and outliers. 

## Assessment of Missingness

### NMAR Analysis
A columns that is most likely  NMAR is `OUTAGE.START`. The paper that this data comes from states that the data was aquired from a variety of public datasets. In this case it might be possible the time `OUTAGE.RESTORATION` was avaiable from a dataset , but its corresponding `OUTAGE.START` was not hence the NA value. Information we could collect in order for the missigness to determine if missingess Mechanism is MAR is to check the sources used for the collection of `OUTAGE.RESTORATION` (`OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`) and Determine whether some sources less likely to have the correspodinng OUTAGE.START for an `OUTAGE.RESTORATION`. 

### Missingness Dependency
We will be testing the missingness dependency for `DEMAND.LOSS.MW` with respect to `CAUSE.CATEGORY` and `CLIMATE.REGION`.
- **CAUSE.CATEGORY**:
  We will first examine the distribution of `CAUSE.CATEGORY` when `DEMAND.LOSS.MW` is missing vs not missing.

  **Null Hypothesis:**: The distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing vs not missing.

  **Alternative Hypothesis:**: The distribution of `CAUSE.CATEGORY` is different when `DEMAND.LOSS.MW` is missing vs not missing.

<iframe
  src="assets/demand_loss_missingness_Cause_cat.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin: 0; padding: 0;"
></iframe>

  The observed TVD was about 0.18, corresponding to a p-value of 0.0. Consequently, we reject the null hypothesis that the distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing versus not missing. This suggests that the missingness of `DEMAND.LOSS.MW` may be dependent on `CAUSE.CATEGORY`, but we cannot conclusively state that there is a causal relationship.
     
<iframe
  src="assets/EMP1.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin: 0; padding: 0;"
></iframe>

- **CLIMATE.REGION**:
  Now we will examine the distribution of `CLIMATE.REGION` when `DEMAND.LOSS.MW` is missing vs not missing.

  **Null Hypothesis:**: The distribution of `CLIMATE.REGION` is the same when `DEMAND.LOSS.MW` is missing vs not missing.

  **Alternative Hypothesis:**: The distribution of `CLIMATE.REGION` is different when `DEMAND.LOSS.MW` is missing vs not missing.

<iframe
  src="assets/demand_loss_missingness_CLIMATE_cat.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin: 0; padding: 0;"
></iframe>

  The observed TVD was about 0.034, corresponding to a p-value of 0.348. Consequently, we fail to reject the null hypothesis that the distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing versus not missing. This suggests that the missingness of `DEMAND.LOSS.MW` is not dependent on `CAUSE.CATEGORY`, but we cannot conclusively state that there is no causal relationship.

<iframe
  src="assets/EMP2.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin: 0; padding: 0;"
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
  style="margin: 0; padding: 0;"
></iframe>

## Framing a Prediction Problem
Our model aims to predict the duration of an outage, a continuous variable, based on features available at the time of the event. By accurately forecasting outage durations, communities can be better prepared, enabling more efficient response strategies and minimizing disruption to essential services. Since this is a regression problem, we will evaluate our model using two key metrics:

R² : This will help us understand how well our model fits the data. R² measures the proportion of variance in the target variable that is explained by the model.

Mean Absolute Error (MAE): This will be used to quantify the error of the model's predictions. MAE gives us the average absolute difference between predicted and actual values. It is less sensitive to outliers compared to other metrics like Mean Squared Error (MSE) or Root Mean Squared Error (RMSE), making it more robust when there are extreme values in the data.

Features known at the time of prediciton that will be helpful in our model include YEAR', 'MONTH', 'U.S._STATE', 'NERC.REGION', 'CLIMATE.REGION' ,'ANOMALY.LEVEL', 'CLIMATE.CATEGORY', 'CAUSE.CATEGORY.DETAIL', 'TOTAL.PRICE', 'TOTAL.SALES','TOTAL.CUSTOMERS', 'POPULATION', and 'SEASON'.

## Baseline Model
Our baseline model incorporates the month of the outage and the cause category of the power outage to predict its duration and severity. By accurately forecasting these factors, communities can better anticipate and prepare for outages. We selected the month as a feature based on the hypothesis that the duration of power outages varies significantly across different seasons. This relationship, highlighted by the statistical significance of seasonality, suggests that including the month may help capture important seasonal patterns affecting outage duration. As for cause cateogry, certain causes of power outages are more likey to result in longer lasting outages than other cause (i.e Hurricane vs Vandalism).

We first onehotencoded the cause category and then fit the model.

The MAE was 2586 and R^2 was 0.16 indicating that our baseline model did not perform the best. 

## Final Model
For our final model, we wanted to explore what features of our dataset would be useful for our predictive tasks. We started by one hot encoding our categorical variables. Because all of our categories are nominal, we used sklearns OneHotEncoder(). We also needed to make sure that all of our quantitative variables are represented properly as they were initially represented as objects. Some of the features that we incorporated in our final model were `U.S._STATE`, `CLIMATE.REGION`, `ANOMALY.LEVEL`, `CLIMATE.CATEGORY`, `YEAR`, `MONTH`, `CAUSE.CATEGORY.DETAIL`, `TOTAL.PRICE`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`,`POPULATION`,`SEASON`. 

We carefully looked through the work we did at this point in our project to make this decision. For example, in our hypothesis test we found that `SEASON` played a role in outage duration, so we decided to use `YEAR` and `MONTH` as features in our model because these are more granular than season, so we believed that they would be a good indicator of the outage duration. We also included customers affected because in our bivariate analysis we saw that there was some relation between customers affected and outage duration. We also saw that the cause of an outage may play a factor in the duration of an outage, so this was a natural feature for us to use. 

After trying multiple models including multple linear regression, random forests, and gradient boosted decision trees (XGBoost), we found that our best performing model in terms of RMSE and R² on the test set was a random forest. We found the best performing hyperparamters using three cross folds and 180 hyperparameter combinations. We found that the best performing hyperparameters were {'model__max_depth': 20, 'model__min_samples_leaf': 1, 'model__min_samples_split': 5, 'model__n_estimators': 200}. This resulted in a RMSE of  1.93 and an R² score of 0.56. Since we scaled the value of outage duration by log1p before we made our predictions (in order to stabilize the variance given the large range of values for duration), we need to compute the inverse of this if we want to accurately compare it to our basline model. This resulted in a RMSE of 7052.49 and an R² score of 0.19. This is significantly worse than the scaled verion, however we still saw an improvement over our baseline model. 

## Fairness Analysis
For our fariness analysis, we wanted to see if our best model made similarly accurate predictions for states that were on the east coast vs states that were not. From our heatmap in the Bivariate analysis section, it seemed that states on the east coast tended to have longer outage duration than other states, so we wanted to test whether our model would do similarly on a group of our data consisting of just states on the east coast and states not on the east coast. After dividing our data based on these groups, and making predictions using our model we found that the difference in rmse on the predictions between the two groups was -0.01418. This seemed pretty fair, but to make sure we needed to perform a permutation test on our data.

**Null Hypothesis:** Our model is fair. Predictions for outage duration for states on the east coast have roughly the same rmse as predictions for states not on the east coast.

**Alternative Hypothesis:** Our model is not fair. Predictions for outage duration for states on the east coast have different rmse than states not on the east coast. 

To complete our permutation test, we ran 1000 simulations by shuffling the labels and predicting outage duration for the new groups. After running all of the simulations we found a p-value of 0.93. Given this, we fail to reject the null hypothesis that our model is fair.

The figure below show the distribution of the test statistic. 

<iframe
  src="assests/fairness_analysis.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin: 0; padding: 0;"
></iframe>

Given the distributions of the test stastics within it is clear to see that our observed value is reasonable.