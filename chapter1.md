--- 
title       : "Modeling for Causal Inference"
description : "This chapter will introduce you to modeling for causal inference"

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:812b575a73
## The Basics of Modeling Behavior
*** =video_link
//player.vimeo.com/video/231746347

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:09bccc78e5
## Discrete Choice Analysis
*** =video_link
//player.vimeo.com/video/231746571

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:4dfa681543
## Examples of Discrete Choice Analysis 
Which of the following are examples where we might apply discrete choice analysis?

A) Learning the effect of a new cancer drug on life expectancy
B) Learning whether giving people informational brochures affects whether they take prescription drugs as prescribed, such as whether they take antibiotics for the entire duration of the prescription or whether they stop early once they feel that symptoms have disappeared.
C) Learning whether a certain plant thrives under warmer or colder weather.
D) Learning the effect of the income tax on whether people participate in the labor market or not.
*** =instructions
- A
- B
- C
- D
- A and C
- B and D
*** =sct
```{r}
msg1 = “Try again“
msg2 = “Try again”
msg3 = “Try again“
msg4 = "In (A) and (C) we are still interested in causal effects, but now there are no people making decisions, so discrete choice analysis (usually) will not apply."
msg4 = "(B) and (D). These are both situations where a person is making a discrete choice. In (B) they are decided whether to take the drug as prescribed. In (D) they are deciding whether to participate in the labor market. Hence we can use discrete choice analysis to study the causal effects of various policies. In (A) and (C) we are still interested in causal effects, but now there are no people making decisions, so discrete choice analysis (usually) will not apply."
test_mc(correct = 5, feedback_msgs = c(msg1,msg2,msg3,msg4,msg5))
```

--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:
## Kids Love Ice Cream: Part 1 - Data Discovery
You have been newly hired as a data analyst for ConsumerCore Inc., and your very first assignment is to go through a small dataset to learn about food consumption trends in Gluttown, Michigan. The topic is way too complicated to do a good experiment on, but you decide to look at something fun in their data, and you see that there is information about the consumption of ice cream by 20 families in the town.  You start by finding out how much ice cream a household consume in a year by using the dataset `icecream` on 20 households.The data dictionary is below.

Data Dictionary:
	#consumption: ice cream consumption (in pints)
	#children: number of children in the family 
	#age: average age of the children 

 
*** =instructions
- 1) Get a quick sense of the variable names and variable types 
- 2) show the descriptive statistics of `consumption`, `children`, and `age`. 
- 3) What's the maximum number of children in a family for this sample?

*** =pre_exercise_code
```{r}
set.seed(1234)
children <- round(rnorm(20,3,4),0)
children[children<0] <-0
age <- round(rnorm(20,7,5),0)
age[age<0] <-1
u <- rnorm(20,0,5)
consumption <- 100 + 4*children-3*age+u
icecream <- data.frame(consumption, children, age)
```

*** =hint
- Use the `summary` function to generate a table of summary statistics about `icecream`.


*** =sample_code
```{r}
#
# 1) First, let’s take a quick glance at how the data is formatted. We can do this by looking at the first 6 observations with the head() command, so insert the name of our dataframe, icecream, into the head() command:

Solution1<-
  
# Good. Now that you can see the names of variables and some of their values, we need to calculate some key summary statistics.

# We want the descriptive statistics of ice cream consumption, number of children in the family, and average age of the children. For each of these solutions, use the `summary` command to generate a table of summary statistics on each variable. We’ve done the first one for you, so select the following code and hit the “Run Code” button:

   summary(icecream$consumption)

# 2) Now repeat this command, but for the variable `children`:

#---- Question 2——————————————————#
      solution2<-
#----------------------------------------------------#

3) Now repeat the summary command again, but for the variable `age`:

#---- Question 3——————————————————#
      solution3<-
#----------------------------------------------------#

# Now let’s find out the largest number of children in any one family. You can use the `max` command to find this, or you can just type the answer in. The format for the max command is max(dataframe$variable):

#---- Question 4——————————————————#
      solution4<-
#----------------------------------------------------#


*** =solution
```{r}
solution1<-head(icecream)
solution2<-summary(icecream$children)
solution3<-summary(icecream$age)
solution4<-max(icecream$children)
```

*** =sct
```{r}
test_object(“solution1")
test_object(“solution2”)
test_object(“solution3”)
test_object(“solution4”)
success_msg(“As you can see, the largest family in this sample is unusually big as it has 13 children in it. Descriptive statistics can help us detect extreme values. 13 children in a household seems like a lot. 

But is this observation an outlier? An outlier may result from variability or it may indicate measurement error (i.e., the survey participant writes down a wrong number). If it's the latter, we can exclude this observation from data. However, we are not sure whether this is the case. After all, there is a reality show featuring a family with 19 children. So, 13 children could be possible.”)
```

--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:

## Kids Love Ice Cream: Part 2 - Ordinary Least Squares Regression (OLS) 
It can be a bit difficult to decide where to start looking for connections, so let’s begin with a question to help us frame the situation. Since we can guess that kids love ice cream, and that kids may eat more ice cream as they get older, does our data show any relationship between total ice cream consumption and the ages and number of children in a family?  Let's find the correlation between the number of children, their ages, and family ice cream consumption through a basic linear regression.

Data Dictionary:
	#consumption: ice cream consumption (in pints)
	#children: number of children in the family 
	#age: average age of the children 

*** =instructions
- 1) Use the `lm` function to create a linear regression of children and age on ice cream consumption.
- 2) Use the `lm.out` function to output the linear regression.
- 3) Use the `summary` function to generate a table of summary statistics about that regression.
- 4) Create an equation for a model of family ice cream consumption based on our results.

*** =pre_exercise_code
```{r}
set.seed(1234)
children <- round(rnorm(20,3,4),0)
children[children<0] <-0
age <- round(rnorm(20,7,5),0)
age[age<0] <-1
u <- rnorm(20,0,5)
consumption <- 100 + 4*children-3*age+u
icecream <- data.frame(consumption, children, age)
```

*** =sample_code
```{r}
# 1) The first step is to generate the results of a linear regression that looks at the correlation between our demographic variables and total ice cream consumption by each family. First use the `lm` function to create a linear regression of children and age on outcome variable, ice cream consumption:

# Note: If you’ve never run a regression with multiple independent variables in R before, the format for the lm() function is: lm(outcome ~ variable1+variable2))

#---- Question 1-------------------------------------#
      Solution1<-
#----------------------------------------------------#

# 2) Great. Now we’ll repeat that code, but within the lm.out() function, which will output the linear regression. 

# 3) Now run the summary() function on Solution1 to look at the correlation. Note the tour y intercept shows the base level of ice cream eaten in families with no children: 

#---- Question 2——————————————————#
      solution2<-
#----------------------------------------------------#

# 4) Fill in the blanks of the following equation to create a model for family ice cream consumption based on our regression results from Solution2:

#---- Question 3——————————————————#
      Solution3<-“ ” + “ ”*children - “ ”*age
#----------------------------------------------------#

```

*** =hint
- The first step is to put the output of your `lm` command into a dataframe like this: lm.out<-with(dataframe, lm(outcome ~ variable1+variable2))
- Then output the summary statistics of that dataframe with summary(Solution1)



*** =solution
```{r} 
solution1<-lm(consumption~age+children)
solution2<-summary(solution1)
Solution3<-100.2988 + 3.5469*children - 3.2950*age
```


*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
success_msg(“Nice job. The prediction equation is: consumption = 100.2988 + 3.5469*children - 3.2950*age.”)
```


--- type:NormalExercise lang:r xp:50 skills:1 key:

## Kids Love Ice Cream: Part 3 - Understanding the Output Table
Running a regression is the easy part, and before we can finish our argument for causality, we need to interpret our regression in the context of your theoretical framework, central question, and the limitations of your data. So let’s break down the numbers and look at what they can (or can’t) tell us about our main question about kids and ice cream consumption.

*** =pre_exercise_code
```{r}
set.seed(1234)
children <- round(rnorm(20,3,4),0)
children[children<0] <-0
age <- round(rnorm(20,7,5),0)
age[age<0] <-1
u <- rnorm(20,0,5)
consumption <- 100 + 4*children-3*age+u
icecream <- data.frame(consumption, children, age)
model<-lm(consumption ~ children+age)

```

*** =instructions
1) Run the summary() command on our model 
What's the coefficient on `children`?
2) What's the standard error of this estimate?


***=sample_code
```{r}
# 1) There are several relationships we could choose to understand better, but let’s start with a single but important one. What does our regression help illuminate between the number of children in a family and its total ice cream consumption?

# First, run the summary command again on our equation of our first regression, `model`.

#---- Question 1-------------------------------------#
      Solution1<-
#----------------------------------------------------#

# 2) What is the coefficient on `children`?

#---- Question 2——————————————————#
      solution2<-
#----------------------------------------------------#

# Nice going. This result gives us an estimate for how many pints of ice cream each child eats in a family.

# But what is the standard error for this coefficient? Now type in the standard error for regression on `children` below: 

#---- Question 3——————————————————#
      Solution3<-
#----------------------------------------------------#
```

*** =solution
```{r}
Solution1<-summary(model)
solution2<-3.5469
solution3<-0.3380
```

*** =sct
```{r}
test_object("Solution1")
test_object(“Solution2”)
test_object(“Solution3”)
success_msg("Good work! Our coefficient on `children` has an error margin of roughly +/- 0.3, or a little less than 10%. It would be great if that error margin were smaller, and sometimes it’s hard to tell at a glance whether that standard error should change our interpretation about the strength of our results.”) 







--- type:NormalExercise lang:r xp:50 skills:1 key:
## Kids Love Ice Cream: Part 4 - Statistical Significance Checks
There are several numbers that the summary() function generates that that can help us decide whether our coefficients are statistically significant, so let’s look at 3 of them: the t-statistic, the p-value, and the $R^$2 value. 

***=instructions
- **Interpreting t-statistics:**
- A t-statistic of 0 is the worst case scenario. It means that we should have zero confidence in the statistical significance of our regression coefficient.
- In contrast, a t-statistic larger than +/-2 usually means that you can have over 95% confidence in your coefficient’s significance, and the larger the t-statistic, the more confidence you can have in your coefficient’s predictive power. 
- **Interpreting p-values**
- A p-values range from 0 to 1, and express the likelihood that our results are due to chance.
- Typically, p-values under 0.05 are considered statistically significant
- **Interpreting $R^2$ values**
- 0% indicates that the model explains none of the variability of the response data around its mean. 
- 100% indicates that the model explains all the variability of the response data around its mean. 
*** =pre_exercise_code
```{r}
set.seed(1234)
children <- round(rnorm(20,3,4),0)
children[children<0] <-0
age <- round(rnorm(20,7,5),0)
age[age<0] <-1
u <- rnorm(20,0,5)
consumption <- 100 + 4*children-3*age+u
icecream <- data.frame(consumption, children, age)
model<-lm(consumption ~ children+age)
summary(model)
```
# We calculate the t-statistic by dividing the regression coefficient by its standard error—it’s a quick way to help us see how precise our regression coefficient is. It can be positive or negative, but for now you can ignore the sign and look at the size of the result to learn about how statistically significant our coefficient is.

# 1) What's the t-statistic for the coefficient on `children`? 

#---- Question 1-------------------------------------#
      Solution1<-
#----------------------------------------------------#

# Nice. That is pretty far from 0 for a t-statistic, so we can very safely say that this is a statistically significant result.

# Another test we can use is the p-value, which is a more common (and misused!) statistic that returns a value between 0 and 1. Let’s see if our p-value is under 0.05, which is a standard goal for significance.

# What’s the p-value for our regression?

#---- Question 2——————————————————#
      solution2<-
#----------------------------------------------------#

# Good. It’s very safe to interpret that as a very strong sign of statistical significance!

# You now want to make sure that your regression closely represents what’s happening in  your actual data. The $R^2$ coefficient of determination is a statistical measure of how well a linear regression line approximates the real data points. It tells you how much of the scattering of data points around the mean is explained by the model, from 0% - 100%.

# Of course, it’s not always that simple if your data is nonlinear or on a subject that’s generally hard to predict, so we can’t always assume that a high $R^2$ is better than a low $R^2$. 

 #In this case, R-squared is a useful term, which is termed “Multiple R Squared” in this table to distinguish it from a different measure called Adjusted R Squared. What is the Multiple R-squared value is of this model?

#---- Question 3——————————————————#
      Solution3<-
#----------------------------------------------------#

*** =solution
```{r}
Solution1<-10.10
Solution2<- 5.115x10e-11
Solution3<-0.9384
```

*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
test_object("Solution3”)
success_msg("Good work! Our t-statistic is 10.10, well above the 2 that reflects 95% confidence in our coefficient. In addition, our p-value of 0.00000005115 is well below our 0.05 goal. Finally, our $R^$2 value says that the data explains 93.8% of the variability in the data. We can have extreme confidence that our calculated coefficient on `children` is statistically significant. That is, if we can trust our data!")
```


--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:
## Income Inequality at FutureChew: Part 1 - Data Discovery

Food-focused software company FutureChew is growing, and like in many companies, the Director of Human Resources has noticed that the female employees are somehow getting paid slightly less than their male coworkers. The Director doesn’t think it’s due to their company hiring policies, but she’s not sure exactly what’s going on, so she asks her data analyst to use the information in their employee database to help find any causes for this. She knows it would be highly unethical to run an experiment that randomly changed people’s salaries, so she’s looking to models for an understanding of the situation. She also knows that it will be complicated, and the answer might not be completely clear.

We shall take the role of this analyst, and let’s start by looking at the hourly income for men and women at FutureChew. Here is a dataset `data` has 20 observations.

Data dictionary:

inc         : hourly income
female      : 1 if female
age         : age
exp         : work experience in years
educ        : education in years

*** =instructions
- 

***=pre_exercise_code
```{r}
set.seed(123)
female <- rbinom(20,1,0.5)
age <- round(rnorm(20,40,20),0)
age[age<24] <- 24
exp <- age-25-rnorm(20,0,4)
exp[exp<0] <- 0
exp[exp>35] <-35
u <- rnorm(20,0,1)
data <- data.frame(female,age,exp)
data$educ[data$female==1] <- round(rnorm(sum(female),10,5),0)
data$educ[data$female==0] <- round(rnorm(20-sum(female),15,5),0)
data$educ[data$educ<0] <- 0
data$inc <- 2+0.4*data$educ+0.3*data$exp-2*female+u
data$inc[data$inc<8] <-8
```

***=sample_code
```{r}
## Correlation
# Let’s start by generating a correlation matrix. This will show us the correlations between different pairs of variables, which can give us a general sense of what variables seem to be connected in our data (either positively or negatively). And luckily for us, we don’t have many variables to consider—it’s a different story when you have thousands to look at!

# 1 ) Run the head() command to take a look at the first 6 rows of the dataframe `data`. This will tell us the variable names and some of their values:

#---- Question 1-------------------------------------#
      Solution1<-
#----------------------------------------------------#

# 2) Now that we know what variables are in the dataframe, think about what variables might have the largest and smallest correlations. Write down what pair of variables will be most correlated:

#---- Question 2——————————————————#
      most.correlated<-“ “ and “ “
#----------------------------------------------------#

# 3) Now write down the pair of variables you think will be the least correlated:

#---- Question 3——————————————————#
      least.correlated<-“ “ and “ “
#----------------------------------------------------#

# 4) Now let’s see what the biggest and smallest correlations are between them. Make a correlation matrix of `female`,`age`,`exp`,`educ` and `inc`.

#---- Question 4——————————————————#
     Solution2<-
#----------------------------------------------------#


# THE SOLUTION IS SOME WEIRD FORMAT—USE IT OR DROP IT?

*** =solution
```{r}
Solution1<-head(data)
Solution2<-cor(data,use = "complete.obs",method = "pearson")
```

*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
success_msg("Good work! Great job. Take a look at the results, and look at the correlations of different variables in each row and column. Which correlations look larger than you were expecting? Which ones are smaller than you were expecting? We don’t know if we can trust any of these numbers yet, but perhaps these results can lead to interesting questions. Let’s keep going.
")
```







--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:

# Income Inequality at FutureChew: Part 2 - OLS Regression 
The Director tells you that the company tries to ignore an employee’s age when setting their salary, so you assume that’s true and think about a model that bases income just on education, experience, and gender, like the following:

$inc=\alpha+\beta_1educ+\beta_2exp+\beta_3female+u$,where $u$ is random error.

So let’s check out whether these variables seem to be correlated with salary in our data.

***=instructions
- 1) Start with a simple regression
- 2) Identify a variable that is negatively correlated with income.

***=pre_exercise_code
```{r}
set.seed(123)
female <- rbinom(20,1,0.5)
age <- round(rnorm(20,40,20),0)
age[age<24] <- 24
exp <- age-25-rnorm(20,0,4)
exp[exp<0] <- 0
exp[exp>35] <-35
u <- rnorm(20,0,1)
data <- data.frame(female,age,exp)
data$educ[data$female==1] <- round(rnorm(sum(female),10,5),0)
data$educ[data$female==0] <- round(rnorm(20-sum(female),15,5),0)
data$educ[data$educ<0] <- 0
data$inc <- 2+0.4*data$educ+0.3*data$exp-2*female+u
data$inc[data$inc<8] <-8

```


***=sample_code

# 1) We can use regression to help us determine the coefficients for each of these factors in an employee’s income. Regress `inc` on `female`,`exp`, and `educ`.

#---- Question 1——————————————————#
      solution1<-
#----------------------------------------------------#

# Great. The regression coefficient estimate in the (Intercept) row is suggesting that there’s a positive relationship between these variables and someone’s hourly wage at FutureChew. Now let’s dig down and find out more about what this regression can (or can’t) tell us. 

2) Which variable has a negative effect on income?

#---- Question 2——————————————————#
     solution2<-“”
#----------------------------------------------------#


*** =solution
```{r}
Solution1<-lm(inc ~ female + exp + educ)
summary(solution1)
Solution2<-“female”
```

*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
(add more for multi stage questions)
success_msg("Good work! While the data overall shows a positive correlation of gender, experience, and education  on an employee’s hourly wage, it seems that the company does tend to pay their women less: this regression says that being female seems to put a downward pressure on your income.")
```






--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:

## Income Inequality at FutureChew: Part 3 -Omitted Variable Bias

As we know, we don’t want to omit any variables in our regression that might be affecting outcomes, because that would create a bias in our results. However, we can also use the “omitted variable bias” as a tool to check whether our first guess at important factors was correct—if we remove a variable and nothing changes in our estimate, then that variable was probably not necessary. If we omit a variable and our estimate does change, the size and direction of the difference can show us more about the relationships between the variables, namely which ones are pushing our estimates up or down in any significant way. 

*** =instructions
-

***=pre_exercise_code
```{r}
set.seed(123)
female <- rbinom(20,1,0.5)
age <- round(rnorm(20,40,20),0)
age[age<24] <- 24
exp <- age-25-rnorm(20,0,4)
exp[exp<0] <- 0
exp[exp>35] <-35
u <- rnorm(20,0,1)
data <- data.frame(female,age,exp)
data$educ[data$female==1] <- round(rnorm(sum(female),10,5),0)
data$educ[data$female==0] <- round(rnorm(20-sum(female),15,5),0)
data$educ[data$educ<0] <- 0
data$inc <- 2+0.4*data$educ+0.3*data$exp-2*female+u
data$inc[data$inc<8] <-8
model<-lm(inc ~ female + exp + educ)
```

***=sample_code
```{r}

# While the data overall shows a positive correlation of gender, experience, and education  on an employee’s hourly wage, it seems that the company does tend to pay their women less: this regression says that being female seems to put a downward pressure on your income.

# In this case, let’s remove our gender variable from the regression to see if that makes any difference at all. Omit `female` from the regression, and just regress `inc` on `exp`, and `educ`. Then use the summary() command to see the results.

#---- Question 1-------------------------------------#
     lm.out2<-
#----------------------------------------------------#

# In our original regression that included `female`, the coefficient for experience was 0.25116. What is the coefficient on `exp` now that we’ve omitted `female`?

#---- Question 2——————————————————#
     solution2<-
#----------------------------------------------------#

# Likewise, our original coefficient for education was 0.26599. What is the coefficient on `educ` now that we’ve omitted `female`?

#---- Question 3——————————————————#
     solution3<-
#----------------------------------------------------#
```


*** =solution
```{r}

lm.out2 <- with(data, lm(inc ~ exp + educ))
summary(lm.out2)
Solution2<-0.25580
Solution3<-0.33554
```

*** =sct
```{r}
test_object("Solution1")
(add more for multi stage questions)
success_msg("Good work! Let’s compare these values to those in the original regression, because any changes can reveal a bias working silently within our estimate.  That means that by themselves, the effects of `exp` and `educ` are trying to pushing our coefficient estimate higher, and that was hidden when we had `female` in the regression. But it also means that `female` had a non-zero effect on salary, so gender does indeed appear to be a factor. In fact, its negative relationship with salary is masking the positive effects of education and experience in our model.")
```




--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:

## Income Inequality at FutureChew: Part  4 - Multicollinearity

But there’s more to this than just gender, education, and experience. There’s also the factor of the employee’s age: they might be surprisingly young for their education and experience, or they might be much older but still very new to the profession. However, we know that many of these variables will run in parallel: the older you are, the more likely you are to have more experience and more education. If we look at the details, are these variables actually functioning very differently from each other? Or are they so similar that we can drop one from our regression to make our lives simpler?

*** =instructions
- 1) Add a variable for age into our regression and see what happens.
- 2) If multicollinearity is a problem, drop a collinear variable and run another regression without it.

***=pre_exercise_code
```{r}
set.seed(123)
female <- rbinom(20,1,0.5)
age <- round(rnorm(20,40,20),0)
age[age<24] <- 24
exp <- age-25-rnorm(20,0,4)
exp[exp<0] <- 0
exp[exp>35] <-35
u <- rnorm(20,0,1)
data <- data.frame(female,age,exp)
data$educ[data$female==1] <- round(rnorm(sum(female),10,5),0)
data$educ[data$female==0] <- round(rnorm(20-sum(female),15,5),0)
data$educ[data$educ<0] <- 0
data$inc <- 2+0.4*data$educ+0.3*data$exp-2*female+u
data$inc[data$inc<8] <-8
model<-lm(inc ~ female + exp + educ)
```


***=sample_code
```{r}

# 1) Let’s start with the basics by including `age` in the regression. Regress `inc` on `female`,`age`,`exp`, and `educ`. Then output the results with the summary function

#---- Question 1-------------------------------------#
    solution1<-
#----------------------------------------------------#

# You should see that when the model includes `age`, the estimators have very large standard error, and none of the coefficients are significant at 5% level. 

# Note `age` is almost linear to `exp` (their correlation is 0.97).

# The multicollinearity inflates the standard error, thus the coefficients are very likely to be insignificant. We can drop one of the highly correlated variables. But which one to drop? In this case, it makes more sense to drop `age`. People always age but not necessarily gain more working experience. Thus, we can drop `age` to produce a model with significant coefficients. Now, re-run the regression without `age`.

#---- Question 2——————————————————#
     solution2<-
#----------------------------------------------------#


*** =solution
```{r}
solution1<-lm(inc ~ female + age + exp + educ)
summary(solution1)
solution2<-lm(inc ~ female + exp + educ)
summary(solution2)
```

*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
success_msg("Nice job! In the end, our data supports a model of income that uses a combination of gender, experience, and education as its variables.")
```


--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:059e8eb292
## Confounders in Discrete Choice Analysis
*** =video_link
//player.vimeo.com/video/231746457


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:d81765e7b7
## Confounders in Discrete Choice Analysis 
Someone says “I included over 1,000 variables in my analysis. Since I have so many control variables, I do not have to worry about confounders.” Do you agree?
*** =instructions
- Maybe
- Yes
- No
*** =sct
```{r}
msg1 = “Try again“
msg2 = “Try again“
msg3 = "No! The total number of variables you include generally has no relationship to whether you have to worry about confounders or not. All of these 1,000 variables might be just randomly generated numbers from the computer. In that case it’s actually worse to include them than to leave them out! What matters is the substantive interpretations of these variables, and whether they measure all variables that might have a causal effect on outcomes."
test_mc(correct = 3, feedback_msgs = c(msg1,msg2,msg3))
```

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:4608217f1d
## How Do People Choose Healthcare Plans?
*** =video_link
//player.vimeo.com/video/231746698


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:fe92309865
## Policy Questions About Health Insurance 
What policy questions might we be able to answer if we knew people’s preferences over health insurance plans?

A) The effect of lowering health insurance premiums on the number of unemployed people.
B) Whether people care more about having prescription drugs covered or having contraceptive use covered.
C) Whether having health insurance makes people healthier.

*** =instructions
- A
- B
- C
- A and B
- A and C
- B and C
*** =sct
```{r}
msg1 = “Try again“
msg2 = “Try again”
msg3 = “(C) is about the impact of having insurance on outcomes. If we only know people’s preferences over insurance plans, we still don’t have any data on health *outcomes* and hence preference data alone isn’t enough to learn about the effect of insurance on outcomes. Try again.“
msg4 = “Correct. In (A) we are specifically interested in computing a counterfactual policy prediction: What would happen to market shares of each health plan——including the “outside option” of not getting any health insurance plan and hence going uninsured——if we changed plan premiums. (C) however is about the impact of having insurance on outcomes. If we only know people’s preferences over insurance plans, we still don’t have any data on health *outcomes* and hence preference data alone isn’t enough to learn about the effect of insurance on outcomes.”
msg4 = "(C) is about the impact of having insurance on outcomes. If we only know people’s preferences over insurance plans, we still don’t have any data on health *outcomes* and hence preference data alone isn’t enough to learn about the effect of insurance on outcomes. Try again."
msg5 = "(C) is about the impact of having insurance on outcomes. If we only know people’s preferences over insurance plans, we still don’t have any data on health *outcomes* and hence preference data alone isn’t enough to learn about the effect of insurance on outcomes. Try again."
test_mc(correct = 4, feedback_msgs = c(msg1,msg2,msg3,msg4,msg5,msg6))
```


--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:0dd082cdd9
## Reading the Data to Find Preferences
*** =video_link
//player.vimeo.com/video/231746940


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:a4c08d7b61
## Reading the Coefficient Estimates 
Suppose that instead of the -0.4330 and -0.2127 coefficients we see in the first two rows of column (1) we instead saw -0.4330 and -0.4227. What would we conclude?
*** =instructions
- The data is inconsistent with rational choice.
- The data is consistent with rational choice.
- The data are not sufficient to draw a conclusion either way.
*** =sct
```{r}
msg1 = "Try again"
msg2 = “Correct. If the coefficient estimates are roughly the same, then that means people care about these two characteristics roughly the same. And that is exactly what we would expect a rational person to think. Now, you could actually justify (C) as being correct, since I didn’t tell you what the new confidence intervals / standard errors were! This is something we have to worry about in any finite sample, but here I’m ignoring it just for simplicity.”
msg3 = "Try again"
test_mc(correct = 2, feedback_msgs = c(msg1,msg2,msg3))
```


--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:
## Cash or Card? Part 1 - Data Discovery in a Table

Consumers today have many payment options, and the small town West Cargo Bank is interested in learning the probability of someone using cash or a card in a store or restaurant. They are thinking that cash may be used more frequently at low value transactions, but debit and credit cards might be used more intensively in higher value transactions for record keeping purposes. The Bank knows they can’t control how credit cards and cash are used in the macroeconomy, so instead they look to model these systems based on the data they have.  The data set `Data` contains 3 variables and 72 observations. The data dictionary is below.

Data Dictionary:

ID          : subject identifier
payment        : choice of payment (cash, credit card, or debit card)
value         : value of transaction in dollars

*** =instructions
-

***=pre_exercise_code
```{r}
library(dplyr)
set.seed(123)
#Dataframe
ID <- 1:72
payment <- c(rep("Cash",15),rep("Credit",35),rep("Debit",22))
payment <- sample(payment, replace = FALSE)
value <- rep(NA,72)
reward <- rep(NA,72)
Data <- data.frame(ID,payment,value,reward)
```


***=sample_code
```{r}
# Cash is used mostly in low value transactions, Credit cards are used more frequently for higher value transactions. Run the following code to see what this looks like:

Data$value[Data$payment == "Cash"] <- round(rnorm(15,16,4),2)
Data$value[Data$payment == "Credit"] <- round(rnorm(35,150,10),2)
Data$value[Data$payment == "Debit"] <- round(rnorm(22,100,50),2)

# Credit card rewards can also influence payment choice
Data$reward[Data$payment == "Cash"] <- rbinom(15,1,0.1)
Data$reward[Data$payment == "Credit"] <- rbinom(35,1,0.5)
Data$reward[Data$payment == "Debit"] <- rbinom(22,1,0.2)

# Make a Table
# Create a 3 by 3 table that contains averages of `value` by `payment` in the first column and the standard deviations of `value` by drinking level in the second column. Provide the R code below. 

#Note: the format to do this with dplyr is: ddply(dataframe, ~variable1, variable2, mean=mean (value), sd=sd(value))


#---- Question 1-------------------------------------#
     solution1<-
#----------------------------------------------------#
```

*** =solution
```{r}
solution1<-ddply(Data, ~payment, summarise, 
      mean = mean (value), sd = sd(value))
```

*** =sct
```{r}
test_object("Solution1")
(add more for multi stage questions)
success_msg("Good work!")
```



--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:

## Cash or Card? Part 2 - Linear Probability Model
The bank wants to know how the value of transaction influence the probability of using cash versus card (both debit and credit card). 

Data Dictionary:

ID          : subject identifier
payment        : choice of payment (cash, credit card, or debit card)
value         : value of transaction in dollars

*** =instructions
-

***=pre_exercise_code
```{r}
set.seed(123)
#Dataframe
ID <- 1:72
Data$value[Data$payment == "Cash"] <- round(rnorm(15,16,4),2)
Data$value[Data$payment == "Credit"] <- round(rnorm(35,150,10),2)
Data$value[Data$payment == "Debit"] <- round(rnorm(22,100,50),2)

# credit card rewards can also influence payment choice
Data$reward[Data$payment == "Cash"] <- rbinom(15,1,0.1)
Data$reward[Data$payment == "Credit"] <- rbinom(35,1,0.5)
Data$reward[Data$payment == "Debit"] <- rbinom(22,1,0.2)
payment <- c(rep("Cash",15),rep("Credit",35),rep("Debit",22))
payment <- sample(payment, replace = FALSE)
value <- rep(NA,72)
reward <- rep(NA,72)
Data <- data.frame(ID,payment,value,reward)
```

***=sample_code
```{r}
# Create a new dummy variable `card` with values 1 or 0 that describe whether the person used card or not. In other words, `card` is 0 if the person used a debit or credit card, 1 if he/she used cash. Then, add this variable `card` to the dataset `Data`. Provide the R code below.

#---- Question 1——————————————————#
     solution1<-
#----------------------------------------------------#

# Then we can estimate how the value of transaction influence the probability of using cash or card. One way to estimate this probability is to use Linear Probability Model (LPM). LPM works like a normal linear regression model, but the dependent variable is binary. 

# 2) Regress `card` on `value`, and then look at summary results. What's the coefficient on the independent variable? 

#---- Question 2——————————————————#
     solution2<-
#----------------------------------------------------#

# 3)The coefficient on `value` is 0.0057 (0.57%), which is the change in the probability that a person chooses card for an $1 increase in transaction value.  A predicted value $\widehat{card}$ is the predicted probability that the dependent variable `card` equals one, given `value`.

$P(\widehat{card = 1}) = 0.1697433 +  0.0057374 \times value$

# What's the expected probability of using card for a $10 transaction? 

#---- Question 3——————————————————#
     solution3<-
#----------------------------------------------------#


# 4) How will the probability of using card change if the value of transaction is $200?

#---- Question 4——————————————————#
     solution4<-
#----------------------------------------------------#

## NOT SURE IF SOLUTIONS WORK FOR 3 AND 4

*** =solution
```{r}
solution1<-card <- rep(1,72)
card[which(Data$payment == "Cash")] <- 0
Data$card <- card
solution2<-lm(card ~ value))
summary(solution2)
solution3<-predict(solution2, data.frame(value = c(10,200)))
solution4<-predict(solution2, data.frame(value = c(10,200)))
```

*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
test_object("Solution3”)
test_object("Solution4”)
success_msg("Good work! You should see that for a $10 transaction, the expected probability of using card is 22.71% . For a transaction with $200 value, your expected probability of using card should be 131.72%.

But that second number is a problem: how can a probability larger than 100%? It cannot. This is the problem with the Linear Probability Model (LPM). The model can actually produce probabilities outside [0,1]. ")
```



--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:
## Cash or Card? Part 3 - Probit Model

Let’s try to work around the issues with the LPM results by using a probit model instead. In probit regression, the predicted values are calculated using as a cumulative probability distribution function of standard normal distribution, which starts out at 0 and maxes out at 1, like this:
 
$P (Y = 1|X) = \phi(\hat{\beta_{0}}+\hat{\beta_{1}}X)$

Thus, the predicted values are bounded between 0 and 1.  


Data Dictionary:

ID          : subject identifier
payment        : choice of payment (cash, credit card, or debit card)
value         : value of transaction in dollars

*** =instructions
-

***=pre_exercise_code
```{r}
set.seed(123)
#Dataframe
ID <- 1:72
Data$value[Data$payment == "Cash"] <- round(rnorm(15,16,4),2)
Data$value[Data$payment == "Credit"] <- round(rnorm(35,150,10),2)
Data$value[Data$payment == "Debit"] <- round(rnorm(22,100,50),2)

# credit card rewards can also influence payment choice
Data$reward[Data$payment == "Cash"] <- rbinom(15,1,0.1)
Data$reward[Data$payment == "Credit"] <- rbinom(35,1,0.5)
Data$reward[Data$payment == "Debit"] <- rbinom(22,1,0.2)
payment <- c(rep("Cash",15),rep("Credit",35),rep("Debit",22))
payment <- sample(payment, replace = FALSE)
value <- rep(NA,72)
reward <- rep(NA,72)
Data <- data.frame(ID,payment,value,reward)

#INCLUDE ANSWER FROM PREVIOUS QUESTION HERE
```


***=sample_code
```{r}

# 1) Now, run a probit regression of `card` on `value`. 

#---- Question 1-------------------------------------#
     solution1<-
#----------------------------------------------------#


# 2) Using probit, what's the expected probability of using card for a $10 transaction? 

#---- Question 1-------------------------------------#
     Solution1<-
#----------------------------------------------------#

# 3) What  probability does probit give you of using card change if the value of transaction is $200?

#---- Question 1-------------------------------------#
     Solution1<-
#----------------------------------------------------#

## NOT SURE IF SOLUTIONS 2 AND 3 WORK

*** =solution
```{r}
solution1<- glm(card ~ value, family = binomial(link = "probit"), data = Data)
summary(solution1)
solution2<-predict(myprobit, data.frame(value = c(10,200)), type = "response")
solution3<-predict(myprobit, data.frame(value = c(10,200)), type = "response")
```

*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
test_object("Solution3”)
success_msg("Good work! Now we see that the predicted probabilities fall within 0 and 1.")
```




--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:

## Cash or Card, Part 4 - Probit or Logit?
There’s another kind of probability model that we should check before we’re done called a legit model, and it’s often done as a counterpart to probit models.

Data Dictionary:

ID          : subject identifier
payment        : choice of payment (cash, credit card, or debit card)
value         : value of transaction in dollars

*** =instructions
-

***=pre_exercise_code
```{r}
set.seed(123)
#Dataframe
ID <- 1:72
Data$value[Data$payment == "Cash"] <- round(rnorm(15,16,4),2)
Data$value[Data$payment == "Credit"] <- round(rnorm(35,150,10),2)
Data$value[Data$payment == "Debit"] <- round(rnorm(22,100,50),2)

# credit card rewards can also influence payment choice
Data$reward[Data$payment == "Cash"] <- rbinom(15,1,0.1)
Data$reward[Data$payment == "Credit"] <- rbinom(35,1,0.5)
Data$reward[Data$payment == "Debit"] <- rbinom(22,1,0.2)
payment <- c(rep("Cash",15),rep("Credit",35),rep("Debit",22))
payment <- sample(payment, replace = FALSE)
value <- rep(NA,72)
reward <- rep(NA,72)
Data <- data.frame(ID,payment,value,reward)

#INCLUDE ANSWER FROM PREVIOUS QUESTION HERE
```


***=sample_code
```{r}

# We show the predicted probability of using cash in the following graph. Overall, you should see that in this case, the probit and logit regressions result in similar predicted probability. 

value <- Data$value
plot(value,card,xlab = "value of transaction",ylab="probability of using card")
curve(predict(myprobit,data.frame(value=x),type="resp"),add=TRUE,col="red")
curve(predict(mylogit,data.frame(value=x),type="resp"),add=TRUE,col="blue")
legend("bottomright",legend = c("Probit","Logit"),col=c("red","blue"),inset = c(0.1,0.1),lwd=1)


## Logit Model 
# In logit regression, the predicted values are calculated using:
$P (Y = 1|X) = \frac{1}{1+e^{-(\hat{\beta_{0}}+\hat{\beta_{1}}X)}}$

# 1) Try it for yourself and run a logit regression of `card` on `value`. Provide the R code below.

#---- Question 1-------------------------------------#
     Solution1<-
#----------------------------------------------------#

# 2) Using a logit model, what's the expected probability of using card for a $10 transaction? How will the probability of using card change if the value of transaction is $200?
Provide the R code below.

#---- Question 2——————————————————#
     solution2<-
#----------------------------------------------------#

*** =solution
```{r}
Solution1<-glm(card ~ value, family = binomial(link = "logit"), data = Data)
summary(Solution1)
solution2<-predict(mylogit, data.frame(value = c(10,200)), type = "response")
```

*** =sct
```{r}
test_object("Solution1")
test_object("Solution2”)
success_msg("Good work!")
```



--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:4c6f64e320
## Using Modeling When Experiments Are Impossible
*** =video_link
//player.vimeo.com/video/231747210


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:e26d5f2fe4
## Why Might Experiments Be Impossible?
What are some reasons why we can’t do randomized experiments?
*** =instructions
A) High rates of violence encourages states to instate a death penalty.
B) States that have the death penalty tend to have lower education, and education reduces violent behavior.
C) States that have the death penalty tend to have lower rates of mortality.
D) States that don't have the death penalty also tend to legalize more recreational drugs, which pacifies people's violent behavior.

- A
- B
- C
- D
- A and B
- B and C
- C and D
- None of the Above
*** =sct
```{r}
msg1 = "Try again"
msg2 = "Try again"
msg3 = “Try again“
msg4 = “Try again“
msg5 = “Correct! (C) might be a practical reason that prevents us from doing an experiment, but it is not a “fundamental” reason. That is, it could be overcome simply by raising enough money. And for (D), well, doing good research is a lot of work, but that’s no excuse!”
msg6 = “Try again”
msg7 = “Try again”
msg8 = “Try again
test_mc(correct = 5, feedback_msgs = c(msg1,msg2,msg3,msg4,msg5,msg6,msg7,msg8))
```

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:cc7e8a6309
## Modeling the Impact of Government Stimulus
*** =video_link
//player.vimeo.com/video/231746794


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:0a16d28c74
## Analyzing the 2009 Stimulus
True or false: The 2009 stimulus worked and helped end the Great Recession.
*** =instructions
- We have proved that it did work
- The data shows it had no effect
- I can’t tell either way
*** =sct
```{r}
msg1 = “It’s a good and important question, but unfortunately since we cannot observe the counterfactual outcome, the answer is not clear. Macroeconomists are still working to overcome this very difficult problem! (hint: put on your serious scientist hat and try again)"
msg2 = “It’s a good and important question, but unfortunately since we cannot observe the counterfactual outcome, the answer is not clear. Macroeconomists are still working to overcome this very difficult problem! (hint: put on your serious scientist hat and try again)”
msg3 = “It’s a good and important question, but unfortunately since we cannot observe the counterfactual outcome, the answer is not clear. Macroeconomists are still working to overcome this very difficult problem!"
test_mc(correct = 3, feedback_msgs = c(msg1,msg2,msg3))
```

--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:
## Red Wine: the Secret to Living Longer?

Red wine has resveratrol, a substance that reduces the risk for heart disease. Moderate consumption of red wine is believed to promote longevity. Jenny is very health-conscious and wants to figure out whether red wine is a longevity promoter. She is interested in examining the effect of wine consumption on longevity. She doesn’t have the capacity to run a huge medical experiment herself, so she decides to instead download some public data and develop some models to see what she can learn. The data set `Data.wine` contains 4 variables and 60 observations. The data dictionary is below.

***=pre_exercise_code
```{r}
library(plyr)
set.seed(123)
#Dataframe
Wine <- c(rep("light",30),rep("moderate",20),rep("heavy",10))
Wine <- factor(sample(Wine,replace = FALSE),levels = c("light","moderate","heavy"))
ID <- 1:60
Age <- rep(NA,60)
Exercise <- rep(NA,60)
Data.wine <- data.frame(ID,Wine,Age,Exercise)
#moderate drinkers live longer, heavy drinkers live the least
Data.wine$Age[Data.wine$Wine=="light"] <- round(rnorm(30,78,10),0)
Data.wine$Age[Data.wine$Wine=="moderate"] <- round(rnorm(20,85,6),0)
Data.wine$Age[Data.wine$Wine=="heavy"] <- round(rnorm(10,75,6),0)

#Moderate drinkers also exercise more 
Data.wine$Exercise <- round((Data.wine$Age + 10)/10 + rnorm(60,0,1),0)
Data.wine$Exercise[Data.wine$Exercise<0] <- 0

col <- rep("red",60)
col[Data.wine$Wine=="moderate"] <- "orange"
col[Data.wine$Wine=="heavy"] <- "yellow"
```

Data Dictionary:

ID          : subject identifier

Wine        : drinking level (light, moderate, and heavy)

Age         : age at death

Exercise    : hours spent on exercise per week

***=instructions
-


***=sample_code
```{r}
# Plotting the Age at Death among Different Drinking Levels

# The primary outcome of interest is difference in age at death (`Age`) among different drinking levels (`Wine`).  The first 6 observations is below:

head(Data.wine)

# Create a box plot that plots the primary outcome as a function of drinking level. Use different plotting color for each drinking level. Make sure to include an informative title and axis labels. Is there any evidence of a relationship between the two variables? Provide the R code below.

#---- Question 1-------------------------------------#
     solution1<-
#----------------------------------------------------#

# The mean values of age at death look different, but are they? Let's use linear regression (lm) to find out. 

# 2) Simply, regress `Age` on `Wine` and print the summary statistics. Please provide the R code below.

#---- Question 2——————————————————#
     solution2<-
#----------------------------------------------------#

#Hours of Exercise among Different Drinking Levels

# The results from simple regression show that moderate wine consumption is associated with longer life expectancy. However, can we draw the conclusion that moderate consumption of red wine promotes longevity? Moderate wine consumers may be more health-conscious and have better self-control. The might have other healthy habits (i.e., physical exercise) that contribute to longevity. 

# 3) Create a 3 by 3 table that contains averages of `Exercise` by drinking level in the first column and the standard deviations of `Exercise` by drinking level in the second column. Provide the R code below. 

#---- Question 3——————————————————#
     solution3<-
#----------------------------------------------------#

# Now, control for hours spent on exercise per week (`Exercise`) in the linear regression.

# 4) Run a multiple linear regression of `Age` on `Wine` and `Exercise`. Please provide the R code below.

#---- Question 4——————————————————#
     solution4<-
#----------------------------------------------------#




*** =solution
```{r}
solution1<-plot(Data.wine$Wine, Data.wine$Age, main = "Difference in Age at Death", ylab = "Age at Death", xlab = "Drinking Level", col = col)
solution2<-lm(Age ~ Wine))
summary(solution2)
solution3<-ddply(Data.wine,~Wine, summarise, mean = mean (Exercise), sd = sd(Exercise))
solution4<-lm(Age ~ Wine + Exercise))
summary(solution4)

```

*** =sct
```{r}
test_object("Solution1")
success_msg("Good work! The base group (light drinkers) is represented by the intercept. Light drinkers have an average life span of 79.83 years. The regression coefficient on moderate drinkers is 4.467, indicating the expected life of moderate drinkers is 4.467 years longer than that of light drinkers. The p-value of this coefficient around 3%, suggesting the difference in expected life  between those two groups is significant at 5% significance level. 

Moderate consumers spend the most time on exercise and heavy drinkers are the most physically inactive. When we control for exercise, the coefficient on moderate drinkers is no longer significant (p-value:7.24%). This implies that wine consumption cannot explain the age differences between moderate drinkers and light drinkers. However, the coefficient on `Exercise` is significantly positive at 1% significance level. Thus, longevity is driven mostly by physical activity instead of wine consumption in this case.")
```

