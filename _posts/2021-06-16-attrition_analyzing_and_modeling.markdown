---
layout: post
title:      "Attrition: Analyzing and Modeling "
date:       2021-06-16 00:09:35 -0400
permalink:  attrition_analyzing_and_modeling
---



For this project I am using the HR Analytics Case Study dataset that contains information of a fictitious company with around 4400 employees. This project brings an exhaustive exploration and analysis of the variables that can affect attrition and models to predict it.

The data includes 5 datasets but for this project I am using 3. The first one includes information about education, salaries, gender, marital status, years at company and more. The second and third datasets are surveys. One of them is about job satisfaction and the other one is about performance. 

As I always do, all the libraries go together at the beginning. They basically are Pandas, Numpy, Matplotlib, Seaborn and Sklearn for modeling. 

## Obtaining and Scrubbing

This dataset gives us a dictionary where we can find an explanation, so this makes us save some time searching in the web.

Looking at the missing data I decided to use the mode to replace the missing values (‘Numcompaniesworked’, ‘TotalWorkingYears’, ‘EnvironmentSatisfaction’, ‘JobSatisfaction’ and ‘WorkLifeBalance ‘) because they will not make a significant difference compared to the next highest value and we are dealing with integers.

## Exploring Data

Once I replaced the missing values, I merged the data frames. After this I created histograms for every numerical variable to have an approach with the distribution of values and understand a bit more about them. Following this line, I could easily drop some variables that have no value for our work.

After that, to understand the scenario and analyze our problem I created data frames per each variable splitting the total number of employees by staying in the company or not and its respective percentage. Then I used different kinds of visualizations like bars, boxplots, violin plots, etc. depending on each variable.

In some cases when my experience in Human Resources field tells me to explore deeper, I created visualizations combining some variables specially to explain how some correlations work. This is extremely useful for recommendations.

Here I want to mention just some of the variables that have a mayor impact direct or indirectly on attrition.

### Attrition

This is our target variable, and the first action is to transform values from object to numerical and then I got some insights about it. 16% of employees leaved the company last year. This number could be tricky when you try to understand it because it may vary between different variables. When we talk about attrition, we necessarily talk about money that leaks out, is a hidden cost, hard to track and hard to explain too. That is why sometimes companies explain its cost with time invested on training or customer service quality.
It is part of the exploration to compare variables by attrition and we must pay attention on those whose have an attrition percentage considerable upper or lower than 16.

![](https://i.imgur.com/AUwnYwp.png)

### Exploring Job Involvement

For this variable we can assume that those employees with low qualification tend to get fired and a higher attrition level is normal. When the punctuation is 4 and the attrition is high, we have a problem, we can assume that we are losing good employees.
Why almost 18% of employees that rated 4 in this variable leaved the company?

### Exploring Work Life Balance

This variable is interesting because attrition for employees who rated 1 is 31% and for other values is almost the half. We need to pay attention on Research and Development department because the bulk of workers are in that area so, each percentage point causes a greater impact.

### Exploring Training

We can see that the attrition decreases dramatically when the employee has 6 trainings. The result of our algorithms is going to tell us how important this feature is, but to me there is missing a key variable: 'kind of training', so we can figure out if the company provides internal and/or external trainings and the cost of this. We must remind that attrition has a hidden cost.

### Exploring Years with Current Manager

It seems like the first year with a new manager is critical for attrition. Around 30% of those employees leave the company.
That makes us think about the impact in employee’s attrition when a manager leaves the company.

![](https://i.imgur.com/s030PK5.png)

### Exploring Age

To get a better sense about how age can affect attrition I decided to add a column that divides ages by groups. This visualization helped me to confirm that attrition’s level is higher on younger employees.

![](https://i.imgur.com/pSXE9N6.png)

### Exploring Business Travel category

We can clearly see that employees that travel frequently have a higher percentage of attrition.

![](https://i.imgur.com/zypyyiT.png)

### Exploring Department

The company has 3 departments. Human resources attrition’s percentage is double compared to the other departments. Although HR team is not big, its attrition may impact directly on our target.

### Exploring Marital Status

Attrition on single employees is considerable higher than married and divorced.

### Exploring Total Working Years

Employees with less worked years tend to quit more than those who have more experience.

![](https://i.imgur.com/ZrPaR8X.png)

### Exploring Years At Company

Employees with less worked years tend to quit more than those who have less.

### Exploring Percent Salary Hike

As you can see, employees who recieved the highest percent salary hike leaved more than others. This make me think if the company has a good plan to retain talent.

![](https://i.imgur.com/Upt5gG0.png)



After analyze each variable I dropped 'EmployeeCount', 'StandardHours', 'PerformanceRating', 'StockOptionLevel', 'Ages', 'Income' and 'Over18'.

Once the exploration work is done, I created dummy variables for categorical. We cannot forget to drop those columns, if not, the model is going to take it twice.

## Modeling

First step, as we know, we separate our target variable (Attrition) in a different data frame called “y”, the rest of variables are saved in a data frame called “X”. Once this is done, we scale our “X” data frame, this means that all the values are transformed in a 0 to 1 scale. Standard Scaler was used just for logistic regression, in models like Random Forest Classifier is not necessary.

This is the most important step of modeling: Choose your model.

### Results

* Logistic Regression Score :  0.84
* Random Forest Score :  0.93
* Decision Trees Score :  0.86
* Adaboost Score :  0.84

Looking at the results I am selecting Random Forest Classifier for its 93% accuracy, but I am also interested in how this model could impact on medium - long term. This is not just about who is prone to leave. To me, this is more about how the company should pay more attention on hiring and retainment staff. That is the reason why I am choosing Random Forest Classifier as the best model for this case. The model predicts the higher number of true positives compared with the other models. It also predicts that 65 are going to leave the company but they are actually not. This is called a False Positive and, in this case, for this company, means that those employees could have a high risk to leave, it is like a yellow flag to track them. Here I ask myself: Can the Human Resources department handle or support other departments with this? They have the highest percentage of attrition, and it seems that HR employees are not happy working there, I mean, HR have serious problems to solve first.

![](https://i.imgur.com/gKhgXPE.png)



Before I finish, I want to thank you for the time you spent reading this. I would love any feedback and if you are interested In the Human Resources world too or have any question, feel free to contact me on LinkedIn [](https://www.linkedin.com/in/diegoval/).


