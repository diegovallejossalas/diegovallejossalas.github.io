---
layout: post
title:      "My first Data Sciente Project at Flatiron School"
date:       2020-04-14 15:13:56 +0000
permalink:  my_first_data_sciente_project_at_flatiron_school
---

Tomorrow will be the presentation of my first project for Flatiron School and I would like to make a few comments about it. This is, in short, the most challenging task I have had in recent years. This doesn’t mean that I haven’t been challenged before. However, for this specific project it was very different because I had to test my skills as a student and professional (and this is outside any comfort zone).
Here I will detail some tips that helped me get ahead.

* Save your own code blocks:

If you know that a code block is important and you could use it in the future, then you should save it. We don't always want to create from scratch, it doesn't make much sense because it makes us waste a lot of time.
Here is a long and tedious example:


```
y = dependent
X = independents

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.15)

print(len(X_train), len(X_test), len(y_train), len(y_test))

linreg = LinearRegression()
linreg.fit(X_train, y_train)

y_hat_train = linreg.predict(X_train)
y_hat_test = linreg.predict(X_test)

train_residuals = y_hat_train - y_train
test_residuals = y_hat_test - y_test

train_mse = mean_squared_error(y_train, y_hat_train)
test_mse = mean_squared_error(y_test, y_hat_test)
print('Train Mean Squarred Error:', train_mse)
print('Test Mean Squarred Error:', test_mse)

import random
random.seed(11)

train_err = []
test_err = []
t_sizes = list(range(5,100,5))
for t_size in t_sizes:
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=t_size/100)
    linreg.fit(X_train, y_train)
    y_hat_train = linreg.predict(X_train)
    y_hat_test = linreg.predict(X_test)
    train_err.append(mean_squared_error(y_train, y_hat_train))
    test_err.append(mean_squared_error(y_test, y_hat_test))
plt.scatter(t_sizes, train_err, label='Training Error')
plt.scatter(t_sizes, test_err, label='Testing Error')
plt.legend()
```

We definitively don't want to write that again.

* Restart and Run All

Do this every time you open your notebook. This way you make sure that some of the changes you make are being applied correctly and you will have no surprises in the end. Many times it has happened to me that I add a block far behind where I am currently working and I mess everything up. Then you realize that it is not the notebook, it is you. This option Is in the top menu, in Kernel.
* Make it easy

My friend, don't compare with other students at this point, especially if you don't have a background in statistics or code. We are in a long term learning process and there is a long way to go. Do what you know, what you really know, and if you can go a little further then it's fine. Don't try to impress your instructor by trying to do things you don't know well.

* Order, clarity.

This hint is perhaps one of the most important. My recommendation is to always comment on each piece of code, no matter if it is obvious. Our memory is fragile, especially in stressful situations and we don't want to be left with our minds blank when our instructor asks us "why did you do that?". Definitely not.
Many times we are even forced to review past code blocks and we get horribly entangled because we forget why we did it that way. Believe me, this point really matters.

* Features

Take a look and explore each variable you have in your dataset. Sometimes some variables seem to look like garbage. We think we wouldn't use them. Well, you better stop there. My advice is to wait a little bit if you want to drop a column, at least it has less than 50% of valid information, for example. The deeper you go, the more surprises you will find.
Is not easy to select your variables when you are making a model, you just want to include as much as you can in order to increase your predictable coefficient and this is not always good. Let's try to be more realistic. The best way is explore as much as you can about the topic you are working on. For example, if your dataset is about cars, you should investigate a little about brands, models, engine features, etc. Something else that can give you more knowledge in order to make decisions.

How did I explore?

All begins when you open the file. Use the .head() or .tail() functions to take a look on how the dataset is ordered. Is a good idea to identify a feature that you can use it as an index, something that has a unique value per each element.
.info() function gives you more clarity in order to understand what kind of data we are dealing with. I use this to know the number of rows, columns and the data type.
.describe() also helps us with some categorical data at this point. So you can easily find some outliers, for example.
Then we should check por duplicates with .duplicated().
With .isna().sum() we get the number of null values in the data set ordered per column names. This is pretty useful and easy to read. Then you know wich way you should take in order to clean your data.
If you have missing values you need to make a decision that could change the course of your work. For me is easier when you have a continuous variable, so you can replace values with the mean or median depending on your distribution. For categorical data you need to understand it first in order to make a decision. Sometimes a null or missing value could be a 0 or the median. As I said before, it depends.
Use value_counts() on each variable to understand it better.
Plot, plot any independent variable just to have an idea about how they work on the independent variable.

And now, what else?

At this point you already did the dirty job. I thing the next step is to validate your dataset taking out outliers, applying log transformations or dummies, verifying milticollinearity and dropping some columns that doesn’t make much sense to have them or that are highly correlated with others.

Then make your model work through statsmodels or sklearn and validate it. You can do train-test split or cross validation methods, It depends on you model.

This is not the most effective explanation about how to make your project but sometimes it’s kind of confusing to start from scratch and is really helpfull to have some clear and basic line to follow.

Thanks so much for your time!


