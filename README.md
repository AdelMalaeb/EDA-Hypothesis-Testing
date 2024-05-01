# Scenario

New York City Taxi and Limousine Commission (TLC) is an agency responsible for licensing and regulating New York City's taxi cabs and for-hire vehicles.

The TLC data comes from over 200,000 taxi and limousine licensees, making approximately one million combined trips per day.

In this activity, I will examine the data provided and prepare it for analysis. I will also design a professional data visualization that tells a story, and will help data-driven decisions for TLC.


# Aim

To conduct an extensive explanatory data analysis on a provided data set to identify a possibility of an idea to generate more revenues for TLC.

Once I find the idea, I will continue by examining the statistical significance using hypothesis testing.




# Part 1: Explanatory Data Analysis

## **Import the data and the relevant libraries**


```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import datetime as dt
import seaborn as sns 
from scipy import stats
```


```python
df=pd.read_csv('/Users/adel/Desktop/TLC/2017_Yellow_Taxi_Trip_Data.csv')
```

## **Data exploration and cleaning**

### **a)** Discover the data set

Discovering the data set is very crucial:
    For the following task I have uploaded a link that directs me to     the data base administrator (NYC OpenData). In this link I will find all the relevant information about the source of the data as well as a data dictionary.

[NYC OpenData](https://data.cityofnewyork.us/Transportation/2021-Yellow-Taxi-Trip-Data/m6nq-qud6/about_data)

### **b)** Understand the structure of the data

Functions that help:

* head()
* info() - to print a concise summary of a DataFrame, including the data types of each column, the number of non-null values, and memory usage
* describe() - to generate descriptive statistics of a DataFrame
* groupby()
* sortby()


```python
#display the first 5 rows of the dataset
df.head(5)
```


```python
df.info()
#The info() enabled me to know that I don't have any missing values in the dataset.
df.shape
#The shape enabled me to know that I will be dealing with 22,699 data points with 17 avaliable variables / features.
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 22699 entries, 0 to 22698
    Data columns (total 18 columns):
     #   Column                 Non-Null Count  Dtype  
    ---  ------                 --------------  -----  
     0   Unnamed: 0             22699 non-null  int64  
     1   VendorID               22699 non-null  int64  
     2   tpep_pickup_datetime   22699 non-null  object 
     3   tpep_dropoff_datetime  22699 non-null  object 
     4   passenger_count        22699 non-null  int64  
     5   trip_distance          22699 non-null  float64
     6   RatecodeID             22699 non-null  int64  
     7   store_and_fwd_flag     22699 non-null  object 
     8   PULocationID           22699 non-null  int64  
     9   DOLocationID           22699 non-null  int64  
     10  payment_type           22699 non-null  int64  
     11  fare_amount            22699 non-null  float64
     12  extra                  22699 non-null  float64
     13  mta_tax                22699 non-null  float64
     14  tip_amount             22699 non-null  float64
     15  tolls_amount           22699 non-null  float64
     16  improvement_surcharge  22699 non-null  float64
     17  total_amount           22699 non-null  float64
    dtypes: float64(8), int64(7), object(3)
    memory usage: 3.1+ MB





    (22699, 18)



**Does the following data types for the variable makes sense?**

* VendorID - int64 - makes sense since it represents identification number

* tpep_pickup_datetime - object - dates should be **datetime** objects

* tpep_dropoff_datetime - object -  dates should be **datetime** objects

* fare_amount, extra, mta_tax, tip_amount, tolls_amount, improvement_surcharge, total_amount - float64 - makes sense since all these values represent dollar value

* PULocationID, DOLocationID - int64 - **needs to be investigated!!!!!**

* !!payment_type!! - **int64** - Since it is a type of payment I should expect a string. For ex, Cash or Credit!!! - **needs to be investigated!!!!!**

### **c)** Convert data columns to datetime


```python
df['tpep_pickup_datetime'] = pd.to_datetime(df['tpep_pickup_datetime'])
df['tpep_dropoff_datetime'] = pd.to_datetime(df['tpep_dropoff_datetime'])

# Create a month column
df['month'] = df['tpep_pickup_datetime'].dt.month_name()
# Create a day column
df['day'] = df['tpep_pickup_datetime'].dt.day_name()

```

### **d-1)** Check for missing Values

There is no missing data according to the results from the `info()` function. 

### **d-2)** Check for duplicate values


```python
df.duplicated().sum()
```
The data set contains 0 duplicates

### **d-3)** Check for Outliers

To check for outlier, several methods can be effective:

* **1- Summary Statistics:** Calculate summary statistics such as mean, median, standard deviation, and quartiles (25th and 75th percentiles) to get an overview of the data distribution. Outliers are often defined as observations that fall far from the mean or median.

* **2- Box Plot:** Visualize the data distribution using a box plot (also known as a box-and-whisker plot). Outliers are typically identified as data points that fall outside the "whiskers" of the box plot, which are defined as 1.5 times the interquartile range (IQR) above the 75th percentile or below the 25th percentile.

* **3- Histogram:** Plot a histogram of the data to visualize the frequency distribution. Outliers may appear as data points that are far from the bulk of the data or as isolated peaks in the histogram.

* **4- Scatter Plot:** For datasets with two or more variables, create scatter plots to visualize the relationship between variables. Outliers may appear as data points that deviate significantly from the overall pattern or trend in the scatter plot.

* **5- Z-Score:** Calculate the z-score for each data point, which measures how many standard deviations away from the mean a data point is. Data points with a z-score above a certain threshold (e.g., 3 or -3) may be considered outliers.

* **6- Tukey's Method:** Use Tukey's method, which defines outliers as data points that fall below the first quartile minus 1.5 times the IQR or above the third quartile plus 1.5 times the IQR.

**I am going to use the Box Plot method**

**Results:**
It looks like the column trip distance have significant outliers, therefore, I'm going to investigate trip distance


# Findings

### Trip Distance
The majority of trips were journeys of less than two miles.

### Total amount
It looks like the total cost of each trip also has a right skewed distribution, with most costs falling in the $5-15 range.

### Tip amount
It looks like the tip amount of each trip also has a right skewed distribution, with most amount falling in the $0-4 range.

### Vendor
In New York, taxi services are offered by two vendors: Creative Mobile Technologies, LLC (Vendor 1) and VeriFone Inc. (Vendor 2).

VeriFone Inc contributes more than Creative Mobile Technologies, LLC. However, the difference in the following data set is not very significant.

### Total amount vs Vendor
There are no noticeable difference in the distribution of total amount between the two vendors in the dataset.

### Payment Types
Since the most widely used method of payment is credit card, I assume that payment type 1 referes to credit card and 2 refers to cash. There is still no clear assumptions regarding 3 and 4.

* 1: **credit card**
* 2: **cash**


### **Investigate total amount (Continued)**

Total amount is directly postively correlated with revenues: As total_amount increases, revnues will increase.

Total amount = fare amount + extra + mta_tax + tip_amount + tolls_amount + improvement_surcharge

To closely investigate the total amount we have to know which variable contributes most to the total amount; therefore, I developed the following function that gives me an idea.



I concluded that **Fare amount** contributes the most and therefore has to be investigated.


#### Look for any possible trends in the total amount for the months of 2017
It looks like there is a small trend of negative sloping in the months of July, August and September. However, the trend is not very significant and might be due to several reason taking seasons into consideration. **This will be out of the scope of the project**. *I might have to model that in a time series with much more historical data!*



#### Look for any possible trends in the total amount for the days of 2017
It looks like the lowest days of total amount are Mondays and Sundays. In this case, the Idea of probably increasing the presence of taxi cabs in these days will generate more revenues. However, It doesn't seem like a concrete idea, and thus I decided to continue looking for a better one.


###  Investigate fare amount

#### Examine the relationship between fare amount and trip distance
The majority of the trips falls within [1,2] miles; however, we have long distance trips which is represented after the 3rd Quartile of the box plot 


I noticed that the fare amount of long trips are greater than short trips. However, this is theoratically known since fare amount and trip distance are positively correlated.

So it makes sense for long trips to cost more, and therefore I will **NOT** be able to provide an idea to promote long distances trips because it is not practical in reality.

I know from statistics that -1 < correlation coefficient > 1
* 1: strongly positively correlated
* -1: strongly negatively correlated

The corr between the fare amount and trip distance is 0.7565 which supports my claim in the previous cell.


#### Examine the relationship between fare amount and payment type

Recall that payment type 1: refers to credit card and 2: refers to cash

**Theoratical expectation:** I should Not notice a differnce between fare amount and payment type. So *lets examine that*.

**!!! I didn't expect to see this result!!!** customers who pay with credit cards pays on average more fare amount that customers who pay in cash

##### The IDEA to generate more revenue for TLC

I believe if TLC was able to initiate a marketing campaign to promote credit card payments, this will increase the fare amount of trips.

Given we investigated the relationship between fare amount and total amount, we noticed that fare amount contributes well to the total amount. 

Therefore, if TLC was able to increase fare amount, the total amount will significantly increase and thus the total revenues will increase.

**!!This is a very intersting idea!!**

However, there is a very important Question to ask before implementing this idea.

Is the difference in fare amount between credit cards and cash  **statistically significant** or is it just due to **random variation**

I will examine this claim in PART 2 of this project.



# Part 2: Hypothesis Testing

**Examine my idea through Statistical Analysis**

To perform Statistical analysis, specifically -Hypothesis testing- I have to do random sampling. 

**Note:** For the purpose of this exercise, I will assume that the sample data comes from an experiment in which customers are randomly selected and divided into two groups: 1) customers who are required to pay with credit card, 2) customers who are required to pay with cash. Without this assumption, I cannot draw causal conclusions about how payment method affects fare amount.

In reality, random sampling has a complex methodology and requires experiment designs to be able to subset our population into **representative samples** that eliminates or at the least reduces the possibility of having bias in our estimation results.

**Claim:** I believe if TLC was able to initiate a marketing campaign to promote credit card payments, this will increase the fare amount of trips, thus increases TLC total revenues.

**Goal:** To apply hypothesis testing and analyze whether there is a relationship between payment type and fare amount. For example: discover if customers who use credit cards pay higher fare amounts than customers who use cash.

**Expectations:** If what I claimed is statistically significant; therefore, as a consultant, I have to suggest for TLC to do a marketing campaign to promote credit card payments and increase revenues.

**Question:** Is there a relationship between fare amount and payment type?

## Understand the probability distribution of fare amount

**Using a fast visualization technique - Histogram**
Both ditributions resembles a right-skewed normal distribution - positively skewed

**Use the Normal distribution emperical values to check for normality**


Under this rule:
* **68%** of the data falls within one standard deviation
* **95%** percent within two standard deviations
* **99.7%** within three standard deviations from the mean.

**Checking for Normality in fare values for Credit Card payments**
Interval 95% = 95.958074 - Which is approximately equal

Interval 99.7% = 99.403865 - Which is approximately equal

Interval 68% = 90.540452 - NOT equal; however, this is because majority of the values are on the left curve (right-skwed) as the histogram showed me.

**Checking for Normality in fare values for Credit Card payments**
Interval 95% = 95.958074 - Which is approximately equal

Interval 99.7% = 99.403865 - Which is approximately equal

Interval 68% = 90.540452 - NOT equal; however, this is because majority of the values are on the left curve (right-skwed) as the histogram showed me.

I concluded that the distribution of fare amounts for credit card and cash payments approximately follows a **right-skewed normal distribution**


## Hypothesis Testing

**Assumptions:** The two groups are Independant and the population standard deviation is not known.

Before I conduct the hypothesis test, I should consider the following questions where applicable to complete my code response:

1. Recall the difference between the null hypothesis and the alternative hypotheses. Consider my hypotheses for this project as listed below.

$H_0$: There is no difference in the average fare amount between customers who use credit cards and customers who use cash.

$H_A$: There is a difference in the average fare amount between customers who use credit cards and customers who use cash.

Steps is to conduct a two-sample t-test: 


1.   State the null hypothesis and the alternative hypothesis
2.   Choose a signficance level
3.   Find the p-value
4.   Reject or fail to reject the null hypothesis 

I choose 5% as the significance level and proceed with a two-sample t-test.

* 1- One-sample t-test: Compares the mean of a single sample to a known value or hypothesized mean.
* **2- Two-sample t-test: Determines if there is a significant difference between the means of two independent samples.**
* 3- Paired t-test: Assesses whether the means of two dependent samples differ significantly.
* 4- Chi-square test: Examines the association between categorical variables in a contingency table.
* 5- ANOVA (Analysis of Variance): Tests for differences in means across multiple groups or treatments.
* 6- ANOVA with repeated measures: Determines differences in means across multiple related groups, such as in longitudinal studies.
* 7- Mann-Whitney U test: Nonparametric test for comparing the medians of two independent groups when assumptions of the t-test are not met.
* 8- Wilcoxon signed-rank test: Nonparametric alternative to the paired t-test for comparing the medians of two related groups.
* 9- Kruskal-Wallis test: Nonparametric alternative to one-way ANOVA for comparing the medians of three or more independent groups.
* 10- Fisher's exact test: Determines if there is an association between two categorical variables in a contingency table with small sample sizes.
* 11- Binomial test: Assesses whether the proportion of successes in a sample differs significantly from a hypothesized value.
* 12- Sign test: Nonparametric test for comparing medians or proportions of paired data.
* 13- Z-test: Similar to the t-test but used when the population standard deviation is known.
* 14- Mood's median test: Nonparametric test for comparing the medians of two independent groups.
* 15- Cochran's Q test: Nonparametric test for comparing three or more matched groups when the outcome is dichotomous.
* 16- Friedman test: Nonparametric alternative to repeated measures ANOVA for comparing three or more related groups.
* 17- McNemar's test: Tests for differences in proportions of paired data, often used in before-after studies.
* 18- Logistic regression: Statistical method for analyzing the association between a binary outcome variable and one or more predictor variables.
* 19- Survival analysis: Statistical techniques for analyzing time-to-event data, often used in medical research or reliability analysis.
* 20- Permutation test: Nonparametric test that does not rely on distributional assumptions, often used in situations where traditional tests are not appropriate.


## Conclusion

**Result:** p_value < 0.05 (Significance level) 

$H_0$: There is no difference in the average fare amount between customers who use credit cards and customers who use cash.

**Conclusion:** I reject the Null Hypothesis 

Therefore, the diffrence between the average fare amount between credit card and cash payments is **Statistically significant**



## Business Decision

Given I investigated the relationship between fare amount and total revenue, I noticed that fare amount contributes well to the total revenue.

So, I believe if TLC was able to initiate a marketing capmaign to promote **credit card payments** this will increase the fare amount per single trip, and thus the total revenues will increase.
