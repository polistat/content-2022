---
title: "The Death of GOOFI?"
description: "IS SILLY THE SHARK TO GOOFI’S GUPPY?
The Rise and Fall of GOOFI; A SILLY tale of love, betrayal, and murder."
date: "2022-11-10T09:00-17:00"
---
  
## The Problem with GOOFI

The GOOFI (Gradient of Ordinary Fixedness Index) adds variance to our model based on error from the 2020 Oracle was about each state’s behavior in the 2020 presidential election. Taking a closer look at this method - however, one might find that it isn’t the most reasonable way of adding variance to the model. 

GOOFI takes its data from the 2020 presidential election - which may not necessarily match state behavior today, in Senate and Gubernatorial races. Presidential races operate in a much different way than midterm elections do.


### Presidential Elections and Midterm Elections. The Big Difference.

GOOFI’s root problems lie in the differences between the presidential elections and the midterm ones. Presidential elections garner a lot more media attention than midterm elections do. This attention incentivizes more people to vote. In 2020, according to the Census Bureau, 154.6 million U.S citizens voted in the presidential election - a turnout over 40 million voters higher than the 2018 midterm estimate (conducted by CBS News). The aforementioned disparity in voters between a presidential election and a midterm election is highly likely to influence the outcomes and biases that our previous Oracle 2020 predictions made, that are being factored into GOOFI.


### Overcoming GOOFI’s Shortcomings - the New Era of SILLY.

A more effective method of adding variance may be to actually use the Senate and Governor data from previous years, instead of the presidential election that doesn’t have as strong a prediction. Using the error from Senate and Governor incumbents’ elections from 6 and 4 years ago, respectively, we could then adjust the variance of the current model instead of using the 2020 presidential election. This would make variation more precise, and tailored to our situation.


### Constructing ORACLE’s New Most Powerful Weapon: SILLY

*https://docs.google.com/spreadsheets/u/1/d/1N5tLqJTcU0vIomM0JdcEiGf2bL1xs1A5qi7pA52YZXc/edit*


## Methodology

If one of the candidates is the incumbent and won the previous election:
SILLY = 10 - (2 * (two party percentage incumbent received in previous election) - 100) * 0.15

If neither of the candidates is the incumbent:
SILLY = 10

If one of the candidates is the incumbent but didn’t win the previous election (for reasons such as their predecessor died or the election was canceled):
SILLY = 9

Neither of the candidates being the incumbent makes the variance higher since there is an about equal chance of the two candidates winning the race if most of their other stats are the same. It is the same as if an incumbent won 50% of the vote, which they couldn’t even win an election with.

If one of the candidates is the incumbent but didn’t win the previous election, it is treated as if they received 53.333% of the vote, a slim margin since they never won an election.

Lastly, if one of the candidates is the incumbent, the variance goes down. It is calculated using their previous election, one of the inputs being their vote percentage minus their opponent’s vote percentage (how much they won the election by). It is calculated so that if the incumbent had 83.333% of the vote or more, the variance becomes 0. This is because if the incumbent already won by that much before, it is almost certain that whatever the final prediction is will be close to the correct value since the incumbent is very likely to win the current election. The larger the gap between the incumbent and their previous opponent, the lower the variance.


## Comparison

For all races in the chart found in the link above, the SILLY variance is greater than the GOOFI variance. This is because there should be more variance to allow for greater error.

Specifically, the Tennessee Governor race is an example of very little error given by GOOFI, but large error given by SILLY. The Tennessee presidential race in 2020 was predicted very well by the Oracle, meaning that the GOOFI is very close to 0. However, the SILLY is about 7 because the Tennessee Governor incumbent is running again, and he received about 60% of the vote. SILLY is most likely more effective in this model because the Oracle’s prediction of the presidential race in Tennessee being basically correct does not predict that the gubernatorial race will go the same way, it just shows that the prediction for that presidential race went well in that one state. The SILLY takes into account the races for Senate and Governor instead of the president since our current elections do not involve the president.

### What happens overall if SILLY is used instead of GOOFI

Variance generally goes up, which means that the probabilities will have less certainty and more error. It also means that our range should be larger, which will allow for more correct ranges but less confidence.

The 2020 presidential election is not used, Senate and Governor races are used instead. This method should be more tailored to the races we’re actually looking at instead of how close the Oracle was during the 2020 presidential election.