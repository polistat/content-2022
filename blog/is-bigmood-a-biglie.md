---
title: "Is BIGMOOD a BIGLIE?"
description: "Considering the effect of BIGMOOD on governor races"
date: "2022-11-02T16:00-01:00"
---
  
In our Oracle of Blair model, we used BIGMOOD to incorporate the national mood shift into our predictions for Senate races, but not gubernatorial races.This blog post aims to analyze the impact of including BIGMOOD in governor predictions using statistical methods.

BIGMOOD (also known as the National Mood) was incorporated into Senate races because the national mood changes the results of nationwide races more than those of races per state, such as the gubernatorial ones. Gubernatorial races center more on local perspectives as opposed to national issues. However, if the national mood was incorporated into gubernatorial races, how would that change our model?

To calculate the BIGMOOD, we took the weighted average of Democratic two-party percentages from generic ballot polls that earned at least a C- grade or higher on FiveThirtyEight. We weighted these generic ballot polls based on the number of days that had passed since their end date so that more recent polls had more weight in our model, specifically <Math inline>{"\\(e^{(-0.05\\cdot\\text{ days})}\\)"}</Math>, where d is the number of days since the poll was conducted. 

### Methodology

We performed a match pair t test between the Democratic win chances for the governor races as the model predicts and those same chances once BIGMOOD had been added. 

### Results

When we performed the matched pair t-test we found a p-value of 0.0118. This means that if our null hypothesis, that there was no difference  between the Democratic win chances with or without BIGMOOD, was true, there would only be a 1% chance of seeing differences between the two as extreme as these. This is a low enough chance to reject the null hypothesis and conclude that there is a significant difference between the governor results with or without BIGMOOD. 

![image](https://drive.google.com/uc?id=1LLBxAOfpbQXxWgBesk-wwCDlwoHXTFw1)

However, although there is a "statistically significant" difference in the data when BIGMOOD is added, the difference is far too small to impact any individual race. The largest differences were in the Florida and New Mexico governor races where BIGMOOD subtracted 0.1115 and 0.1352 percentage points from the Democratic win chance respectively. Realistically a difference of at most 0.1352 percentage points is not really going to impact our predictions at all. If someone sees a win chance of 79.431% or 79.296% they will register them as exactly the same. A 0.1352 percentage point difference in expected vote could make a difference, but not in race win chance. Most races changed even less then this when we added BIGMOOD, falling largely between -0.01 and 0.06 percentage points difference with a median of 0.008. Essentially, BIGMOOD really makes no difference when added to governor races, and as such BIGMOOD was most likely a superfluous inclusion in Senate races in its current state. Either BIGMOOD should have been weighted more in Senate races to actually matter or not included at all. In conclusion BIGMOOD did not live up to its name and needs to either go BIG or go home.