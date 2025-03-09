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




# Data Cleaning and Exploratory Data Analysis

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis
