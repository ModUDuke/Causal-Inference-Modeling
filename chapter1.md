
--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:812b575a73
## The Basics of Modeling Behavior
*** =video_link
//player.vimeo.com/video/231746347

--- type:NormalExercise lang:r aspect_ratio:62.5 xp:50 skills:1 key:
## How much ice cream does a household consume?
You have been newly hired as a data analyst for ConsumerCore Inc., and your very first assignment is to go through a small dataset to learn about food consumption trends in Gluttown, Michigan. You decide to look at something fun, and you see that there is data about the consumption of ice cream by 20 families in the town.  You start by finding out how much ice cream a household consume in a year by using the dataset `icecream` on 20 households.The data dictionary is below.
 
*** =instructions
- With the `summary` command, show the descriptive statistics of `consumption`, `children`, and `age`. 
- What's the maximum number of children in this sample?

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
#Data Dictionary:
	#consumption: ice cream consumption (in pints)
	#children: number of children in the family 
	#age: average age of the children 

# The first 6 observations is below:
	head(icecream)
  
# We want the descriptive statistics of ice cream consumption, number of children in the family, and average age of the children. For each of these solutions, use the `summary` command to generate a table of summary statistics on each variable. We’ve done the first one for you.
#---- Question 1-------------------------------------#
      (solution1<-summary(icecream$consumption))
#----------------------------------------------------#
#---- Question 2——————————————————#
      solution2<-“”
#----------------------------------------------------#
#---- Question 3——————————————————#
      solution3<-“”
#----------------------------------------------------#

# Now let’s find out the largest number of children in any one family. You can use the `max` command to find this, or you can just type the answer in.
#---- Question 4——————————————————#
      solution4<-“”
#----------------------------------------------------#


*** =solution
```{r}
solution1<-summary(icecream$consumption)
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

