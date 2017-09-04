---
title       : Causal Inference Bootcamp: Modeling
description : This chapter will introduce you to principles of modeling causality.
attachments :

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:
## The Basics of Modeling Behavior
*** =video_link
//player.vimeo.com/video/231746347

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:
## Discrete Choice Analysis
*** =video_link
//player.vimeo.com/video/231746571

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:
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

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:
## Confounders in Discrete Choice Analysis
*** =video_link
//player.vimeo.com/video/231746457


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:
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

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:
## How Do People Choose Healthcare Plans?
*** =video_link
//player.vimeo.com/video/231746698


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:
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


--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:
## Reading the Data to Find Preferences
*** =video_link
//player.vimeo.com/video/231746940


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:
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

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:
## Using Modeling When Experiments Are Impossible
*** =video_link
//player.vimeo.com/video/231747210


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:
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

--- type:VideoExercise lang:arsd aspect_ratio:62.5 xp:50 skills:1 key:
## Modeling the Impact of Government Stimulus
*** =video_link
//player.vimeo.com/video/231746794


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:
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

