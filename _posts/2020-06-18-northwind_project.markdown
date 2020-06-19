---
layout: post
title:      "Northwind Project"
date:       2020-06-19 03:03:27 +0000
permalink:  northwind_project
---


Hi there, this is my second project for data science career at Flatiron school. For this project we were working with the Northwind database that contains information from a fictional company.

For this project we need to query a database to get the data and perform a statistical analysis. There we need to define a hypothesis, this includes to define a null and alternative hypothesis. Why?

When scientists do research and investigate about certain phenomena, they can't assume deliberately  i.e. that A causes B. They instead create a null hypothesis that states exactly the opposite they thing would happen. So the main point here is to reject that null hypothesis with a certain level of confidence.

In order to be successful in our project, we need to respond the next question:

Does discount amount have a statistically significant effect on the quantity of a product in an order? If so, at what level(s) of discount?¶

So we need to define our hypothesis as:

H0 = There is no significant difference in quantity of products in orders when discounts are applied.
H1 = There is a significant difference in quantity of products in orders when discounts are applied.

And then we need to test  another 3 different hypothesis that we choose. It's important that that information would be useful for the company.

The first step is to download the necessary libreries:

```
import sqlite3 as sql
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import numpy as np
import seaborn as sns
import scipy.stats as stats
import statsmodels.api as sm
from statsmodels.formula.api import ols
```

Then let's query the tables using this lines of code:

```
conn = sql.connect('Northwind_small.sqlite')
cur = conn.cursor()
#The selected table is OrderDetail because the discount info is there.
cur.execute("""SELECT OrderID, ProductID, ProductName, CategoryID, 
               UnitPrice, Quantity, Discount
               FROM Product
               JOIN OrderDetail
               USING(UnitPrice)""")
# Let's put the info in a df.
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df.head()
```

As a necessary step we need to check our data in order to find null or missing values. Also to check for outliers it's a good idea. All this is going to make our job easier in the next steps.

To make possible our task we need to divide our universe in two samples, the first sample includes al the orders that had a discount and the second sample is composed of all the orders with no discount in them. The reason behing this is because in order to test our hypothesis we need to have an experimental and a control group.
Experimental group is composed of discounted orders because we need to contrast the result against a control group with has no discounts in it and see the differences.

At this point is important to check normality to make a decision for the next steps.

```
print(stats.normaltest(experimental))
print(stats.normaltest(control))
```

In this case our experimental sample doesn't have a normal distribution but loocking at the graph is not bad. We can deal with that.

Then, we decided to compare the mean and standard deviation just to have a better idea of the samples. 

Experimental group = Mean 25.8      Std 7.3
Control group =          Mean 21.7       Std 6.0

```
difference_in_means = abs(experimental.mean() - control.mean())
difference_in_means
```

The means of samples differs from each other by 4.10. At this point we know that there is a difference but we need to use some statistical tests to explore more in depth.

First, we want to know how different are both samples. The effect size tells us that and and we can use Cohen's d. This is one of the most common ways to measure effect size

We can assume how different are our samples using this rule of "rule of thumb":

Small effect = 0.2

Medium Effect = 0.5

Large Effect = 0.8

Our result gave us a 0.6 so we can assume that our samples have a medium effect size.

Our next step is to test the null hypothesis following the next steps:

1. When sizes and sample variances between the two groups are not equal, Welch's t-test is generally a more reliable test. 
2. Get the degrees of freedom in order to get our P-value.
3. Get the P-Value.

Wait, what is that P-value? The P-value tells you that the result is not due to randomness.in other words, if you get a 2% that means that there is a 2% of probabilities that you rejected your null hypothesis when it is true. When we want to test our hypothesis we need to set an alpha, wich is usually 5% when is a one tail test or 2.5% in a two tail test. This alpha means that there is less than 5% chance (in this case) that the results are due to randomness.

So, you could say that you reject your null hypothesis when your p-value is under your alpha value.

In this case, our P-Value is  0.00011889999780123617so we can reject our null hypothesis.

But this is not all. we need to know if there is a statistical significance between discounts. In order to answer our question we did this.

Wich level of discount has a mayor impact in orders quantity?¶

In this case out hypothesis are:

H0: There is no significant difference in effect of different discounts over quantities of products ordered.
H1: There is a significant difference in effect of different discounts over quantities of products ordered

In this case we need to divide the sample by amount of discount. After taking a look on the mean and compare with each other amount of discount we have to assure that this result is not due to randomness so in this case it would be appropiate to use ANOVA test to compare different groups. Basically, you’re testing groups to see if there’s a difference between them. 
In this case out P-value is 1.47 so we can not reject our null hypothesis. Due to this, we can assume that there is no significant difference in effect of different discounts over quantities of products ordered.

Our 3 next questions are:

Is there a significant difference in profit when discounts are applied?

* H0 = There is no significant difference in Profit in orders when discounts are applied.
* H1 = There is a significant difference in profit in orders when discounts are applied.

Steps:

1. Checking normality
2. Check means and standard deviation 
3. Compare means
4. Cohen's d for effect size (low size effect 0.12)
5. Welch's T test (0.7734188190551299) 
6. Degrees of freedom (146.318783787519)
7. P-Value = 0.22

We couldn't reject the null hypothesis so we can assume that profit is not being affected significatively by discounts.



Is there a significant difference in profit between customer's countries?

* H0: There is no significant difference in profit mean of products purchased by region¶
* H1: There is a significant difference in profit mean of products purchased by region

Steps:

1. Plot means per category
2. Aplying ANOVA.
3. P-Value = 0.000003

We rejected the null hypothesis so we can assume that there is a significant differente in profit by countries.



Is there a significant difference in quantity of orders per month?

H0: there is no significant difference in quantity of products purchased by month
H1: there is a significant difference in quantity of products purchased by month

Steps:

1. Plot means per category
2. Aplying ANOVA.
3. P-Value = 0.039229

we rejected the null hypothesis so we can assume that there is a significant difference in quantity of orders per month.

Our results lead us to have a better overview of the business and how it is currently working, this in order to improve the company's profit, which is always the most important point in a business.

Thanks for yout time.




