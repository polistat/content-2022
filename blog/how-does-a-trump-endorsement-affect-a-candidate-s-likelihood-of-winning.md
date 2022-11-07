---
title: "How does a Trump endorsement affect a candidate's likelihood of winning?"
description: "We analyze whether or not Trump-endorsed candidates perform differently than non-Trump-endorsed candidates"
date: "2022-11-04T08:16-04:00"
---
  
## Introduction 
Since 2016, Donald Trump has taken up a unique position in American politics. Despite having been impeached twice and currently being under investigation for treason, he still retains a huge following. Because of this, there is a split in the Republican party based upon whether or not someone supports Trump. However, our model did not account for how Trump-friendly a candidate is while calculating win percentages. We questioned how a Trump endorsement would affect a candidate's chance of winning in the 2022 Senate election. 
## Methodology
To see the effects of a Trump endorsement, we divided the 2022 Republican Senate candidates into a Trump-endorsed category and a non-Trump-endorsed category and compared the differences between the Republican BPI (equal to 100 - regular BPI) and the predicted vote percent of the Republican candidate, as predicted by the Oracle, of those two categories. We modeled the distribution of the difference with each category. Using a two sample t-test, we compared the distributions of difference of both categories to see if there was a significant difference between the distributions that could be explained by a Trump endorsement.
We did not include data from the Alaska Senate race since both candidates are Republicans, which makes us unable to use BPI as a predictor of performance. Although Utah had no Democratic candidate, we were still able to analyze it with BPI since the candidates are not from the same party.
## Results and Analysis
The t-test returned a p-value of .678, meaning there is a 67.8% chance of getting a difference between endorsed and unendorsed this extreme with no actual difference between odds. Since 67.8% is too high a chance to rule out a lack of difference, there appears to be no significant distinction between the BPI differences of candidates that were endorsed by Trump and those that weren’t. Despite this overall similarity of results, it should be noted that several candidates showed significant differences from the BPI. Whether or not this was related to endorsements from Donald Trump is not clear.
![endorsed](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/14_endorsed.png)
![unendorsed](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/14_unendorsed.png)
Figure 1: Boxplots of BPI differences for endorsed and unendorsed republican candidates.

As evident in the graphs above, the BPI differences for the unendorsed republican candidates and the endorsed republican candidates are similar in shape and skew. However, the unendorsed category has a smaller spread. 
![Map](https://raw.githubusercontent.com/polistat/content-2022/master/blog/assets/14_Map.png)
Figure 2: Distribution of BPI differences for endorsed and unendorsed republican candidates. 
States with endorsed candidates are dotted and ones that lack endorsed candidates are solid. Red states are states where the Republican candidate is underperforming in comparison to BPI, green states are states where the Republican candidate is overperforming in comparison to BPI. The intensity of the color is greater when the difference between BPI and predicted percent is larger. Gray states do not have 2022 Senate Races.

<Center>
| State | Candidate name (Republican) | Endorsed? | BPI (D) | Predicted Vote | BPI Diff |
| ---- | ---------- | ---- | ---------- | ---- | ---------- | 
| California | Mark Meuser | No | 62.59 | 37.3 | -0.11 |
|Colorado |Joe O'Dea|No|54.27|46.2|0.47|
|Hawaii|Bob McDermott|No|69.17|30.7|-0.13|
|Illinois|Kathy Salvi|No|57.93|39.7|-2.37|
|Indiana|Todd Young|No|42.43|57.2|-0.37|
|Louisiana|John Kennedy|No|20.83|69|-10.17|
|Maryland|Chris Chaffee|No|59.6|40.8|0.4|
|New Hampshire|Donald C. Bolduc|No|50.29|46.3|-3.41|
|New York|Joe Pinion|No|65.33|37.8|3.13|
|Oregon|Jo Rae Perkins|No|57.61|39.6|-2.79|
|South Dakota|John R. Thune|No|36.27|63.2|-0.53|
|Vermont|Gerald Malloy|No|56.16|37.5|-6.34|
|Washington|Tiffany Smiley|No|57.71|45.3|3.01|
|Alabama|Katie Britt|Yes|38.86|61|-0.14|
|Arizona|Blake Master|Yes|41.39|47.6|-11.01|
|Arkansas|John Boozeman|Yes|36.58|63.1|-0.32|
|Connecticut|Leora Levy|Yes|58.53|44.6|3.13|
|Florida|Marco Rubio|Yes|48.54|52|0.54|
|Georgia|Herschel Walker|Yes|41.97|49|-9.03|
|Idaho|Mike Crapo|Yes|34.36|65.5|-0.14|
|Iowa|Chuck Grassley|Yes|45.49|54.2|-0.31|
|Kansas|Jerry Moran|Yes|43.26|58.1|1.36|
|Kentucky|Rand Paul|Yes|38.47|61.3|-0.23|
|Missouri|Eric Schmitt|Yes|43.49|56.5|-0.01|
|Nevada|Adam Laxalt|Yes|50.12|50.3|0.42|
|North Carolina|Ted Budd|Yes|49.57|50.9|0.47|
|North Dakota|John Hoeven|Yes|26.86|73|-0.14|
|Ohio|J.D. Vance|Yes|45.37|51|-3.63|
|Oklahoma|James Lankford|Yes|32.93|60.3|-6.77|
|Oklahoma|Markwayne Mullin|Yes|32.93|58.5|-8.57|
|Pennsylvania|Mehmet Oz|Yes|52.83|47.1|-0.07|
|South Carolina|Tim Scott|Yes|41.54|58.4|-0.06|
|Utah|Mike Lee|Yes|34.64|55.7|-9.66|
|Wisconsin|Ronald Harold Johnson|Yes|50.71|50.4|1.11|
</Center>

Figure 3: Tables of data including state, senate candidate, whether the candidate was endorsed by Donald Trump, the BPI, the predicted Republican two party percentage vote, and the Difference between the Republican BPI (which was taken by subtracting the BPI from 100) and the predicted Republican two party percentage vote (Pred vote - BPI Republican). 

Candidates with negative BPI differences are underperforming according to our model, while ones with positive differences are performing better than expected. The highest performing Trump endorsed candidate is Leora Levy in Connecticut with a difference of 3.13, and the highest performing non-endorsed candidate is Joe Pinion with an identical difference of 3.13. There are also multiple Trump-endorsed candidates, such as Blake Masters and Hershell Walker, who are significantly underperforming the BPI (performing around 10 percentage points worse than the BPI predicts), whereas there is only one unendorsed candidate, John Kennedy, doing that poorly compared to BPI. The runner-up underperforming candidate for the unendorsed is Gerald Malloy from Vermont at -6.34 below BPI.
## References
https://www.stapplet.com/ \
https://www.mapchart.net/