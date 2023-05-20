# Severe Power Outages in the US 


## Introduction

This site contains an analysis of major power outages in the continental U.S. from January 2000 to July 2016. The data was aquired from 
the following link https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks, and was compiled by Sayanti Mukherjee et. al. 

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

<iframe src="assets/file-name.html" width=800 height=600 frameBorder=0></iframe>


### Bivariate Analysis


### Interesting Aggregates


---

## Assessment of Missingness

## Hypothesis Testing