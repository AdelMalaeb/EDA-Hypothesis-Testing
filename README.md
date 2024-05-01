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
    
<iframe src="/Users/adel/Desktop/Yellow_Taxi_Trip_Data_Data_Dictionary.xlsx" width="100%" height="500px"></iframe>

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




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>VendorID</th>
      <th>tpep_pickup_datetime</th>
      <th>tpep_dropoff_datetime</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>RatecodeID</th>
      <th>store_and_fwd_flag</th>
      <th>PULocationID</th>
      <th>DOLocationID</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>24870114</td>
      <td>2</td>
      <td>03/25/2017 8:55:43 AM</td>
      <td>03/25/2017 9:09:47 AM</td>
      <td>6</td>
      <td>3.34</td>
      <td>1</td>
      <td>N</td>
      <td>100</td>
      <td>231</td>
      <td>1</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.76</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>16.56</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35634249</td>
      <td>1</td>
      <td>04/11/2017 2:53:28 PM</td>
      <td>04/11/2017 3:19:58 PM</td>
      <td>1</td>
      <td>1.80</td>
      <td>1</td>
      <td>N</td>
      <td>186</td>
      <td>43</td>
      <td>1</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>4.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>20.80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106203690</td>
      <td>1</td>
      <td>12/15/2017 7:26:56 AM</td>
      <td>12/15/2017 7:34:08 AM</td>
      <td>1</td>
      <td>1.00</td>
      <td>1</td>
      <td>N</td>
      <td>262</td>
      <td>236</td>
      <td>1</td>
      <td>6.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>1.45</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>8.75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38942136</td>
      <td>2</td>
      <td>05/07/2017 1:17:59 PM</td>
      <td>05/07/2017 1:48:14 PM</td>
      <td>1</td>
      <td>3.70</td>
      <td>1</td>
      <td>N</td>
      <td>188</td>
      <td>97</td>
      <td>1</td>
      <td>20.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>6.39</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>27.69</td>
    </tr>
    <tr>
      <th>4</th>
      <td>30841670</td>
      <td>2</td>
      <td>04/15/2017 11:32:20 PM</td>
      <td>04/15/2017 11:49:03 PM</td>
      <td>1</td>
      <td>4.37</td>
      <td>1</td>
      <td>N</td>
      <td>4</td>
      <td>112</td>
      <td>2</td>
      <td>16.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.80</td>
    </tr>
  </tbody>
</table>
</div>




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


df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>VendorID</th>
      <th>tpep_pickup_datetime</th>
      <th>tpep_dropoff_datetime</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>RatecodeID</th>
      <th>store_and_fwd_flag</th>
      <th>PULocationID</th>
      <th>DOLocationID</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
      <th>month</th>
      <th>day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>24870114</td>
      <td>2</td>
      <td>2017-03-25 08:55:43</td>
      <td>2017-03-25 09:09:47</td>
      <td>6</td>
      <td>3.34</td>
      <td>1</td>
      <td>N</td>
      <td>100</td>
      <td>231</td>
      <td>1</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.76</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>16.56</td>
      <td>March</td>
      <td>Saturday</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35634249</td>
      <td>1</td>
      <td>2017-04-11 14:53:28</td>
      <td>2017-04-11 15:19:58</td>
      <td>1</td>
      <td>1.80</td>
      <td>1</td>
      <td>N</td>
      <td>186</td>
      <td>43</td>
      <td>1</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>4.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>20.80</td>
      <td>April</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106203690</td>
      <td>1</td>
      <td>2017-12-15 07:26:56</td>
      <td>2017-12-15 07:34:08</td>
      <td>1</td>
      <td>1.00</td>
      <td>1</td>
      <td>N</td>
      <td>262</td>
      <td>236</td>
      <td>1</td>
      <td>6.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>1.45</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>8.75</td>
      <td>December</td>
      <td>Friday</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38942136</td>
      <td>2</td>
      <td>2017-05-07 13:17:59</td>
      <td>2017-05-07 13:48:14</td>
      <td>1</td>
      <td>3.70</td>
      <td>1</td>
      <td>N</td>
      <td>188</td>
      <td>97</td>
      <td>1</td>
      <td>20.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>6.39</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>27.69</td>
      <td>May</td>
      <td>Sunday</td>
    </tr>
    <tr>
      <th>4</th>
      <td>30841670</td>
      <td>2</td>
      <td>2017-04-15 23:32:20</td>
      <td>2017-04-15 23:49:03</td>
      <td>1</td>
      <td>4.37</td>
      <td>1</td>
      <td>N</td>
      <td>4</td>
      <td>112</td>
      <td>2</td>
      <td>16.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.80</td>
      <td>April</td>
      <td>Saturday</td>
    </tr>
  </tbody>
</table>
</div>



### **d-1)** Check for missing Values

There is no missing data according to the results from the `info()` function. 

### **d-2)** Check for duplicate values


```python
df.duplicated().sum()
```




    0



The data set contains 0 duplicates

### **d-3)** Check for Outliers

To check for outlier, several methods can be effective:

* **1- Summary Statistics:** Calculate summary statistics such as mean, median, standard deviation, and quartiles (25th and 75th percentiles) to get an overview of the data distribution. Outliers are often defined as observations that fall far from the mean or median.

* **2- Box Plot:** Visualize the data distribution using a box plot (also known as a box-and-whisker plot). Outliers are typically identified as data points that fall outside the "whiskers" of the box plot, which are defined as 1.5 times the interquartile range (IQR) above the 75th percentile or below the 25th percentile.

* **3- Histogram:** Plot a histogram of the data to visualize the frequency distribution. Outliers may appear as data points that are far from the bulk of the data or as isolated peaks in the histogram.

* **4- Scatter Plot:** For datasets with two or more variables, create scatter plots to visualize the relationship between variables. Outliers may appear as data points that deviate significantly from the overall pattern or trend in the scatter plot.

* **5- Z-Score:** Calculate the z-score for each data point, which measures how many standard deviations away from the mean a data point is. Data points with a z-score above a certain threshold (e.g., 3 or -3) may be considered outliers.

* **6- Tukey's Method:** Use Tukey's method, which defines outliers as data points that fall below the first quartile minus 1.5 times the IQR or above the third quartile plus 1.5 times the IQR.

**Box Plot**


```python
# Specify the columns I want to include in the box plot
columns = ['trip_distance', 'fare_amount', 'extra','mta_tax','tip_amount','tolls_amount','total_amount'] 

# Set up figure and axes configuration
fig, axes = plt.subplots(nrows=len(columns), ncols=1, figsize=(25, 10))

# Create box plots for multiple variables
for i, column in enumerate(columns):
    sns.boxplot(x=df[column], ax=axes[i])
    axes[i].set_title(column)  # Set title for each subplot

# Customize axes labels and ticks


plt.tight_layout()

plt.savefig('box plots.pdf')
```


    
![png](output_22_0.png)
    


It looks like the column trip distance have significant outliers, therefore, I'm going to investigate trip distance


```python
df['trip_distance'].describe()
```




    count    22699.000000
    mean         2.913313
    std          3.653171
    min          0.000000
    25%          0.990000
    50%          1.610000
    75%          3.060000
    max         33.960000
    Name: trip_distance, dtype: float64



### **e)** Investigate the relevant variables 

### Trip Distance


```python
# Create histogram of trip_distance
plt.figure(figsize=(10,5))
sns.histplot(df['trip_distance'], bins=range(-5,40,1))
plt.title('Trip distance histogram');

plt.savefig('Trip distance histogram.pdf')
```


    
![png](output_27_0.png)
    


The majority of trips were journeys of less than two miles.

### Total amount


```python
# Create histogram of total_amount
plt.figure(figsize=(12,6))
ax = sns.histplot(df['total_amount'], bins=range(-20,150,5))
ax.set_xticks(range(-10,101,5))
ax.set_xticklabels(range(-10,101,5))
plt.title('Total amount histogram');

plt.savefig('Total amount histogram.pdf')
```


    
![png](output_30_0.png)
    


It looks like the total cost of each trip also has a right skewed distribution, with most costs falling in the $5-15 range.

### Tip amount


```python
# Create histogram of tip_amount
plt.figure(figsize=(12,6))
ax = sns.histplot(df['tip_amount'], bins=range(-5,25,1))
ax.set_xticks(range(0,21,2))
ax.set_xticklabels(range(0,21,2))
plt.title('Tip amount histogram');

plt.savefig('Tip amount histogram.pdf')
```


    
![png](output_33_0.png)
    


It looks like the tip amount of each trip also has a right skewed distribution, with most amount falling in the $0-4 range.

### Vendor

In New York, taxi services are offered by two vendors: Creative Mobile Technologies, LLC (Vendor 1) and VeriFone Inc. (Vendor 2).


```python
# Count occurrences of each label
label_counts = df['VendorID'].value_counts()

# Plotting the pie chart
plt.figure(figsize=(6, 6))
plt.pie(label_counts, labels=label_counts.index, autopct='%1.1f%%', startangle=90)

# Add a title
plt.title('Pie Chart for Vendor Proportions')

plt.savefig('Pie Chart for Vendor Proportions 1.pdf')
# Show the plot
plt.show()



```


    
![png](output_36_0.png)
    


VeriFone Inc contributes more than Creative Mobile Technologies, LLC. However, the difference in the following data set is not very significant.

### Total amount vs Vendor


```python
# Create histogram of tip_amount by vendor
plt.figure(figsize=(12,7))
ax = sns.histplot(data=df, x='total_amount', bins=range(0,21,1), 
                  hue='VendorID', 
                  multiple='stack',
                  palette='pastel')
ax.set_xticks(range(0,21,1))
ax.set_xticklabels(range(0,21,1))
plt.title('Total amount by vendor histogram');

plt.savefig('Total amount by vendor histogram.pdf')
```


    
![png](output_39_0.png)
    


There are no noticeable difference in the distribution of total amount between the two vendors in the dataset.

### Payment Types


```python
payment_types = df["payment_type"].value_counts()
payment_types.index
```




    Int64Index([1, 2, 3, 4], dtype='int64')




```python
# Create a bar plot of total rides per month
plt.figure(figsize=(12,7))
sns.barplot(x=payment_types.index, y=payment_types)

plt.xlabel("Payment Type")
plt.ylabel("Count")
plt.title("Count of Payment types")

plt.savefig('Count of Payment types 1.pdf')
plt.show()


```


    
![png](output_43_0.png)
    


Since the most widely used method of payment is credit card, I assume that payment type 1 referes to credit card and 2 refers to cash. There is still no clear assumptions regarding 3 and 4.

* 1: **credit card**
* 2: **cash**


### **Investigate total amount (Continued)**

Total amount is directly postively correlated with revenues: As total_amount increases, revnues will increase.

Total amount = fare amount + extra + mta_tax + tip_amount + tolls_amount + improvement_surcharge

To closely investigate the total amount we have to know which variable contributes most to the total amount; therefore, I developed the following function that gives me an idea.


```python

def ratio_amounts_to_total_amount (y,x):
    '''
     This functions is created to check the contribution of amounts to the total_amount
     '''
# Initialize an empty list to store the ratios
    ratios = []

# Iterate over the data points
    for i in range(len(y)):
    # Check if the denominator is not zero to avoid division by zero error
        if  y[i] != 0:
            ratio = (x[i] / y[i])*100
            ratios.append(ratio)
        else:
        # Handle division by zero error
            ratios.append(float('inf'))
    return ratios  


```


```python
ratio_amounts_to_total_amount(df["total_amount"], df["fare_amount"])
```




    [78.50241545893721,
     76.92307692307692,
     74.28571428571429,
     74.03394727338389,
     92.69662921348313,
     72.81553398058253,
     80.29073698444896,
     81.71603677221655,
     91.83673469387755,
     78.54984894259819,
     77.91327913279133,
     71.31102578167855,
     74.44168734491315,
     73.30246913580247,
     76.58643326039387,
     83.33333333333333,
     72.8862973760933,
     92.23300970873785,
     73.52941176470588,
     71.875,
     83.33333333333334,
     70.49891540130152,
     70.51282051282051,
     67.6328502415459,
     72.8476821192053,
     66.17038875103391,
     75.80174927113703,
     87.3015873015873,
     77.73851590106007,
     71.82618064284432,
     77.37656595431098,
     91.83673469387755,
     96.88995215311006,
     74.69879518072288,
     80.76923076923077,
     87.96296296296295,
     86.20689655172414,
     77.91327913279133,
     72.46376811594205,
     56.390977443609025,
     70.05899705014748,
     76.59574468085107,
     85.4922279792746,
     71.27192982456141,
     61.274509803921575,
     66.14785992217898,
     92.92035398230087,
     80.03048780487805,
     89.43089430894308,
     67.82945736434108,
     96.95817490494296,
     85.36585365853658,
     95.62841530054644,
     59.523809523809526,
     95.23809523809523,
     80.12820512820512,
     76.16707616707616,
     68.07351940095303,
     89.74358974358975,
     65.31204644412192,
     57.47126436781609,
     75.75757575757575,
     96.3302752293578,
     98.62385321100918,
     71.94244604316546,
     84.90566037735849,
     81.0185185185185,
     75.75757575757575,
     75.34246575342466,
     75.34246575342466,
     92.92035398230087,
     83.33333333333334,
     68.4931506849315,
     68.4931506849315,
     73.52941176470588,
     71.7948717948718,
     83.25404376784016,
     73.92996108949417,
     80.03048780487805,
     86.02150537634408,
     76.16487455197132,
     71.30509939498704,
     89.04109589041096,
     71.68458781362007,
     81.63265306122447,
     84.90566037735849,
     73.52941176470588,
     77.4134790528233,
     69.26406926406926,
     94.42724458204336,
     95.62841530054644,
     77.66990291262135,
     71.35016465422613,
     88.95705521472392,
     73.86363636363636,
     73.37526205450735,
     83.33333333333334,
     73.30246913580247,
     81.46399055489965,
     84.33734939759036,
     90.9090909090909,
     76.77067855373947,
     75.5287009063444,
     81.14035087719299,
     96.0591133004926,
     93.98496240601504,
     81.63265306122447,
     94.20289855072464,
     94.86166007905138,
     91.21621621621621,
     98.48484848484848,
     80.47210300429184,
     87.96296296296295,
     64.65517241379311,
     89.74358974358975,
     69.19374247894103,
     78.3132530120482,
     84.33734939759036,
     67.48466257668711,
     76.60687593423019,
     87.37864077669903,
     87.4125874125874,
     89.84375,
     80.0,
     80.4093567251462,
     85.9375,
     57.6923076923077,
     77.43362831858407,
     98.52216748768473,
     80.88235294117648,
     77.60532150776052,
     66.88963210702342,
     94.20289855072464,
     76.92307692307693,
     69.10569105691057,
     73.86363636363636,
     88.49557522123894,
     97.22222222222221,
     70.75471698113208,
     68.57142857142857,
     93.4959349593496,
     74.28571428571429,
     80.88235294117648,
     85.22727272727272,
     66.66666666666666,
     72.91666666666667,
     72.72727272727273,
     93.22033898305084,
     88.23529411764706,
     70.86614173228347,
     78.32080200501252,
     66.13756613756614,
     85.75770733825445,
     79.36507936507937,
     83.6734693877551,
     94.96124031007753,
     73.93051966695377,
     72.46376811594203,
     84.24908424908425,
     71.89542483660131,
     89.28571428571428,
     98.48484848484848,
     75.23510971786834,
     93.75,
     79.58801498127342,
     77.57092198581562,
     78.3132530120482,
     74.15254237288136,
     79.36507936507937,
     78.82882882882882,
     77.58620689655173,
     93.59605911330048,
     96.49122807017544,
     78.71720116618076,
     98.89349930843707,
     65.32663316582915,
     86.01485148514851,
     61.274509803921575,
     78.50241545893721,
     66.99947451392538,
     70.33248081841433,
     73.86363636363636,
     76.51715039577837,
     71.66123778501628,
     80.47210300429184,
     92.23300970873785,
     76.72634271099744,
     87.4125874125874,
     87.99171842650104,
     77.23577235772358,
     61.22448979591836,
     75.53956834532374,
     77.05779334500875,
     89.74358974358975,
     69.44444444444444,
     90.36144578313252,
     80.8080808080808,
     65.24184476940383,
     59.4059405940594,
     68.18181818181817,
     96.41255605381166,
     90.57971014492753,
     92.10526315789474,
     76.92307692307692,
     73.1981981981982,
     64.65517241379311,
     75.26881720430107,
     89.74358974358975,
     80.76923076923077,
     81.69934640522875,
     78.65168539325842,
     84.33734939759036,
     87.71929824561403,
     71.09004739336491,
     75.75757575757575,
     86.95652173913044,
     82.52427184466019,
     77.43362831858407,
     77.91327913279133,
     91.77215189873418,
     82.13859020310635,
     95.95959595959596,
     75.37688442211056,
     68.4931506849315,
     80.64516129032258,
     83.33333333333334,
     74.8663101604278,
     76.59574468085107,
     86.20689655172414,
     64.81481481481481,
     69.44444444444446,
     80.84577114427861,
     80.50847457627118,
     75.75757575757575,
     93.75,
     68.02721088435374,
     74.36260623229462,
     75.47169811320755,
     96.15384615384615,
     70.92198581560284,
     74.78632478632478,
     87.3015873015873,
     76.19321449108682,
     96.3302752293578,
     65.66604127579737,
     83.33333333333333,
     65.78947368421053,
     98.48484848484848,
     79.36507936507937,
     75.55406312961719,
     82.19178082191782,
     92.0245398773006,
     93.75,
     91.39784946236558,
     66.28787878787878,
     71.42857142857143,
     77.1604938271605,
     90.9090909090909,
     90.36144578313252,
     86.02150537634408,
     68.96551724137932,
     84.33734939759036,
     96.95817490494296,
     86.73469387755102,
     75.3012048192771,
     96.3302752293578,
     87.3015873015873,
     77.1604938271605,
     68.18181818181817,
     94.5945945945946,
     95.85492227979275,
     83.33333333333334,
     71.74887892376681,
     79.24335378323109,
     74.07407407407408,
     71.74172977281785,
     80.16208597603946,
     80.37383177570094,
     78.43137254901961,
     89.43089430894308,
     64.51612903225806,
     87.3015873015873,
     81.9327731092437,
     66.13756613756614,
     76.92307692307692,
     91.83673469387755,
     79.11392405063292,
     75.0,
     85.22727272727272,
     80.88235294117648,
     90.42553191489361,
     58.139534883720934,
     74.78632478632478,
     81.1541929666366,
     61.64383561643836,
     79.05138339920948,
     89.04109589041096,
     78.32080200501252,
     68.4931506849315,
     78.17589576547232,
     84.90566037735849,
     82.52427184466019,
     67.82945736434108,
     70.27027027027027,
     83.69098712446352,
     67.43737957610789,
     77.91327913279133,
     69.3407296778323,
     96.70781893004114,
     72.3404255319149,
     72.3404255319149,
     81.3953488372093,
     72.75132275132276,
     89.74358974358975,
     65.78947368421053,
     85.22727272727272,
     82.27848101265823,
     74.07407407407408,
     89.74358974358975,
     65.78947368421053,
     84.7457627118644,
     66.66666666666666,
     97.31543624161073,
     75.75757575757575,
     93.22033898305084,
     84.03361344537815,
     79.36507936507937,
     72.27891156462584,
     70.8215297450425,
     89.74358974358975,
     76.53061224489795,
     72.46376811594203,
     79.24335378323109,
     93.4959349593496,
     83.33333333333333,
     92.92035398230087,
     59.523809523809526,
     77.22007722007721,
     68.2704811443433,
     80.88235294117648,
     84.7457627118644,
     67.40595781691671,
     74.28571428571429,
     77.58620689655173,
     87.60683760683762,
     78.96505376344085,
     72.3404255319149,
     89.55223880597015,
     79.7872340425532,
     74.70946319867183,
     76.89556509298998,
     94.9367088607595,
     74.28571428571429,
     74.20494699646642,
     59.97001499250375,
     95.0920245398773,
     97.5378787878788,
     92.92035398230087,
     80.51529790660226,
     62.893081761006286,
     86.20689655172414,
     92.59259259259258,
     88.60759493670885,
     75.82938388625593,
     91.83673469387755,
     92.59259259259258,
     93.15589353612167,
     68.02721088435374,
     73.22175732217573,
     84.54810495626823,
     72.91666666666667,
     90.36144578313252,
     90.36144578313252,
     71.83908045977012,
     70.75471698113208,
     81.25,
     76.01351351351352,
     83.33333333333334,
     78.125,
     71.03825136612022,
     61.34969325153374,
     81.25,
     88.98305084745762,
     71.27192982456141,
     89.04109589041096,
     82.78145695364239,
     80.95952023988005,
     78.38479809976246,
     82.74984086569064,
     69.44444444444444,
     76.53061224489795,
     69.5187165775401,
     89.04109589041096,
     72.75132275132276,
     74.78632478632478,
     79.51807228915662,
     78.99159663865547,
     68.4931506849315,
     82.52427184466019,
     86.89024390243904,
     81.08108108108108,
     76.51715039577837,
     66.39004149377593,
     75.37688442211056,
     76.01351351351352,
     76.01351351351352,
     88.79781420765028,
     82.03125,
     93.22033898305084,
     69.93006993006993,
     83.33333333333334,
     90.9090909090909,
     81.63265306122447,
     71.94244604316546,
     82.38501659554291,
     73.52941176470588,
     81.3953488372093,
     71.94244604316546,
     68.4931506849315,
     86.02150537634408,
     67.48466257668711,
     72.72727272727273,
     72.91666666666667,
     85.22727272727272,
     25.0,
     66.3129973474801,
     83.33333333333334,
     90.36144578313252,
     72.29612492770387,
     69.67213114754098,
     76.72955974842768,
     74.8663101604278,
     96.96261682242991,
     77.10843373493977,
     75.75757575757575,
     64.51612903225806,
     77.66990291262135,
     69.56521739130434,
     80.14796547472257,
     75.67567567567568,
     90.9090909090909,
     74.15254237288136,
     83.59133126934985,
     89.04109589041096,
     72.46376811594203,
     60.76388888888889,
     84.070796460177,
     79.28388746803068,
     79.24335378323109,
     88.79781420765028,
     85.22727272727272,
     76.51715039577837,
     70.79646017699115,
     73.52941176470587,
     75.18796992481202,
     76.92307692307693,
     77.27272727272727,
     92.89617486338797,
     84.50704225352112,
     75.34246575342466,
     70.75471698113208,
     90.36144578313252,
     71.02272727272727,
     77.91327913279133,
     89.74358974358975,
     70.11070110701107,
     89.8876404494382,
     86.02150537634408,
     88.79781420765028,
     76.92307692307693,
     81.4176245210728,
     73.74631268436578,
     84.070796460177,
     67.20430107526882,
     76.92307692307693,
     85.22727272727272,
     75.26881720430107,
     81.46067415730337,
     77.23577235772358,
     87.3015873015873,
     89.04109589041096,
     91.39784946236558,
     92.69662921348313,
     66.66666666666666,
     80.69828722002636,
     75.34246575342466,
     78.94736842105263,
     77.1604938271605,
     91.21621621621621,
     84.90566037735849,
     62.5,
     75.3012048192771,
     75.18796992481202,
     76.92307692307693,
     78.7037037037037,
     78.50241545893721,
     62.15469613259669,
     96.7741935483871,
     86.20689655172414,
     74.28571428571429,
     87.3015873015873,
     64.33007985803017,
     93.22033898305084,
     71.42857142857143,
     73.61963190184049,
     90.9090909090909,
     74.8663101604278,
     71.8562874251497,
     89.74358974358975,
     79.36507936507937,
     74.96251874062969,
     61.53846153846154,
     85.4416333777186,
     97.01492537313433,
     79.54545454545455,
     97.79614325068871,
     75.23510971786834,
     83.33333333333334,
     71.42857142857143,
     66.35071090047393,
     89.96539792387543,
     77.48260594560405,
     74.46808510638297,
     77.9467680608365,
     84.36213991769547,
     60.76388888888889,
     79.6359499431172,
     83.33333333333334,
     76.53061224489795,
     95.3757225433526,
     91.83673469387755,
     87.96296296296295,
     78.67132867132867,
     76.40449438202246,
     62.217194570135746,
     86.46616541353383,
     93.08510638297872,
     66.22516556291392,
     77.96610169491525,
     89.04109589041096,
     89.04109589041096,
     91.83673469387755,
     88.23529411764706,
     89.04109589041096,
     78.54984894259819,
     81.44796380090497,
     72.27891156462584,
     91.39784946236558,
     73.52941176470587,
     76.0233918128655,
     89.59537572254335,
     94.20289855072464,
     81.69934640522875,
     68.18181818181817,
     80.43875685557586,
     78.3132530120482,
     82.19178082191782,
     71.1864406779661,
     80.69828722002636,
     92.59259259259258,
     61.72839506172839,
     77.96610169491525,
     78.2472613458529,
     89.0625,
     76.01351351351352,
     62.893081761006286,
     93.22033898305084,
     76.86084142394822,
     92.59259259259258,
     82.07070707070707,
     71.875,
     76.92307692307692,
     74.00028461647929,
     91.39784946236558,
     70.37037037037037,
     84.90566037735849,
     88.23529411764706,
     71.875,
     75.34246575342466,
     73.74631268436578,
     66.13756613756614,
     87.37864077669903,
     94.20289855072464,
     68.18181818181817,
     89.04109589041096,
     89.04109589041096,
     74.00028461647929,
     80.64516129032258,
     78.50241545893721,
     75.37688442211056,
     77.38095238095238,
     94.4055944055944,
     73.39449541284402,
     93.4959349593496,
     77.25321888412017,
     76.51715039577837,
     73.86363636363636,
     78.67132867132867,
     77.38095238095238,
     79.64601769911503,
     73.52941176470588,
     74.20091324200914,
     70.17543859649122,
     89.74358974358975,
     57.47126436781609,
     82.6086956521739,
     81.08108108108108,
     60.86956521739131,
     73.74631268436578,
     92.5925925925926,
     75.37688442211056,
     81.30081300813008,
     71.42857142857143,
     90.9090909090909,
     95.50561797752809,
     89.74358974358975,
     94.9367088607595,
     82.19178082191782,
     73.30246913580247,
     79.54545454545455,
     74.8663101604278,
     75.3012048192771,
     79.47019867549669,
     92.23300970873785,
     86.20689655172414,
     92.92035398230087,
     95.74468085106382,
     79.91360691144709,
     88.98305084745762,
     79.21419518377694,
     80.88235294117648,
     95.74468085106382,
     79.36507936507937,
     80.64516129032258,
     73.41772151898735,
     86.02150537634408,
     62.893081761006286,
     85.22727272727272,
     71.02272727272727,
     94.5945945945946,
     79.11392405063292,
     82.82208588957054,
     71.05263157894737,
     76.15894039735099,
     83.33333333333333,
     77.43362831858407,
     91.74311926605505,
     36.76470588235294,
     87.37864077669903,
     72.8476821192053,
     79.52008928571428,
     77.22007722007721,
     92.69662921348313,
     65.1890482398957,
     88.29825187299322,
     62.5,
     78.07807807807808,
     84.90566037735849,
     78.36990595611285,
     71.09004739336491,
     80.61749571183535,
     89.8876404494382,
     66.13756613756614,
     89.74358974358975,
     76.92307692307693,
     78.50241545893721,
     92.59259259259258,
     84.33734939759036,
     84.90566037735849,
     72.75132275132276,
     93.75,
     57.23006486074017,
     68.41236140599199,
     90.9090909090909,
     93.75,
     89.28571428571428,
     79.01907356948227,
     74.78632478632478,
     70.28112449799197,
     75.34246575342466,
     85.8085808580858,
     79.28388746803068,
     82.52427184466019,
     71.83908045977012,
     97.70114942528735,
     71.94244604316546,
     78.7037037037037,
     95.62841530054644,
     78.75457875457876,
     77.66990291262135,
     89.04109589041096,
     80.27522935779817,
     74.00028461647929,
     86.20689655172414,
     77.91327913279133,
     91.39784946236558,
     78.65168539325842,
     75.42147293700089,
     90.36144578313252,
     77.11621233859398,
     70.86614173228347,
     90.9090909090909,
     66.71899529042386,
     75.75757575757575,
     73.33333333333333,
     77.66990291262135,
     88.23529411764706,
     75.82938388625593,
     87.3015873015873,
     73.30246913580247,
     74.52574525745258,
     90.36144578313252,
     77.77777777777779,
     74.8663101604278,
     71.74887892376681,
     60.86956521739131,
     71.27192982456141,
     80.73280546498991,
     91.83673469387755,
     68.72852233676976,
     69.51466127401416,
     78.3132530120482,
     72.46376811594205,
     78.94736842105263,
     86.80555555555556,
     76.92307692307693,
     79.87910189982729,
     86.46616541353383,
     94.75218658892129,
     88.23529411764706,
     69.89247311827957,
     78.125,
     84.90566037735849,
     63.45177664974619,
     76.53061224489795,
     76.86084142394822,
     87.37864077669903,
     94.17040358744394,
     87.3015873015873,
     92.92035398230087,
     87.96296296296295,
     92.59259259259258,
     83.57113738279656,
     77.25321888412017,
     75.63025210084034,
     82.82208588957054,
     82.19178082191782,
     96.41255605381166,
     90.9090909090909,
     95.0920245398773,
     65.47619047619048,
     71.13821138211382,
     68.2704811443433,
     76.23318385650224,
     78.94736842105263,
     85.83690987124464,
     64.74820143884892,
     83.66533864541832,
     68.4931506849315,
     94.20289855072464,
     79.62529274004683,
     76.86084142394822,
     76.53061224489795,
     74.44168734491315,
     78.50241545893721,
     96.24413145539906,
     74.20494699646642,
     93.98496240601504,
     62.78538812785388,
     70.58823529411765,
     84.7457627118644,
     78.54984894259819,
     72.11538461538461,
     68.57142857142857,
     73.35907335907336,
     75.75757575757575,
     76.86084142394822,
     66.26506024096385,
     77.86195286195286,
     74.20091324200914,
     85.22727272727272,
     80.64516129032258,
     78.78787878787878,
     69.44444444444446,
     90.22556390977444,
     80.88235294117648,
     94.20289855072464,
     79.54545454545455,
     79.54545454545455,
     92.10526315789474,
     82.19178082191782,
     75.18796992481202,
     61.53846153846154,
     91.39784946236558,
     93.89671361502347,
     90.9090909090909,
     63.22444678609062,
     75.32281205164993,
     79.11392405063292,
     60.97560975609756,
     77.58620689655173,
     93.22033898305084,
     84.7457627118644,
     73.52941176470588,
     76.92307692307693,
     74.20091324200914,
     74.20091324200914,
     78.53881278538812,
     93.98496240601504,
     75.3012048192771,
     80.10012515644556,
     74.78632478632478,
     83.8150289017341,
     89.74358974358975,
     78.96505376344085,
     83.69098712446352,
     86.17832283725555,
     93.98496240601504,
     74.00028461647929,
     70.51282051282051,
     84.26966292134831,
     71.94244604316546,
     73.46938775510205,
     80.74935400516796,
     68.02721088435374,
     83.33333333333333,
     66.13756613756614,
     72.91666666666667,
     84.7457627118644,
     64.1648843087692,
     72.28915662650601,
     75.3012048192771,
     90.36144578313252,
     94.5945945945946,
     88.95705521472392,
     78.78787878787878,
     74.20091324200914,
     72.41551523222907,
     94.77124183006535,
     74.20494699646642,
     88.79781420765028,
     93.75,
     76.92307692307693,
     77.89473684210526,
     86.02150537634408,
     78.36990595611285,
     71.7948717948718,
     68.18181818181819,
     73.74631268436578,
     67.87330316742081,
     85.9375,
     91.50326797385621,
     72.72727272727273,
     68.96551724137932,
     72.78481012658227,
     86.02150537634408,
     95.3757225433526,
     72.8476821192053,
     93.4959349593496,
     84.90566037735849,
     79.7872340425532,
     80.545229244114,
     83.33333333333334,
     79.36507936507937,
     73.89162561576354,
     82.3045267489712,
     90.9090909090909,
     79.11392405063292,
     88.08290155440415,
     73.52941176470587,
     94.15584415584415,
     88.23529411764706,
     65.93406593406593,
     70.86614173228347,
     46.29629629629629,
     83.33333333333334,
     84.33734939759036,
     74.57627118644068,
     95.74468085106382,
     71.27192982456141,
     91.39784946236558,
     98.48484848484848,
     55.55555555555556,
     72.03389830508475,
     92.10526315789474,
     63.670411985018724,
     79.47976878612715,
     75.34246575342466,
     81.3953488372093,
     72.27891156462584,
     96.41255605381166,
     92.59259259259258,
     74.86979166666667,
     77.1604938271605,
     75.75757575757575,
     93.59605911330048,
     88.60759493670885,
     92.26190476190476,
     78.36391437308869,
     69.91051454138703,
     70.35175879396985,
     73.86363636363636,
     90.22556390977444,
     68.77022653721683,
     93.98496240601504,
     82.71077908217717,
     74.32432432432432,
     78.02874743326488,
     82.26390259953932,
     86.02150537634408,
     64.74820143884892,
     70.37037037037037,
     94.4206008583691,
     77.58620689655173,
     77.31958762886597,
     93.98496240601504,
     77.35352205398289,
     78.48443843031122,
     84.33734939759036,
     84.90566037735849,
     65.78947368421053,
     79.51807228915662,
     78.94736842105263,
     62.857142857142854,
     93.08510638297872,
     80.64516129032258,
     68.57142857142857,
     93.4959349593496,
     84.7457627118644,
     78.78151260504202,
     81.63265306122447,
     66.66666666666666,
     75.21058965102286,
     80.64516129032258,
     78.89546351084812,
     70.53941908713692,
     89.74358974358975,
     82.84023668639054,
     82.21476510067114,
     91.21621621621621,
     72.72727272727273,
     93.26424870466322,
     69.10569105691057,
     80.16032064128257,
     84.90566037735849,
     70.05899705014748,
     66.3129973474801,
     75.0,
     73.52941176470587,
     73.61963190184049,
     74.86979166666667,
     91.39784946236558,
     84.90566037735849,
     71.66123778501628,
     81.46067415730337,
     77.10843373493977,
     68.2704811443433,
     75.75757575757575,
     98.48484848484848,
     77.7537796976242,
     70.06369426751593,
     92.48554913294798,
     76.47058823529412,
     77.43362831858407,
     76.92307692307693,
     66.66666666666666,
     94.77124183006535,
     86.73469387755102,
     70.48872180451127,
     88.23529411764706,
     74.27510355663476,
     66.22516556291392,
     69.44444444444444,
     78.03790412486065,
     70.60669456066945,
     89.74358974358975,
     80.20344287949922,
     94.5945945945946,
     83.33333333333334,
     84.33734939759036,
     69.56521739130434,
     75.21058965102286,
     80.88235294117648,
     73.52941176470588,
     52.21407771864644,
     68.47871892119679,
     85.22727272727272,
     94.20289855072464,
     75.26881720430107,
     72.1830985915493,
     79.11392405063292,
     60.3448275862069,
     93.75,
     66.13756613756614,
     71.09004739336491,
     67.48466257668711,
     78.32080200501252,
     77.60532150776052,
     75.36231884057972,
     67.43737957610789,
     ...]



After running this function on all the amount variables, I concluded that **Fare amount** contributes the most and therefore has to be investigated.


#### Look for any possible trends in the total amount for the months of 2017



```python
# Reorder the monthly ride list so months go in order
month_order = ['January', 'February', 'March', 'April', 'May', 'June', 'July',
         'August', 'September', 'October', 'November', 'December']

total_amount_month = df.groupby('month').agg({"total_amount": "sum"})
total_amount_month = total_amount_month.reindex(index=month_order)
total_amount_month
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_amount</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>January</th>
      <td>31735.25</td>
    </tr>
    <tr>
      <th>February</th>
      <td>28937.89</td>
    </tr>
    <tr>
      <th>March</th>
      <td>33085.89</td>
    </tr>
    <tr>
      <th>April</th>
      <td>32012.54</td>
    </tr>
    <tr>
      <th>May</th>
      <td>33828.58</td>
    </tr>
    <tr>
      <th>June</th>
      <td>32920.52</td>
    </tr>
    <tr>
      <th>July</th>
      <td>26617.64</td>
    </tr>
    <tr>
      <th>August</th>
      <td>27759.56</td>
    </tr>
    <tr>
      <th>September</th>
      <td>28206.38</td>
    </tr>
    <tr>
      <th>October</th>
      <td>33065.83</td>
    </tr>
    <tr>
      <th>November</th>
      <td>30800.44</td>
    </tr>
    <tr>
      <th>December</th>
      <td>31261.57</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a bar plot of total amount by month
plt.figure(figsize=(12,7))
sns.barplot(x=total_amount_month.index, y=total_amount_month['total_amount'])
sns.lineplot(x=total_amount_month.index, y=total_amount_month['total_amount'], color='black', marker='o')

plt.title('Total amount by month', fontsize=16)
plt.xlabel("Month's Name")
plt.ylabel("Total amount");

plt.savefig('Total amount by month.pdf')
```


    
![png](output_51_0.png)
    


It looks like there is a small trend of negative sloping in the months of July, August and September. However, the trend is not very significant and might be due to several reason taking seasons into consideration. **This will be out of the scope of the project**. *I might have to model that in a time series with much more historical data!*


```python
# Reorder the day column
month_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
total_day_amount = df.groupby('day').agg({'total_amount': 'sum'})
total_day_amount = total_day_amount.reindex(index =month_order )
total_day_amount


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_amount</th>
    </tr>
    <tr>
      <th>day</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Monday</th>
      <td>49574.37</td>
    </tr>
    <tr>
      <th>Tuesday</th>
      <td>52527.14</td>
    </tr>
    <tr>
      <th>Wednesday</th>
      <td>55310.47</td>
    </tr>
    <tr>
      <th>Thursday</th>
      <td>57181.91</td>
    </tr>
    <tr>
      <th>Friday</th>
      <td>55818.74</td>
    </tr>
    <tr>
      <th>Saturday</th>
      <td>51195.40</td>
    </tr>
    <tr>
      <th>Sunday</th>
      <td>48624.06</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a bar plot of total amount by day
plt.figure(figsize=(12,7))
sns.barplot(x=total_day_amount.index, y=total_day_amount['total_amount'])
sns.lineplot(x=total_day_amount.index, y=total_day_amount['total_amount'], color='black', marker='o')

plt.title('Total amount by day', fontsize=16)
plt.xlabel("Day")
plt.ylabel("Total amount");

plt.savefig('Total amount by day.pdf')
```


    
![png](output_54_0.png)
    


It looks like the lowest days of total amount are Mondays and Sundays. In this case, the Idea of probably increasing the presence of taxi cabs in these days will generate more revenues. However, It doesn't seem like a concrete idea, and thus I decided to continue looking for a better one.

###  Investigate fare amount

#### Examine the relationship between fare amount and trip distance



```python
#Check for outliers in trip distance to have an idea about the distance
# Create box plot of trip_distance
plt.figure(figsize=(7,2))
plt.title('trip_distance')
sns.boxplot(data=None, x=df['trip_distance'], fliersize=1);

```


    
![png](output_57_0.png)
    


The majority of the trips falls within [1,2] miles; however, we have long distance trips which is represented after the 3rd Quartile of the box plot 


```python
short_trips= (df[(df['trip_distance'] > 0) & (df['trip_distance'] <= 2)])
short_trips_fare_mean = short_trips['fare_amount'].mean()

long_trips = df[df['trip_distance'] > 2]
long_trips_fare_mean = long_trips['fare_amount'].mean()

print("The avg fare amount of short trips is: {}".format(short_trips_fare_mean))
print("The avg fare amount of long trips is: {}".format(long_trips_fare_mean))

```

    The avg fare amount of short trips is: 7.449375835934017
    The avg fare amount of long trips is: 21.116728252501925


I noticed that the fare amount of long trips are greater than short trips. However, this is theoratically known since fare amount and trip distance are positively correlated.

So it makes sense for long trips to cost more, and therefore I will **NOT** be able to provide an idea to promote long distances trips because it is not practical in reality.


```python
# Compute the correlation coefficient between trip distance and fare amount
correlation_coefficient = df['trip_distance'].corr(df['fare_amount'])
print("correlation_coefficient is: {}".format(correlation_coefficient))

```

    correlation_coefficient is: 0.7565989842423747


I know from statistics that -1 < correlation coefficient > 1
* 1: strongly positively correlated
* -1: strongly negatively correlated

The corr between the fare amount and trip distance is 0.7565 which supports my claim in the previous cell.

#### Examine the relationship between fare amount and payment type

Recall that payment type 1: refers to credit card and 2: refers to cash

**Theoratical expectation:** I should Not notice a differnce between fare amount and payment type. So *lets examine that*.


```python
df_CC = df[df['payment_type'] == 1]
df_CC_fare = df_CC['fare_amount'].mean()

df_cash = df[df['payment_type'] == 2]
df_cash_fare = df_cash['fare_amount'].mean()

print("The mean fare value of credit card is: {}".format(df_CC_fare))
print("The mean fare value of cash is: {}".format(df_cash_fare))

```

    The mean fare value of credit card is: 13.429747789059942
    The mean fare value of cash is: 12.21354616760699


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


```python
CC_hist = pd.DataFrame(data = {"fare_amount" : df_CC['fare_amount']})

cash_hist = pd.DataFrame(data = {"fare_amount" : df_cash['fare_amount']})
```


```python
# Create a histogram using Seaborn
sns.histplot(CC_hist, bins=1000)  # bins: number of bins, kde: whether to plot a kernel density estimate

# Customize x-axis
tick_step = 5
x_ticks = np.arange(0, 1000, tick_step)  # Generate a range of tick locations with step 0.5
plt.xticks(x_ticks)

plt.xlim(0, 100)  # Set limits of x-axis from 0 to 60

plt.title('Histogram of fare amount of Credit Cards')
plt.xlabel('Values')
plt.ylabel('Frequency')

plt.savefig('Histogram of fare amount of Credit Cards.pdf')
plt.show()
```


    
![png](output_72_0.png)
    



```python
# Create a histogram using Seaborn
sns.histplot(cash_hist, bins=1000)  # bins: number of bins, kde: whether to plot a kernel density estimate

# Customize x-axis
tick_step = 5
x_ticks = np.arange(0, 1000, tick_step)  # Generate a range of tick locations with step 0.5
plt.xticks(x_ticks)

plt.xlim(0, 100)  # Set limits of x-axis from 0 to 60

plt.title('Histogram of fare amount of Cash')
plt.xlabel('Values')
plt.ylabel('Frequency')

plt.savefig('Histogram of fare amount of Cash.pdf')
plt.show()
```


    
![png](output_73_0.png)
    


Both ditributions resembles a right-skewed normal distribution - positively skewed

**Use the Normal distribution emperical values to check for normality**



```python
def emperical_normal (df,num):
    
    mean = df.mean()
    std = df.std()
    print("mean is: {}".format(mean))
    print("Std is: {}".format(std))
    
    lower_limit = mean - num * std
    upper_limit = mean + num * std
    print("lower_limit is: {}" .format(lower_limit))
    print("upper_limit is: {}" .format(upper_limit))
    
    emperical_dist = ((df >= lower_limit) & (df <= upper_limit)).mean()*100
    return emperical_dist
```


```python
import warnings
warnings.filterwarnings("ignore", category=FutureWarning)
    
def emperical_normal (df):
    
    num = [1,2,3]
    emperical_df = pd.DataFrame()
    
    for i in num:
    
        mean = df.mean()
        std = df.std()
    
        lower_limit = mean - i * std
        upper_limit = mean + i * std
    
        emperical_dist = ((df >= lower_limit) & (df <= upper_limit)).mean()*100
    
        emperical_dict = {"Mean": mean,
                 "Std": std,
                 "lower_limit": lower_limit,
                 "upper_limit": upper_limit,
                 "Distribution percentage %":emperical_dist}
        
        
        index = [f'Interval {68 if i==1 else 95 if i==2 else 99.7}'"%".format(i)]  #Using list of comprehension
    
            
        emperical_df = emperical_df.append(pd.DataFrame(emperical_dict, index=index))
        
 
        
    return emperical_df
```

Under this rule:
* **68%** of the data falls within one standard deviation
* **95%** percent within two standard deviations
* **99.7%** within three standard deviations from the mean.

**Checking for Normality in fare values for Credit Card payments**


```python
emperical_normal(df_CC['fare_amount'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mean</th>
      <th>Std</th>
      <th>lower_limit</th>
      <th>upper_limit</th>
      <th>Distribution percentage %</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Interval 68%</th>
      <td>13.429748</td>
      <td>13.848964</td>
      <td>-0.419216</td>
      <td>27.278711</td>
      <td>90.540452</td>
    </tr>
    <tr>
      <th>Interval 95%</th>
      <td>13.429748</td>
      <td>13.848964</td>
      <td>-14.268179</td>
      <td>41.127675</td>
      <td>95.958074</td>
    </tr>
    <tr>
      <th>Interval 99.7%</th>
      <td>13.429748</td>
      <td>13.848964</td>
      <td>-28.117143</td>
      <td>54.976638</td>
      <td>99.403865</td>
    </tr>
  </tbody>
</table>
</div>



Interval 95% = 95.958074 - Which is approximately equal

Interval 99.7% = 99.403865 - Which is approximately equal

Interval 68% = 90.540452 - NOT equal; however, this is because majority of the values are on the left curve (right-skwed) as the histogram showed me.

**Checking for Normality in fare values for Credit Card payments**


```python
emperical_normal(df_cash['fare_amount'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mean</th>
      <th>Std</th>
      <th>lower_limit</th>
      <th>upper_limit</th>
      <th>Distribution percentage %</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Interval 68%</th>
      <td>12.213546</td>
      <td>11.68994</td>
      <td>0.523606</td>
      <td>23.903486</td>
      <td>90.477501</td>
    </tr>
    <tr>
      <th>Interval 95%</th>
      <td>12.213546</td>
      <td>11.68994</td>
      <td>-11.166334</td>
      <td>35.593426</td>
      <td>95.569011</td>
    </tr>
    <tr>
      <th>Interval 99.7%</th>
      <td>12.213546</td>
      <td>11.68994</td>
      <td>-22.856274</td>
      <td>47.283367</td>
      <td>97.289115</td>
    </tr>
  </tbody>
</table>
</div>



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


```python
sig_level = 0.05

statistic, p_value = stats.ttest_ind(df_CC["fare_amount"], df_cash["fare_amount"], equal_var=False)
```


```python
p_value
```




    6.797387473030518e-12



## Conclusion

**Result:** p_value < 0.05 (Significance level) 

$H_0$: There is no difference in the average fare amount between customers who use credit cards and customers who use cash.

**Conclusion:** I reject the Null Hypothesis 

Therefore, the diffrence between the average fare amount between credit card and cash payments is **Statistically significant**





## Business Decision

Given I investigated the relationship between fare amount and total revenue, I noticed that fare amount contributes well to the total revenue.

So, I believe if TLC was able to initiate a marketing capmaign to promote **credit card payments** this will increase the fare amount per single trip, and thus the total revenues will increase.


```python

```
