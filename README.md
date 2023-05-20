# Severe Power Outages in the US 


## Introduction

This site contains an analysis of major power outages in the continental U.S. from January 2000 to July 2016. The data was aquired from 
[this link](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks), and was compiled by Sayanti Mukherjee et. al. 

The question this analysis is centered around is: <strong> What are characteristics of major power outages? <strong>

More specifically, do the most severe power outages differ in the residential electricity pricing of the area?

The dataset has 1534 rows, and the columns most relevant to me were: 
- `U.S._STATE`
- `CLIMATE.REGION`
- `CLIMATE.CATEGORY`
- `ANOMALY.LEVEL`
- `OUTAGE.START`
- `OUTAGE.RESTORATION`
- `OUTAGE.DUREATION`
- `CAUSE.CATEGORY`
- `CUSTOMERS.AFFECTED`
- `RES.PRICE`
- `POPULATION`

[Data Dictionary](https://www.sciencedirect.com/science/article/pii/S2352340918307182)

---

## Cleaning and EDA

### Data Cleaning

While this dataset did not have any missing data that needed to be NaN, there were a few things to clean. I convered `OUTAGE.START.DATE`  and `OUTAGE.START.TIME` to one pd.Datetime column, and did the same with `OUTAGE.RESTORATION`. I made all the strings uniform in `CAUSE.CATEGORY.DETAIL` by stripping it of spaces and casting it to lowercase. 

Then, I created a refined datset with only the columns I needed, and with `OUTAGE.DURATION` in hours rather than minutes. I defined a new column `ABOVE.50.OUTAGE` to signify if an outage had above 50th percentile duration and above 50th percentile customers affected. Below are the first 5 entries of the refined dataset.


| U.S._STATE   | CLIMATE.REGION     | CLIMATE.CATEGORY   |   ANOMALY.LEVEL | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |   RES.PRICE |   POPULATION | OUTAGE.START        | OUTAGE.RESTORATION   |   PC.REALGSP.STATE | duration        |
|:-------------|:-------------------|:-------------------|----------------:|:-------------------|:------------------------|------------------:|---------------------:|------------:|-------------:|:--------------------|:---------------------|-------------------:|:----------------|
| Minnesota    | East North Central | normal             |            -0.3 | severe weather     | nan                     |        51         |                70000 |       11.6  |      5348119 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |              51268 | 2 days 03:00:00 |
| Minnesota    | East North Central | normal             |            -0.1 | intentional attack | vandalism               |         0.0166667 |                  nan |       12.12 |      5457125 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |              53499 | 0 days 00:01:00 |
| Minnesota    | East North Central | cold               |            -1.5 | severe weather     | heavy wind              |        50         |                70000 |       10.87 |      5310903 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |              50447 | 2 days 02:00:00 |
| Minnesota    | East North Central | normal             |            -0.1 | severe weather     | thunderstorm            |        42.5       |                68200 |       11.79 |      5380443 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |              51598 | 1 days 18:30:00 |
| Minnesota    | East North Central | warm               |             1.2 | severe weather     | nan                     |        29         |               250000 |       13.07 |      5489594 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |              54431 | 1 days 05:00:00 |

### Univariate Analysis

Below is a graph of the total number of customers affected in each state. California appears to have the most affected customers, followed by Texas. This is likely because both these states have large populations. 

<iframe src="assets/customers_state.html" width=800 height=600 frameBorder=0></iframe>


This is a graph of the cumulative outage duration in hours by cause of outage. As we can see, severe weather outages account for the most time. 

<iframe src="assets/duration_cause.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

Below is a plot of customers affected in an outage versus the monthly residential electricity price in the area (USD). This graph does not show a clear relationship - it seems likely that the most severe outages occurr in similarly priced areas as less severe outages.

<iframe src="assets/customers_resprice.html" width=800 height=600 frameBorder=0></iframe>


### Interesting Aggregates

Below are the first ten rows of a table that has the average outage duration per state, as well as number of customers affected and average duration per outage in the state - sorted by average duration. We can see that some states have had very few but very long outages, while others have had many long outages. 


| U.S._STATE           |   ('OUTAGE.DURATION.HRS', 'mean') |   ('OUTAGE.DURATION.HRS', 'sum') |   ('AVG.CUSTOMERS.AFFECTED', 'mean') |   ('TOTAL.OUTAGES', 'count') |   ('AVG.DURATION.PER.OUTAGE', '') |
|:---------------------|----------------------------------:|---------------------------------:|-------------------------------------:|-----------------------------:|----------------------------------:|
| Wisconsin            |                          131.735  |                         2502.97  |                              45876   |                           20 |                          125.148  |
| West Virginia        |                          116.317  |                          465.267 |                             179794   |                            4 |                          116.317  |
| New York             |                          100.583  |                         7040.78  |                             190676   |                           71 |                           99.166  |
| Michigan             |                           88.383  |                         8396.38  |                             152878   |                           95 |                           88.383  |
| Kentucky             |                           84.8987 |                         1103.68  |                             130531   |                           13 |                           84.8987 |
| Iowa                 |                           79.8958 |                          639.167 |                              94000   |                            8 |                           79.8958 |
| Arizona              |                           75.882  |                         1897.05  |                              64402.7 |                           28 |                           67.7518 |
| New Jersey           |                           74.1818 |                         2373.82  |                             160217   |                           35 |                           67.8233 |
| Kansas               |                           72.9381 |                          510.567 |                             108000   |                            9 |                           56.7296 |
| District of Columbia |                           71.7267 |                          717.267 |                             194709   |                           10 |                           71.7267 |


---

## Assessment of Missingness

### NMAR Analysis

A column which I believe to be Not Missing At Random (NMAR) is `OUTAGE.DURATION`. Data in this column could be missing because someone forgot to record the time the outage ended, or was not aware when the outage began. We would need data on both the start and end times of the outage to fill in this missing data. Or, it would be helpful to have a marker of "undetected" outage if the start couldn't be recorded.


### Missingness Dependency

There are quite a few missing values in the `CUSTOMERS.AFFECTED` column. Of those outages where `CUSTOMERS.AFFECTED` was missing, 219 were listed as caused by an "intentional attack". This is 3 times the other listed causes, leading me to believe that the missingness of `CUSTOMERS.AFFECTED` depended on `CAUSE.CATEGORY`.

I performed a permutation test with the null hypothesis that, if `CUSTOMERS.AFFECTED` is MCAR, the distribution of `CAUSE.CATEGORY` would be the same for outages where affected customers are missing and outages where affected customers aren't missing. The test statistic was Total Variation Distance. Below is the plot of the distribution resulting from the permutation test, as well as a red line for the observed test statistic. 

<iframe src="assets/mcar_dist.html" width=800 height=600 frameBorder=0></iframe>

From this, we can confirm that the missingness of `CUSTOMERS.AFFECTED` depends on another column, `CAUSE.CATEGORY`.

## Hypothesis Testing

<strong>Null Hypothesis</strong>: In all outages, the residential electricity price has a similar distribution, and the observed differences in this datset are due to random chance.

<strong>Alternative Hypothesis</strong>: The most severe outages occur in areas with lower residential electricity price, on average. The observed difference is not only due to random chance.

I tested these hypotheses using a permutation test because I want to know if the most severe power outages (those above the median in this dataset) come from the same population as those below the median. 

Here is a plot of the two distributuions. 

<iframe src="assets/hyp_test.html" width=800 height=600 frameBorder=0></iframe>



Test statistic: Difference in Means

Significance Level: p<0.05

P-value: 0.006

The p-value is below the significance level of 0.05, implying that the most severe outages may occur in areas with lower residential electricity price. 
