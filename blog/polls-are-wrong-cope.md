---
title: "Polls Are Wrong: Cope"
description: "Pollsters SEETHE"
date: "2022-11-04T16:41-10:00"
---
  
## Background
 
In recent election cycles, polls have systematically overpredicted Democratic support; according to the [Washington Post](https://www.washingtonpost.com/politics/2020-poll-errors/2021/07/18/8d6a9838-e7df-11eb-ba5d-55d3b5ffcaf1_story.html), the average poll overestimated the margin of support for Joe Biden above Donald Trump by almost 4 percentage points. Furthermore, according to the American Association for Public Opinion Research, the Governor and US Senate election polls fared even worse as the average Democratic candidate was [overpredicted by 6%](https://www.aapor.org/Education-Resources/Reports/2020-Pre-Election-Polling-An-Evaluation-of-the-202.aspx).
 
Our own preliminary analysis on US Senate polls for the 2018 and 2020 election indicated that this error was present regardless of the FiveThirtyEight grade of the poll, with even grade A and A- polls having an average of >2% error in favor of Democratic candidates (Figure 1). Therefore, past election polls regardless of grade exhibit Democratic biases, so we believe that current US Senate election polls used in Blair ORACLE will have the same partisanship.
 
In the Blair ORACLE, we account for historical poll error by adding additional variance (through GOOFI), but we do not adjust the center of current US Senate election polls even though there exists consistent Democratic polling partiality in the past. In this blog post, we seek to account for this bias by adjusting both current US Senate election polls and generic polls using the mean historical poll error for the 2018 and 2020 election cycles.
![Figure 1](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/5_Figure1.png)
<center> **Figure 1**; We compiled 2018 and 2020 US Senate election polls, categorized them by grade, and calculated the error between each poll and the actual election result. Negative error represents Democratic bias while positive error represents Republican bias.</center>
 

## Methodology
Since we are targeting systemic democratic bias in both generic polls and US Senate polls, the two aspects of the current model that are going to be altered are **(a.) the weighted average of 2022 US Senate election polls per state** and **(b.) BIGMOOD, i.e. the weighted average of 2022 generic polls**. We maintained all other aspects of the current election model when running our simulations, so for important background information, please refer to the ORACLE Methodology section.

## Adjusting State Senate Polling Error
 
We compiled all US Senate election polls **with a FiveThirtyEight grade better than D** before separating the polls by state. We excluded states that had **(a.) no Senate elections in 2022** or **(b.) states that had ≤5 total Senate polls**. We made these exclusions because states in group A won’t influence the 2022 election, and states in group B have so few polls that the inherent sampling error would be too large.

### Correct Old Poll Error (COPE)
For each poll, we calculated the Democratic Two-Party Percentage (Poll DTPP) and Actual Democratic Two-Party Percentage (Actual DTPP) with:
<br/>
<center><Math>{"\\(Poll\\space DTPP=\\frac{Poll\\space Dem.\\space Canditate\\space Percent}{Poll\\space Dem.\\space Candidate\\space Percent +Poll\\space Rep.\\space Candidate\\space Percent}\\)"}</Math></center>
<br/>
<center><Math>{"\\(Actual\\space DTPP=\\frac{Actual\\space Dem.\\space Canditate\\space Percent}{Actual\\space Dem.\\space Candidate\\space Percent +Actual\\space Rep.\\space Candidate\\space Percent}\\)"}</Math></center>
<br/>
 
The correct old poll error (COPE) for that poll would be calculated using:
 <center><Math>{"\\(COPE=Poll\\space DTPP-Actual\\space DTPP\\)"}</Math></center>
<br/>
 
### State Correct Old Poll Error (SCOPE)

To average all the polls in a state for a given election year (2018 or 2020), we assigned them a Poll Time Weight (PTW) based on the time till election using the following equation:
<br/>
<center><Math>{"\\(PTW=e^{-0.05(days\\space between\\space date\\space the\\space poll\\space was\\space taken\\space and\\space election\\space date)}\\)"}</Math></center>
<br/>
For each poll *i*, we calculated a Percentage Weight (PW) given a total number of polls *n* :
<br/>
<center><Math>{"\\(\\displaystyle PW_i=\\frac{PTW_i}{\\sum_{1}^{n}{PTW}}\\)"}</Math></center>
<br/>
The polls were then averaged with their percentage weights to calculate the State Correct Old Polling Error (SCOPE):
<br/>
<center><Math>{"\\(SCOPE=\\sum_1^n{PTW_i *COPE_i}\\)"}</Math></center>
<br/>
If a state had Senate elections in both the 2018 and 2020, we averaged each year’s respective SCOPE to obtain a final state SCOPE:
<br/>
<center><Math>{"\\(SCOPE=0.4*SCOPE_{2018}+0.6*SCOPE_{2020}\\)"}</Math></center>
<br/>
 
### State Electoral Error Tweaking by Historical Elections (SEETHE)

Each state’s SCOPE was added onto the Old Weighted Average of Current US Senate Polls for that state (OWACP) to calculate the New Weighted Average of Current Polls (NWACP):
<br/>
<Math>{"\\(NWACP=SCOPE+OWACP\\)"}</Math>
<br/>
 
## Adjusting Generic Polling Error

We took all the generic polls for the 2018 and 2020 election cycles **with a FiveThirtyEight grade better than D**. We grouped the polls based on election season and calculated **(a.) the election season’s Popular Vote Democratic Two Party Percentage (PVDTPP)** and **(b.) each poll’s Generic Poll Democratic Two-Party Percentage (GPDTPP)**:
 
 <center><Math>{"\\(PVDTPP=\\frac{Popular\\space Vote\\space Dem.\\space Percentage}{(Popular\\space Vote\\space Dem.\\space Percentage)+(Popular\\space Vote\\space Rep.\\space Percentage)}\\)"}</Math></center>
<br/>
<center><Math>{"\\(GPDTPP=\\frac{Generic\\space Poll\\space Dem.\\space Percentage}{(Generic\\space Poll\\space Dem.\\space Percentage)+(Generic\\space Poll\\space Rep.\\space Percentage)}\\)"}</Math></center>
<br/>

We obtained the Generic Poll Error (GPE) of each poll which is:
 <center><Math>{"\\(GPE=PVDTPP-GPDTPP\\)"}</Math></center>
 
### Averaging Polls

To average all the generic polls for a given election year, we assigned them a Poll Time Weight (PTW) based on the time till election using the following equation:
  <br/>
<center><Math>{"\\(PTW=e^{-0.05(days\\space between\\space date\\space the\\space poll\\space was\\space taken\\space and\\space election\\space date)}\\)"}</Math></center>
<br/>
For each poll _i_ , we  calculated a Percentage Weight (PW) given a total number of polls n:
<br/>
<center><Math>{"\\(\\displaystyle PW_i=\\frac{PTW_i}{\\sum_{1}^{n}{PTW}}\\)"}</Math></center>
<br/>
 
All the poll’s GPE were averaged with their percentage weights to calculate the Cycle Generic Poll Error (CGPE):
<br/>
<center><Math>{"\\(CGPE=\\sum_1^n{PTW_i *GPE_i}\\)"}</Math></center>
<br/>
To obtain the Total Generic Poll Error (TGPE), we averaged the CGPE's of the 2018 and 2020 election cycle with the following equation:
<br/>
<center><Math>{"\\(TGPE=0.4*CGPE_{2018}+0.6*CGPE_{2020}\\)"}</Math></center>
<br/>
We then added TGPE to the original ORACLE BIGMOOD to obtain the adjusted BIGMOOD.

## Limitations of Our Methodology

Our adjustments have several limitations, both in implementation and in theory. States without enough polls to be counted were treated as if they had zero error, which is definitely not true. However, this was not as significant a flaw as it may seem because states lacking in polls were usually those states that were not competitive. In addition, we only had data for the 2018 and 2020 races, which exposes our analysis to being over-reliant on a single race for determining the pattern of error in a state.

## Regional and State Poll Bias Analysis for 2018 and 2020 Cycle Polls
 
For states that historically lean towards one party, polls tend to overestimate the opposition (Figure 3). However, these errors are greatly skewed in favor of the Democratic party with traditionally Republican states having an average of more than 3.5% polling error in favor of the Democrats (Figure 2).
![Figure 2](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/5_Figure2.png)
 
<center>**Figure 2**; The mean polling bias for each state based on the 2018 and 2020 election cycles were plotted here. The darker the color the more severe the bias.</center>
<br/>
![Figure 3](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/5_Figure3.png)
<center>**Figure 3**; We separated the states into traditionally Democrat or Republican by past national election voting records, and then averaged them using the calculated error of each state. Positive represents polling bias in favor of the Republicans and negative represents that in favor of the Democrats.</center>
 
Most notably Idaho, which leans very heavily Republican, also has a weighted average of 5% pro-Republican bias. Vermont, Maryland, and Colorado are all historically Democratic, and polls slightly over predicted Democratic candidates in those states.
 
Breaking down further by census regions, polls for races in the West overpredict Republicans by an average of around 1%, and those in the South and Midwest overpredict Democrats by an average of around 3.2% and 4% respectively. Those in the Northeast by less than 0.1% in either direction (Figure 4).
 
It should be noted however that the Northeast contains the outlier of New York which when excluded from the average gives a new mean error of just under 1% in favor of Democrats. In summary the Midwest and South both have heavy Democrat bias in polling, while the West and Northeast tend to have a much smaller Republican bias in polling.
 
![Figure 4](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/5_Figure4.png)
<center>**Figure 4**; We separated the states by census region, and then averaged them using the calculated error of each state. Positive represents polling bias in favor of the Republicans and negative represents that in favor of the Democrats.</center>
 

## Simulation Outcomes

We ran 1,000,000 Monte Carlo simulations of possible election results, and the graph below represents our results using polls up to October 25th, 2022. On that day, our adjusted ORACLE model predicts a 68% chance that the Democrats win the Senate (the distribution is shown on Figure 6), while the unaltered ORACLE model predicts a 78% chance that the Democrats win the Senate. We predict that the most likely outcome is that Democrats win 51 seats in the Senate, while ORACLE predicted 52 seats.
 
While this single-seat difference may not seem significant, our adjusted model significantly changed the likelihood for each Democratic candidate to win their seat (Figure 5). This is why our adjusted ORACLE model had the Republican’s chance to win 10 percentage points higher, as the Monte Carlo Simulations would have a much higher chance of declaring Republican victory in those states.
 
![Figure 5](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/5_Figure5.png)
 
<center>**Figure 5**; This is the difference in predicted win percentage for the Democratic between our model and the unchanged ORACLE. A negative value indicates that our model predicts a lower chance of a Democratic victory than the ORACLE does. The states that are included in this graph are those that have Senate races in this election cycle for which we were able to calculate a SCOPE value.</center>
 
![Figure 6](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/5_Figure6.png)
<center>**Figure 6**; This is the US Senate seat distribution that we obtained from our Monte Carlo simulation results. The color of the bar indicates which party is being referenced while the numbers on the x-axis represent the number of seats that said party won.</center>
 
## Specific Elections of Interest
 
### Arizona
We predict that Mark Kelly (D) is more likely to win than Blake Masters (R), but we put gave the Democratic candidate a 53% chance to win in our adjusted model as opposed to a 56% chance according to the original ORACLE model.

### Georgia
We predict that Herschel Walker (R) has a slightly higher chance of winning than Raphael Warnock (D), but we give Warnock better odds than ORACLE. Warnock has a 49% chance of winning according to our model, while according to ORACLE, Warnock has a 47% chance of victory.

### Nevada
We predict that Catherine Masto (D) has very solid odds of winning - 57%! - over Adam Laxalt (R), while ORACLE predicted that Laxalt has a 55% chance of winning.

### Pennsylvania
We predict that Mehmet Oz (R) has a 54% chance of winning, while ORACLE gives Fetterman a 54% chance of becoming a Senator.

### Wisconsin
We predict that Mandela Barnes (D) is going to lose with a fair margin against Ron Johnson (R) with a 38% chance of winning, which makes the race significantly less competitive than how ORACLE predicts it. They put it at a 45% chance of a Democratic win.
 
## Conclusion
We confirmed the claim that polling as a whole has been historically biased in favor of the Democrats, specifically in the Midwest and the South. It should be noted that this is a general trend, and varies by state and race.
 
However, this consistent trend is not accounted for by the ORACLE model. ORACLE does attempt to account for historical polling error by adding variance with GOOFI, but that misunderstands the nature of the problem. Results are not randomly distributed around the true percentage of support for a candidate, but are instead  consistently biased in one direction. Not accounting for this has led to ORACLE overestimating overall support for Democratic candidates for every election in recent years.
 
Clearly, it is not possible to predict error hyper-acccurately before an election occurs, but it can be ballparked - with more advanced statistical methods than we used, better predictions can certainly be reached. This allows for some measure of correction, but it must be borne in mind that the simplest method of resolving this is for pollsters to determine the source of their consistent polling bias and correct for it.