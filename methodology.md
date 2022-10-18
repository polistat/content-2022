---
title: "Methodology"
description: "How ORACLE of Blair's senate and governor models work"
---

## About
---
The Overall Results of an Analytical Consideration of the Looming Elections (ORACLE) of Blair is an election model developed by seniors at Montgomery Blair High School in Silver Spring, Maryland. Under the supervision of statistics teacher Mr. David Stein, we created this particular model during the fall 2022 semester to predict the outcomes of the upcoming 2022 senate and gubernatorial elections. This was the fourth iteration of ORACLE at Blair; previous classes developed ORACLE to forecast the 2016 presidential election, 2018 House of Representatives elections, and 2020 presidential election.

In the spirit of transparency and education, we describe in detail exactly how we came up with all of the numbers in our simulation. You can read about our reasoning and methods for constructing the model in the following sections. The code that implements our model is stored on [GitHub](https://github.com/polistat). All of the decisions made in the creation of this model were made by the students in the class, and we take full responsibility for this model’s methods and predictions. If you are interested in politics, statistics, education, or our model, please consider spreading the word about our work.

## Overview
---
All of our calculations in this model were based on the two-party vote percentage, which represented the ratio of votes cast for the Democratic candidate to the total votes cast for both the Republican candidate and the Democratic candidate. The two-party vote percentage differed from the actual vote percentage, as votes cast for third-party or independent candidates were tossed from vote totals and disregarded. Positive margins (e.g. <Math inline>{"`+5`"}</Math>, <Math inline>{"`55`"}</Math>, <Math inline>{"`0.55`"}</Math>) favored Democratic candidates, while negative margins (e.g. <Math inline>{"`-1`"}</Math>, <Math inline>{"`49`"}</Math>, <Math inline>{"`0.49`"}</Math>) favored Republicans.

All of the polls that we used in this model were taken from [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/), a website owned by ABC News that provides data-driven political news and analysis. It was created in 2008 as a polling aggregation website and blog by analyst Nate Silver.

## Priors
---

### Blair Partisan Index (BPI)
A state’s historic voting tendencies gave important insight into its future voting behavior. The Blair Partisan Index (BPI) was a metric we used to quantify how a state had voted in past elections. We calculated BPI by taking the weighted average of the two-party vote percentage earned by the Democratic candidate in the following elections, according to these respective weights:

<Center>
  <table className="mx-auto my-4 table-auto">
    <thead className="text-left uppercase text-sm text-neutral-400">
      <tr>
        <th className="px-5 py-1.5">Election</th>
        <th className="px-5 py-1.5">Weight (w)</th>
      </tr>
    </thead>
    <tbody className="">
      <tr>
        <td className="px-5 py-1.5 border-y">2014 Senate</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.05`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2014 Governor</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.04`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2016 Presidential</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.042`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2016 House</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.035`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2016 Senate</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.075`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2016 Governor</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.06`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2018 House</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.0435`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2018 Senate</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.10`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2018 Governor</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.10`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2020 Presidential</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.15`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2020 House</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.0945`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2020 Senate</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.11`"}</Math></td>
      </tr>
      <tr>
        <td className="px-5 py-1.5 border-y">2020 Governor</td>
        <td className="px-5 py-1.5 border-y"><Math inline>{"`0.10`"}</Math></td>
      </tr>
    </tbody>
  </table>
</Center>

For each past election, there was a Democratic two-party vote percentage <Math inline>{"`D`"}</Math> and weight <Math inline>{"`w`"}</Math>. Weights were determined by polling our political statistics students for weights and taking the median of the results. The BPI was calculated by taking the weighted average of these previous elections.

<Center>
  <Math>{"`BPI = frac{sum D_i*w_i}{sum w_i}`"}</Math>
</Center>

The BPI served as a starting point in our model for estimating the two-party vote percentage that the Democratic candidate would receive.

### National Mood (Bigmood)
For the Senate Model only.[^1]

[^1]: We did not consider national mood in our governors model because we decided that since gubernatorial races deal with issues on a more local level, the national mood did not play a role in voters’ opinions of gubernatorial candidates.

We incorporated a national mood shift (Bigmood) into our predictions for senate races, but not gubernatorial races. The national mood referred to the general feelings of the American people toward the country’s issues and policymakers’ actions. We decided that national mood did not affect voters’ opinions of gubernatorial candidates because gubernatorial races center more on local, as opposed to national, issues. We evaluated national mood through generic ballot polls, which asked people which party (Democratic or Republican) they would support in the election if it were held today.

To calculate the Bigmood, we took the weighted average of Democratic two-party percentages from generic ballot polls that earned at least a C- grade or higher on [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/). We weighted these generic ballot polls based on the number of days that had passed since their end date so that more recent polls had more weight in our model. The weight w for each poll was

<Center>
  <Math>{"`w = e^{-0.05d}`"}</Math>
</Center>

where <Math inline>{"`d`"}</Math> represents the number of days since the poll was conducted. This gave more recent polls an
exponentially greater impact on national mood than earlier polls.

<Center>
  <Math>{"`text(Bigmood) = frac{sum w_i * D_i}{sum w_i}`"}</Math>
</Center>

We also calculated a sampling variance <Math inline>{"`sigma_text(Bigmood)^2`"}</Math> for Bigmood, which we used in our simulation as documented Bigmood at the end of our methodology.

### BPI and Bigmood On Our Network (BABOON)

BPI And Bigmood On Our Network (BABOON) was the combination of BPI values and Bigmood for each senate race, and represented a single estimate for the prior Democratic two-party vote percentage for every election. We used BPI values in place of BABOON for gubernatorial races, as we decided not to incorporate national mood into our gubernatorial election predictions.

For each iteration of our simulation, we generated a random value from the normal distribution <Math inline>{"`X ~ mathcal{N}(text(Bigmood), sigma_text(Bigmood)^2)`"}</Math>. 

We used <Math inline>{"`X`"}</Math> to calculate BABOON for each iteration by taking the difference between <Math inline>{"`X`"}</Math> and 0.5, which represented an even two-party national mood percentage, and multiplying it by a weight of 0.15 and adding it to each race’s BPI value.

<Center>
  <Math>{"`text(BABOON) = text(BPI) + 0.15(X-0.5)`"}</Math>
</Center>

For gubernatorial races, the shift due to Bigmood is <Math inline>{"`0`"}</Math> and <Math inline>{"`text(BABOON)`"}</Math> is equal to the calculated <Math inline>{"`text(BPI)`"}</Math> value.

### Averaging Polls
Polling data was a valuable predictor of future voting behavior. In our model, we only factored in polls that earned at least a C- grade on [FiveThirtyEight](https://fivethirtyeight.com/polls/) and had no more than one Democratic and one Republican candidate with the exception of Alaska. For Alaska, we categorized any candidate other than the top Democratic and Republican candidates as a third-party candidate. As with Bigmood, we weighted polls based on the number of days that had passed since their end date so that more recent polls had more weight in our model. The weight <Math inline>{"`w`"}</Math> for each poll was

<Center>
  <Math>{"`w = e^{-0.05d)`"}</Math>
</Center>

where <Math inline>{"`d`"}</Math> represented the number of days since the poll was conducted. The average of the polls was determined
by taking the weighted average of the Democratic two-party vote percentage <Math inline>{"`D`"}</Math> for each poll.

<Center>
  <Math>{"text(polls) = frac{sum w_i * D_i}{sum w_i}`"}</Math>
</Center>

We also calculated the sampling variance <Math inline>{"`sigma_text(polls)^2`"}</Math> , which we used in our simulation as explained in a later section.

### Lean
To combine our polling average with BABOON into a single estimate for each race, we took a weighted average of the two values. In a hypothetical world in which we had an infinite number of polls for a race, the vast majority of our prediction for that race would come from polling averages. Thus, we used the arctangent function to calculate the weight <Math inline>{"`w`"}</Math> of the polling average such that as the number of polls conducted for a race approached infinity, the polling average comprised closer to 95% of our prediction. The weight was calculated by

<Center>
  <Math>{"`w = frac{1.9}{pi} arctan(1.75n_30 + 0.05n)`"}</Math>
</Center>

where <Math inline>{"`n_30`"}</Math> and <Math inline>{"`n`"}</Math> were the number of polls for that race in the last thirty days and in total respectively. Each race’s ”lean” was calculated by weighting the polling average in this manner.

<Center>
  <Math>{"`text(Lean) = w*text(polls)+(1-w)*text(BABOON)`"}</Math>
</Center>


## Determining Variance
---
There were many sources of uncertainty in our model. To calculate the overall variance, we calculated the weighted sampling variance of the polls and then added additional variance based on two factors: the number of uncommitted voters for each race (VIBE) and how wrong our Blair ORACLE predictions were for each state historically (GOOFI).

### Sampling Variance
We used the sampling variance of polls in our calculations for BABOON and Lean. Each poll had an inherent sampling variance associated with it. If the sampling variance of a poll <Math inline>{"`sigma^2`"}</Math> was defined by

<Center>
  <Math>{"`sigma^2 = frac{D(1-D)}{n}`"}</Math>
</Center>

where <Math inline>{"`n`"}</Math> was the sample size of the poll, then the total sampling variance of polls for the race <Math inline>{"`sigma_text(*)^2`"}</Math> was the weighted average of the sampling variances for each poll weighted by age using the same formula for weight <Math inline>{"`w`"}</Math> used with Bigmood and Lean.

<Center>
  <Math>{"`sigma_text(*)^2 = frac{sum sigma_i^2*w_i}{sum w_i}`"}</Math>
</Center>

### Variance of Indecisive Ballot Electors (VIBE)
The Variance of Indecisive Ballot Electors (VIBE) was a metric used to add uncertainty to our model based on how many uncommitted voters there were in each race according to polling. We defined uncommitted voters <Math inline>{"`U`"}</Math> as anyone who reported not voting for the top Republican and Democratic candidates using the formula <Math inline>{"`1-(D_text(actual)+R_text(actual))`"}</Math>. Note that <Math inline>{"`D_text(polls)`"}</Math> was not the same as <Math inline>{"`D`"}</Math> used in earlier calculations because <Math inline>{"`D`"}</Math> was the proportion of voters who voted for the top Democratic candidate among those who voted for either the top Democratic or Republican candidates, while <Math inline>{"`D_text(polls)`"}</Math> was the proportion of voters who voted for the top Democratic candidate among all people polled, including those who voted for third parties or were uncommitted. Using the weights

<Center>
  <Math>{"`w = e^{-0.05d)`"}</Math>
</Center>

we used in Bigmood and polling averages, the average uncommitted voter percentage <Math inline>{"`A`"}</Math> was calculated with

<Center>
  <Math>{"`A = frac{sum w_i*U_i}{sum w_i}`"}</Math>
</Center>

We then calculated VIBE using a logarithmic function to standardize <Math inline>{"`A`"}</Math>, as some races only had older polls that resulted in extremely high <Math inline>{"`A`"}</Math> values.

<Center>
  <Math>{"`text(VIBE)=(2ln(A))^2`"}</Math>
</Center>

### Gradient of Ordinary Fixedness Index (GOOFI)
Evaluating how accurately our general methodology predicted election results in the past helped us determine uncertainty for each race in this cycle. We calculated the Gradient of Ordinary Fixedness Index (GOOFI) by looking at how wrong the 2020 ORACLE was about each state’s behavior in the 2020 presidential election. The percent error was calculated by

<Center>
  <Math>{"`epsilon = frac{abs(D_2020 - hat D)}{hat D}`"}</Math>
</Center>

where <Math inline>{"`D_2020`"}</Math> was the actual 2020 Democratic two-party vote percentage and <Math inline>{"`hat D`"}</Math> was the 2020 ORACLE prediction.

We use an arctangent function to calculate GOOFI from this value so that our variance is not disproportionately affected by errors in the past model.

<Center>
  <Math>{"`text(GOOFI) = (frac{0.08}{pi} arctan(0.2epsilon))^2`"}</Math>
</Center>

### Overall Variance
We combined VIBE and GOOFI using the arctangent function and weights previously used to combine BABOON and polling averages. The idea behind this was the same; if we were given an infinite number of polls, the vast majority of our extra variance would come from the uncommitted voters as opposed to how wrong the 2020 ORACLE was.

<Center>
  <Math>{"`sigma_text(extra)^2 = w * text(VIBE) +(1-w) * text(GOOFI)`"}</Math>
</Center>

The overall variance <Math inline>{"`sigma^2`"}</Math> was calculated by adding the sampling variances of the polls with the extra variance.

<Center>
  <Math>{"`sigma^2 = sigma_text(polls)^2 + sigma_text(extra)^2`"}</Math>
</Center>

As a caveat, this method assumed that the polling averages were independent of uncommitted voters and the error of the 2020 ORACLE. Although it was safe to assume the 2020 ORACLE error did not affect 2022 polls, there might have been some correlation between the proportion of uncommitted voters <Math inline>{"`A`"}</Math> and the Democratic two-party vote percentage <Math inline>{"`D`"}</Math>. However, <Math inline>{"`D`"}</Math> could take on any number independent of what <Math inline>{"`A`"}</Math> was. For example, if the actual Democratic vote percentage was 20%, and <Math inline>{"`A`"}</Math> was 20%, the value of <Math inline>{"`D`"}</Math> was 25%. But if the actual Democratic vote percentage was 40%, then the value of <Math inline>{"`D`"}</Math> became 50%. There was likely little correlation between the actual Democratic vote percentage and undecided voter percentage, so the covariance between VIBE and the polling averages should have been significantly smaller than the sum of the extra variance and sampling variance.

<Center>
  <Math inline>{"`sigma_text(extra,polls)`"}</Math> &Lt; <Math inline>{"`sigma_text(polls)^2 + sigma_text(extra)^2`"}</Math>
</Center>

Therefore,

<Center>
  <Math>{"`sigma^2 = sqrt{sigma_text(polls)^2 + sigma_text(extra)^2 + 2sigma_text(extra,polls)}`"}</Math>
</Center>

simplified to the independent case of

<Center>
  <Math>{"`sigma^2 = sigma_text(polls)^2 + sigma_text(extra)^2`"}</Math>
</Center>


## Correlation
---
The leans we had at this point in the methodology were naive predictions for the outcomes of each race. There was a chance that our predictions were wrong, and if our predictions were wrong for one race, they would likely be similarly wrong for races with similar demographics. Thus, we correlated our predictions for races with similar demographics. The demographics our class decided were best for correlating the voting behavior of different races were:

- the percentage of Black residents
- the percentage of Hispanic residents
- the percentage of Evangelical Christians
- the percentage of rural residents
- the percentage of White residents with a college education
- the median household income

We started by standardizing these values for each state by converting to z-scores. We then correlated each race with every other race based on the similarity of their z-scores in these six demographic categories. For each race <Math inline>{"`S`"}</Math> we found the correlation coefficients <Math inline>{"`r_1,r_2,...,r_n`"}</Math> between race <Math inline>{"`S`"}</Math> and all the other races by creating a plot for each correlation coefficient calculation and plotting six points, one for each demographic category, using the z-scores of race <Math inline>{"`S`"}</Math> for the x coordinates and the z-scores of the race <Math inline>{"`S`"}</Math> was being correlated with for the y coordinates. The z-scores for each race were the z-scores of the state that the race was held in. Each time we ran our simulation, we calculated a simulated Democratic voter percentage for each race by pulling a number from a normal distribution with standard deviation equal to the overall variance we previously calculated for that race using VIBE, GOOFI, and sampling error. We noted the difference between our predicted Democratic voter percentage (a.k.a the predicted lean) and the simulated Democratic voter percentage for each race <Math inline>{"`t_1,t_2,...,t_n`"}</Math> in each simulation run. We then found the shift <Math inline>{"`Deltax_text(dem)`"}</Math> due to correlation for each race by taking the weighted average of the differences using the correlation coefficients for that race as weights.

<Center>
  <Math>{"`Deltax_text(dem) = frac{sum_(i=1)^n r_i * t_i}{sum_(i=1)^n r_i}`"}</Math>
</Center>

This calculation ensured that the bigger the correlation due to demographics between two races, the larger the effect each race had on the shift of the other—if one of the two races was way off from its predicted lean in a simulation, the other race was similarly off from its lean.

We also needed to account for the correlation between a senate race and a governor race in the same state, which both had the same demographics because they were held in the same state but should not have been 100 percent correlated. We decided that the correlation coefficient for races in the same state was 0.6 using data from past elections. Using this, we calculated the shift <Math inline>{"`Deltax_text(state)`"}</Math> by multiplying 0.6 to the simulated errors of the two races <Math inline>{"`t_text(senator)`"}</Math> and <Math inline>{"`t_text(governor)`"}</Math> and use that as the shifts for each race. For example, the shift effect due to the governor race on the senate race in a state would be

<Center>
  <Math>{"`Deltax_text(state) = 0.6t_text(governor)`"}</Math>
</Center>

and vice versa. If a state had only a senate race or only a governor race, then<Math inline>{"`Deltax_text(state) = 0`"}</Math>. The two senate races in Oklahoma were also correlated more strongly at 0.8, because not only were they held in the same state, they were also races for the same type of office.

Thus, the total shift <Math inline>{"`Deltax`"}</Math> due to other races for a given race was equal to the sum of its shifts due to demographics and same-state elections.

<Center>
  <Math>{"`Deltax = Deltax_text(dem) + Deltax_text(state)`"}</Math>
</Center>

The final estimate <Math inline>{"`mu`"}</Math> for a given race was then calculated by

<Center>
  <Math>{"`mu = text(Lean) + Deltax`"}</Math>
</Center>

The formula for <Math inline>{"`Deltax_text(dem)`"}</Math> became very problematic in some states. As the denominator approached 0, <Math inline>{"`Deltax_text(dem)`"}</Math> increased asymptotically to infinity. To counteract the excessive shift due to correlation, we limited the value of <<Math inline>{"`Deltax`"}</Math> such that

<Center>
  <Math>{"`Deltax in [-0.05,0.05]text(.)`"}</Math>
</Center>

### Example
Suppose that there are only states <Math inline>{"`S`"}</Math>, <Math inline>{"`A`"}</Math>, <Math inline>{"`B`"}</Math>, and <Math inline>{"`C`"}</Math>. The senator race in state <Math inline>{"`S`"}</Math> has a lean of 0.55.

- the gubernatorial race in state <Math inline>{"`S`"}</Math> has a simulated error of <Math inline>{"`0.05`"}</Math>
- the senator race in state <Math inline>{"`A`"}</Math> has a simulated error of <Math inline>{"`0.01`"}</Math> and has a correlation of <Math inline>{"`0.9`"}</Math> with state <Math inline>{"`S`"}</Math>
- the senator race in state <Math inline>{"`B`"}</Math> has a simulated error of <Math inline>{"`-0.05`"}</Math> and has a correlation of <Math inline>{"`0.1`"}</Math> with state <Math inline>{"`S`"}</Math>
- the senator race in state <Math inline>{"`C`"}</Math> has a simulated error of <Math inline>{"`0.1`"}</Math> and has a correlation of <Math inline>{"`-0.5`"}</Math> with state <Math inline>{"`S`"}</Math>
- the gubernatorial race in state <Math inline>{"`C`"}</Math> has a simulated error of <Math inline>{"`0.02`"}</Math> but has zero correlation with the senate race in state <Math inline>{"`S`"}</Math>

We can perform the calculations outlined above.

<Center>
  <Math>{"`Deltax_text(dem) = frac{0.9*0.01 + 0.1*(-0.05) + (-0.05)*0.1}{0.9 + 0.1 - 0.5} = -0.002`"}</Math>
</Center>

<Center>
  <Math>{"`Deltax_text(state) = 0.6*0.05 = +0.030`"}</Math>
</Center>

<Center>
  <Math>{"`Deltax = -0.002 + 0.03 = +0.028`"}</Math>
</Center>

Therefore the estimated Democratic vote percentage for state <Math inline>{"`S`"}</Math> is

<Center>
  <Math>{"`mu = 0.55 + 0.028 = 0.578`"}</Math>
</Center>


## Simulation
---
Once we had an estimated vote percentage and variance for each race, we obtained the final prediction for the Democratic two-party vote percentage by taking a random value from the normal distribution <Math inline>{"`p ~ mathcal{N}(mu, sigma^2)`"}</Math>. Each iteration of our model, we found a random value <Math inline>{"`p`"}</Math> for each race. The win probability of each race was the percentage of total model iterations in which the Democratic candidate won that race. The probability of winning the Senate was the percentage of total model iterations in which the Democratic party was able to secure 50 seats in the Senate, due to the Vice President being a Democrat, Kamala Harris, who could break Senate vote ties in favor of the Democratic party. Each run of our model runs one million iterations and the results are displayed on [our website](/). The implementation of our model can be found [here](https://github.com/polistat).
