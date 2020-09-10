---
layout: post
title:      "Mi third data science project"
date:       2020-09-10 18:33:19 +0000
permalink:  mi_third_data_science_project
---


In this opportunity I am sharing my 3rd project for the data science career at Flatiron School. Our main task is to create some algorithms that can predict when a customer will churn and then choose the right one. So, why did I say, “The right one” instead of “most accurate”? Let me tell you why. 

#### The process:

For this project we followed the OSEMN process, which means Obtain, Scrub, Explore, Model and iNterpret. 

### Obtain

We downloaded a data set from https://www.kaggle.com/becksddf/churn-in-telecoms-dataset:

```
df = pd.read_csv('datasets_2667_4430_bigml_59c28831336c6604c800002a.csv')
df.head(5)
```

Once we are able to visualize the dataset we can start to explore:

```
df.columns
df.info()
df.isnull().sum()
df.describe()
```


It would be better to transform some information in order to make it easier to manipulate. We have some categorical variables and they are binary, so we are going to transform those "yes" and "no" into "0" and "1".


```
df['churn'] = df['churn'].replace([True, False], [1, 0])
df['international plan'] = df['international plan'].replace(['yes','no'],[1,0])
df['voice mail plan'] = df['voice mail plan'].replace(['yes','no'],[1,0])
```

### Explore

We hace 3332 customers in the dataset. From those, 2850 stayed and 483 churned. That means that 14.5% of our customers churned.

![](http://i.imgur.com/wwCYiWj.png)

Loocking at the data we realized that we needed to pay special attention to some features. The first one was Customer Service Calls. This means that the number of times that a customer called the company has a strong relationship with churn. As we can observe in the next chart, as soon as the customer calls more than 3 times, it has more than 50% or chances to churn. Clearly the company is not solving some problems.

![](https://i.imgur.com/7U06ySg.png)

The second feature with mayor importance is customers with International Plans. As we can see in the next chart, more than 40% of customers who have an international plan have churned. This drives us to think that there is an important issue with this product. We don't have enough information about it so we can't dig more on it.

![](https://i.imgur.com/wrcUIeg.png)



Two more important things to say at this point. First, I had to remove some outliers, the sample was not really big so I decided to remove some features outliers upper than 0.99 percentile and under 0.01 percentile.
At the end of the Explore stage I dropped 5.88 % of our dataset to remove outliers.

The second is that I created two new columns. I added the sum of 'total day charge', 'total eve charge', 'total night charge' and 'total intl charge' in one to create 'total charge'.

Then I added the sum of 'total day minutes', 'total eve minutes', 'total night minutes' and 'total intl minutes' inn one to create 'total minutes'.


## Model

To model our algorithms I had to divide the data into two different groups. In one we defined churn as our target and in the other the variables we are going to use to make predictions and classification.

```
target = df['churn']
variables = df[['international plan', 'total day minutes',
                    'total day charge', 'total eve minutes',
                    'total eve charge', 'total night minutes','voice mail plan', 'number vmail messages',
                    'total intl charge', 'customer service calls', 'total charge',
                    'total minutes']]
```


After thar we need to scale features in order to normalize the data and compare and play with variables. We used MinMaxScaler because we removed most of the outliers.

```
scaler=MinMaxScaler()
X = pd.DataFrame(scaler.fit_transform(X),columns=X.columns)
```


Then we preprocessed the data splititting it into training and test set:

```
X_train , X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)
```


## Model

We performed 4 different models and I'll show you my results:



| Random Forest | Decision Trees | Adaboost  | Logistic Regression |
| -------- | -------- | -------- | -------- |
| Score :  0.96     | Score :  0.95      | Score :  0.92    | Score :  0.84     |
| AUC: 0.87     | AUC: 0.82    | AUC: 0.77   | AUC: 0.78  |


After that we are going to compare their confussion matrix:

![](https://i.imgur.com/mLoA2o3.png)


## Interpret

We could think that our random forest model is enough good to use it and classsify but lets put it in the business context. What do we need? I think that we need to predict churn in customers and is better to have a bigger number of false positives in our model. This is so important because the result of this would be to pay more attention to our customers, not bad.
In this case, due to the nature of the problem we decided to choose Logistic Regression as the best algorithm because it gives us a bigger number of false positives, which means the number of observations where the model predicted the customer will churn (1), but they actually not (0).

We have some questions that could give us some more insights:

*	Do we need to offer international service calls plans?
*	Are your customers paying more than they should?
*	Our competitors have better plans?
*	Are you able to solve customer problems quickly?
*	Is our customer service enough good?
*	Do you have issues with International plans?


### Recommendations:

*	We need a better system to track customer service calls, their problems should be fixed as soon as possible.
*	We need to look at our competitors international plans, maybe they are offering better deals. Should we sell international plans knowing that people are using communication apps to make long distance calls?
*	We can offer different prices depending on the necessities of customers

### Future work

We got some insights, but it would be a great idea to get some more information about:

1.	Surveys with information about why the customer churned.
2.	We need to improve our logistic regression algorithm in order to improve accuracy.
3.	Churn rate of our competitors.

