---
layout: post
title:      "Time Series Analysis"
date:       2021-03-12 22:17:27 -0500
permalink:  time_series_analysis
---


I want to share with you the process and results of my fourth project for Flatiron School Data Science career.
This Project was particularly challenging for me because I had to stop studying for 2 months after finishing the third module because my second daughter was born. Once I returned to study, everything was much more difficult, because I felt that I forgot a lot of what I already knew. But it was also very motivating because I am that kind of the people who likes to connect and this project gave me the possibility to establish a connection between data and the place where I live, Orlando, FL.

The slogan of the project is to advise a real estate company choosing the 5 best zipcodes to invest. This may sound easy, but when you work with data that varies over time, you have to handle it in a particular way. Further, I have found that qualitative factors can be just as important as quantitative ones. I followed the next steps:

1. Download necessary libraries, open the dataset and make it user friendly. Check for missing and null values. Add more information as needed.
2. Transform the data so we can deal with dates. Melt data for ARIMA.
3. Explore data and define ARIMA function.
4. Obtain the optimal p,d,q parameters for the top ten zipcodes.
5. Define the 5 best zipcodes for investment.

The company that needs our services is a small company with just a few years in the market. They have just the necessary human and financing resources.


The dataset is in a wide format. Let's explore deeper before we transform it into a long format. As a first step, we need to rename the RegionName column, Zipcode seems to be the right name.

It was not clear to me the difference between City and Metro columns. Exploring the number of items per column we can assume that the metro column responds to a bigger area. That is very easy using value_counts(). After this we reduced the dataframe selecting just the zipcodes in Orlando. We have 19 different zipcodes.

The next step is to calculate the Return On Investment or simply ROI. This step will be helpfull because we can filter the zipcodes with the highest ROI. The easiest way is using the first and last date in the format it currently is. At this point I had to do a very important decision. To me, it has a lot of sense to select the 10 zipcodes with the best ROI, that tells me that the price increased over the time.

To deal with dates we need to transform the format into date time using to_datetime().
Melting the values we can plot a very interesting and understandable graph, which basically let us know how the values were moving through the time. From the plot we can assume that prices were increasing until 2007 where they reach their highest point.

![](https://imgur.com/tAHH9xd)

Another interesting way to understand your data is through boxplots or violins.

I made a function in order to evaluate stationarity. The value we have to focus on is P-Value. If our P-Value is <5 we can say that our data is stationary. If it's >5 is not.

```
def ad_test(dataset):
     dftest = adfuller(dataset, autolag = 'AIC')
     print("1. ADF : ",dftest[0])
     print("2. P-Value : ", dftest[1])
     print("3. Num Of Lags : ", dftest[2])
     print("4. Num Of Observations Used For ADF Regression:",      dftest[3])
     print("5. Critical Values :")
     for key, val in dftest[4].items():
         print("\t",key, ": ", val)
```

ARIMA comes from Auto-regression(AR) term, the number of lag periods; Integral(I) term for non-stationary differencing and Moving Average(MA) for error term. Those components are known as p, d and q. This model can handle non-stationary data so there is no need to apply any other technique.

How to apply ARIMA? In this case I needed to evaluate a group so I decided to create one model per each zipcode from our top ten. ARIMA function gives you a list of different parameters and what you need to do is to look for the lowest MSE (Mean Square Error) but the same function tells you which one is the best. Model evaluation could spend a lot of time. 

After this, I made a function with the ARIMA parameters that gives us a two year forecast. 
Then we need to classify the ten zipcodes to take the best 5. This means best possible forecast, best case scenario and worst case scenario.


# Results

32822 is our best zipcode. The forecast for the next two years is +23.29 % of profit. Best case scenario is 50.93 % and worst case scenario is 3.65 %. Even if everything goes wrong we will make a profit. Another very important factor to choose this zipcode is that the area has 24,862 houses but only 21,111 are occupied. We have a great opportunity to invest with a guaranteed return on a very high number of options. One thing to consider is that the median household income is $ 32,903, we need to investigate more about how our target.

![](https://i.imgur.com/kUNM13I.png)

32804 is in second place. The forecast for the next two years is +27.03 % of profit. Best case scenario is +50.38 % and worst case scenario is -1.31 %. Not so bad. There are almost 1,000 available houses and the median household income is $ 58,548

![](https://i.imgur.com/e8SrMD8.png)

32807 is in third place. The forecast for the next two years is +25.02 % of profit. Best case scenario is +54.42 % and worst case scenario is -3.60 %.

![](https://i.imgur.com/CrRCwPU.png)

32817 is in fourth place. The forecast for the next two years is +22.47 % of profit. Best case scenario is +48.31 % and worst case scenario is -3.36 %.

![](https://i.imgur.com/7oLuyKa.png)

The fifth zipcode is 32814. The forecast for the next two years is +16.76 % of profit. Best case scenario is +39.57 % and worst case scenario is -6.05 %. The median household income is $ 96,766 which is a huge opportunity to fix and improve the house in order to increase the price and profit.

![](https://i.imgur.com/0N7EkDy.png)

![](https://i.imgur.com/K4d7OPQ.png)

Thanks for your time!



