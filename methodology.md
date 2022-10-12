---
title: "Methodology"
description: "How ORACLE of Blair's senate and governor models work"
---

> [View this page in a PDF format](/Methodology.pdf)

## About
---
The ORACLE (Overall Results of an Analytical Consideration of the Looming Elections) of Blair is an election model developed entirely by senior students at Montgomery Blair High School in Silver Spring, Maryland. Under the supervision of Mr. David Stein, we created this model during the fall semester to predict the outcomes of the upcoming 2022 Senate and Gubernatorial elections. This is the fourth iteration of election modeling at Blair; previous classes have also developed the Oracle to forecast the 2016 presidential election, 2018 congressional elections, and 2020 presidential election. 

In the spirit of transparency and education, we describe in detail exactly how we came up with all of the numbers in our simulation. You can read about our reasoning and methods for constructing the model in the following sections. The code that implements our model is available on [GitHub](https://github.com/polistat). All of the decisions in creating this model were made by the students in the class, and we take full responsibility for this model's methods and predictions. If you are interested in politics, statistics, education, or our model, please consider spreading the word about the work that we've done.


## Overview
---
All of our calculations in this model are based on the two-party vote percentage, which represents the ratio of votes cast for the Democratic candidate to the total votes cast for either the Republican candidate or the Democratic candidate. The two-party vote percentage differs from the actual vote percentage as votes cast for a third-party or an independent candidate are not counted. Positive margins favor the Democratic Party, while negative margins favor the Republican Party.

All of the polls that we use in this model are taken from [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/), a website owned by ABC News that provides data-driven political news and analysis. It was created in 2008 as a polling aggregation website and blog by analyst Nate Silver.


## Priors
---

### Blair Partisan Index (BPI)
A state's historic voting tendencies can give us important insight into their future voting behavior. The Blair Partisan Index (BPI) is a metric we use to quantify how a state has voted in past elections. We calculate BPI by taking the weighted average of the two-party vote percentage earned by the Democratic candidate in the following elections, according to these respective weights:

<Center>
  <table className="mx-auto my-4 table-auto">
    <thead className="text-left uppercase text-sm text-neutral-400">
      <tr>
        <th className="px-5 py-1.5">Election</th>
        <th className="px-5 py-1.5">Weight (<Math inline>{"`w`"}</Math>)</th>
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

For a given election, there is a Democratic two-party vote percentage <Math inline>{"`D`"}</Math> and weight <Math inline>{"`w`"}</Math>. Each weight <Math inline>{"`w`"}</Math> was chosen by the class based on how representative we thought each election was for this year's cycle. The BPI is calculated by taking the weighted average of these previous elections' outcomes:

<Center>
  <Math>{"`BPI = frac{sum D_i*w_i}{sum w_i}`"}</Math>
</Center>

The BPI serves as a starting point in our model for estimating the two-party vote percentage that the Democratic candidate will receive.

## Creating a Center
---

### National Mood (Bigmood)
For the Senate Model only[^1].

[^1]: We did not consider national mood in our governors model because we decided that since gubernatorial races deal with issues on a more local level, the national mood did not play a role in voters’ opinions of gubernatorial candidates.

We incorporated a national mood shift into our senate model predictions. The “national mood” refers to the general feelings of the American people toward the country's issues and our policymakers' actions. We evaluate national mood through generic ballot polls, which ask people ​​which party (Democratic or Republican) they would support in the election if it were held today.

To calculate the national mood, we take the weighted average of generic ballot polls that have earned at least a C- grade or higher on [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/). We weighted polls based on how long ago they were conducted so that more recent polls are weighted more heavily in our model. The weight w for each poll is

<Center>
  <Math>{"`w = e^{-0.05d}`"}</Math>
</Center>

where <Math inline>{"`d`"}</Math> represents the number of days since the poll was conducted. This allows more recent polls to have an exponentially greater effect on national mood than earlier polls.

Bigmood is calculated by taking the weighted average of the Democratic two-party vote percentage <Math inline>{"`D`"}</Math> for each poll.

<Center>
  <Math>{"`text(Bigmood) = frac{sum w_i * D_i}{sum w_i}`"}</Math>
</Center>

### BPI and Bigmood On Our Network (BABOON)
<Math inline>{"`text(BABOON)`"}</Math> (BPI and Bigmood On Our Network) is our method for combining our <Math inline>{"`text(BPI)`"}</Math> value with the Bigmood into a single estimate.

For each iteration of our simulation, we generate a random value from a normal distribution whose center is our calculated <Math inline>{"`text(Bigmood)`"}</Math> value and whose variance is the sampling variance from <Math inline>{"`text(Bigmood)`"}</Math> (see Variance from Bigmood section below). We call this random number <Math inline>{"`X`"}</Math>: 

<Center>
  <Math>{"`X ~ mathcal{N}(text(Bigmood), sigma_text(Bigmood)^2)`"}</Math>
</Center>

We use <Math inline>{"`X`"}</Math> to calculate <Math inline>{"`text(BABOON)`"}</Math> for each iteration by multiplying the difference between <Math inline>{"`X`"}</Math> and the event in which the Democratic and Republican candidates receive the same number of votes by <Math inline>{"`0.15`"}</Math> and adding it to the <Math inline>{"`text(BPI)`"}</Math>:

<Center>
  <Math>{"`text(BABOON) = text(BPI) + 0.15(X-0.5)`"}</Math>
</Center>

For gubernatorial races, the shift due to Bigmood is <Math inline>{"`0`"}</Math> and <Math inline>{"`text(BABOON)`"}</Math> is equal to the calculated <Math inline>{"`text(BPI)`"}</Math> value.

### Averaging Polls
Polling data is a valuable predictor of people's future voting behavior. We take a weighted average of the poll results available for each state race to form our predictions.

In our model, we only include polls that earned at least a C- grade on [FiveThirtyEight](https://fivethirtyeight.com/polls/) and have no more than 1 Democratic and 1 Republican candidate (with the exception of Alaska). In the case of Alaska, we categorized any candidate other than the top Democratic and Republican candidates as third-party candidates.

Similar to <Math inline>{"`text(Bigmood)`"}</Math>, we weight the polls based on how long ago they were conducted so that more recent polls are weighted more heavily in our model. The weight <Math inline>{"`w`"}</Math> for each poll is:

<Center>
  <Math>{"`w = e^{-0.05d)`"}</Math>
</Center>

where <Math inline>{"`d`"}</Math> represents the number of days since the poll was conducted.

The average of the polls (<Math inline>{"`mu`"}</Math>) is determined by taking the weighted average of the Democratic two-party vote percentage <Math inline>{"`D`"}</Math> for each poll.

<Center>
  <Math>{"`mu = frac{sum w_i * D_i}{sum w_i}`"}</Math>
</Center>

### Lean
To combine our polling average with <Math inline>{"`text(BABOON)`"}</Math> (which represents the <Math inline>{"`BPI`"}</Math> shifted by national mood) into a single estimate for each race, we take a weighted average of the two values.

The weight given to the poll results is based on two factors: the total number of polls available for that race (<Math inline>{"`n`"}</Math>) and the number of those polls that were conducted in the past 30 days (<Math inline>{"`n_30`"}</Math>). The weight <Math inline>{"`w`"}</Math> is calculated by:

<Center>
  <Math>{"`w = frac{1.9}{pi} arctan(1.75n_30 + 0.05n)`"}</Math>
</Center>

This is because for races that have been highly polled, our estimate should largely be based on polling averages. We chose the arctangent function to calculate the weight for the polling average so that as the number of polls approaches infinity, the polling average comprises <Math inline>{"`95%`"}</Math> of our model’s estimate.

The weight given to <Math inline>{"`text(BABOON)`"}</Math> is complementary to the weight given to the polls so that the weights sum to <Math inline>{"`1`"}</Math>.

Thus, the overall state lean (our estimate for the Democratic two-party vote percentage) is the weighted average of the poll results and <Math inline>{"`text(BABOON)`"}</Math>:

<Center>
  <Math>{"`text(Lean) = w*text(polls)+(1-w)*text(BABOON)`"}</Math>
</Center>


## Determining Variance
---
There are many sources of uncertainty in our model. To calculate the overall variance, we begin by finding the weighted sampling variance of the polls and then adding additional variance based on two factors: the number of undecided voters for each race (VIBE) and how wrong a state’s polling predictions have been historically (GOOFI).

### Sampling Variance
Every poll has an inherent amount of sampling variance associated with it. The sampling variance of a poll <Math inline>{"`sigma^2`"}</Math> is defined by:

<Center>
  <Math>{"`sigma^2 = frac{D(1-D)}{n}`"}</Math>
</Center>

Where <Math inline>{"`n`"}</Math> is the sample size of the poll and <Math inline>{"`D`"}</Math> is the predicted Democratic two-party percentage.

To find the combined variance from all of the polls used in making our prediction, we take the weighted average of the sampling error for each poll. The weight given to each poll is the same as the weight used to average the polls (see Averaging Polls section). 

<Center>
  <Math>{"`sigma_text(*)^2 = frac{sum sigma_i^2*w_i}{sum w_i}`"}</Math>
</Center>

### Variance from Bigmood
The generic ballot polls used to calculate Bigmood also have sampling error. We follow the same steps used to find variance from the polls (see Variance from the Polls section) to find the sampling error of each generic ballot poll and take a weighted average according to the weights used for calculating Bigmood (see Bigmood section). 

### Variance of Indecisive Ballot Electors (VIBE)
VIBE (Variance of Indecisive Ballot Electors) is a metric used to add uncertainty to our model's predictions based on how many uncommitted voters there are in each race according to the polls. We define the percentage of uncommitted voters <Math inline>{"`U`"}</Math> as the percentage of people in each state who did not report that they plan to vote for either the Democratic or the Republican candidate:

<Center>
  <Math>{"`U = 1-(D+R)`"}</Math>
</Center>

where <Math inline>{"`D`"}</Math> is the poll’s predicted Democratic vote share and <Math inline>{"`R`"}</Math> is the poll’s predicted Republican vote share.

Using the previously assigned weights for each poll (see Averaging Polls section), we take a weighted average of all of the polls' estimates for the percentage of uncommitted voters. The average uncommitted voter percentage <Math inline>{"`A`"}</Math> is:

<Center>
  <Math>{"`A = frac{sum w_i*U_i}{sum w_i}`"}</Math>
</Center>

We calculate VIBE using a logarithmic function to standardize the data as <Math inline>{"`A`"}</Math> was heavily skewed to the right. The variance in our model due to the percentage of uncommitted voters for each state is:

<Center>
  <Math>{"`text(VIBE)=(2ln(A))^2`"}</Math>
</Center>

### Gradient of Ordinary Fixedness Index (GOOFI)
Evaluating how accurately polls have been able to predict election results for states in the past can help us determine uncertainty in the polls for each state in this cycle. We calculate the Gradient of Ordinary Fixedness Index (GOOFI) by looking at how wrong the 2020 ORACLE was about each state’s behavior in the 2020 presidential election. The percent error is calculated by

<Center>
  <Math>{"`epsilon = frac{abs(D_2020 - hat D)}{D_2020}`"}</Math>
</Center>

where <Math inline>{"`D_2020`"}</Math> was the actual 2020 Democratic two-party vote percentage and <Math inline>{"`hat D`"}</Math> was the 2020 ORACLE prediction.

We use an arctangent function to calculate <Math inline>{"`text(GOOFI)`"}</Math> from this value so that our variance is not disproportionately affected by errors in the past model. To calculate variance from  <Math inline>{"`text(GOOFI)`"}</Math>:

<Center>
  <Math>{"`text(GOOFI) = (frac{0.08}{pi} arctan(0.2epsilon))^2`"}</Math>
</Center>

### Overall Variance
We combine the variance from the polls with the combined <Math inline>{"`text(VIBE)`"}</Math> and <Math inline>{"`text(GOOFI)`"}</Math> variance using the method described below.

To combine <Math inline>{"`text(VIBE)`"}</Math> with <Math inline>{"`text(GOOFI)`"}</Math>, we take a weighted average of the two values. We weight <Math inline>{"`text(VIBE)`"}</Math> using the same weight <Math inline>{"`w`"}</Math> used to combine the polling averages with <Math inline>{"`text(BABOON)`"}</Math> (see Combining Polls with <Math inline>{"`text(BABOON)`"}</Math> section). The weight given to <Math inline>{"`text(GOOFI)`"}</Math> is complementary to the weight given to <Math inline>{"`text(VIBE)`"}</Math> so that the weights always sum to <Math inline>{"`1`"}</Math>. The total additional variance from <Math inline>{"`text(VIBE)`"}</Math> and <Math inline>{"`text(GOOFI)`"}</Math> is calculated by:

<Center>
  <Math>{"`sigma_text(extra)^2 = w * text(VIBE) +(1-w) * text(GOOFI)`"}</Math>
</Center>

We compute the overall variance <Math inline>{"`sigma^2`"}</Math> for our state lean by adding the additional variance from <Math inline>{"`text(VIBE)`"}</Math> and <Math inline>{"`text(GOOFI)`"}</Math> to the weighted sampling variance from the polls.

<Center>
  <Math>{"`sigma^2 = sigma_text(polls)^2 + sigma_text(extra)^2`"}</Math>
</Center>

As a caveat, this assumes that the polling averages are independent from undecided voters and the error of the 2020 ORACLE. Although it is safe to assume the 2020 ORACLE error is independent from 2022 polls, there might be some correlation between the proportion of undecided voters <Math inline>{"`A`"}</Math> and the Democratic two-party vote percentage <Math inline>{"`D`"}</Math>. However, <Math inline>{"`D`"}</Math> can take on any number independent of what <Math inline>{"`A`"}</Math> is. For example, if the actual Democratic vote percentage is 20%, and <Math inline>{"`A`"}</Math> is 20%, the value of <Math inline>{"`D`"}</Math> is 25%. But if the actual Democratic vote percentage is 40%, then the value of <Math inline>{"`D`"}</Math> becomes 50%. There is likely little correlation between the actual Democratic vote percentage and undecided voter percentage, so the covariance between <Math inline>{"`text(VIBE)`"}</Math> and the polling averages should be significantly smaller than the sum of the extra variance and sampling variance.

<Center>
  <Math inline>{"`sigma_text(extra,polls)`"}</Math> &Lt; <Math inline>{"`sigma_text(polls)^2 + sigma_text(extra)^2`"}</Math>
</Center>

Therefore,

<Center>
  <Math>{"`sigma^2 = sqrt{sigma_text(polls)^2 + sigma_text(extra)^2 + 2sigma_text(extra,polls)}`"}</Math>
</Center>

simplifies to the independent case of

<Center>
  <Math>{"`sigma^2 = sigma_text(polls)^2 + sigma_text(extra)^2`"}</Math>
</Center>


## Correlation
---
The leans we have now are naive predictions for the outcomes of each race. There is a chance that our predictions are wrong, and if our predictions are wrong for one state, they should be similarly wrong for states with similar demographics. Thus, we must correlate our predictions for states with similar demographics. The demographics of importance that we have decided are:

- the percentage of Black residents
- the percentage of Hispanic residents
- the percentage of Evangelical Christians
- the percentage of rural residents
- the percentage of White residents with a college education
- the median household income

We start by standardizing these values for each state by their z-scores. As an example, we will take state <Math inline>{"`S`"}</Math>. We find the correlation coefficients <Math inline>{"`r_1,r_2,...,r_n`"}</Math> between the z-scores of state <Math inline>{"`S`"}</Math> and the z-scores of all the other states. We then simulate the error between our prediction and what the actual Democratic voter percentage is <Math inline>{"`t_1,t_2,...,t_n`"}</Math> with a normal distribution of the same variance as the overall variance for that race. The idea is that since the error is the result of adding many small, independent sources of error together, the total error becomes normal by the central limit theorem, and the variance we already have is a good guess for the distribution’s variance. We then take the weighted average of the errors with the correlation coefficients as weights to find the shift due to demographics <Math inline>{"`Deltax_text(dem)`"}</Math>.

<Center>
  <Math>{"`Deltax_text(dem) = frac{sum_(i=1)^n r_i * t_i}{sum_(i=1)^n r_i}`"}</Math>
</Center>

This ensures that the bigger the correlation between two states, the larger the effect one state will have on the shift of another. As a note, we are only correlating senator races with each other and governor races with each other for now, and the two Oklahoma senator races are set to have a correlation coefficient of <Math inline>{"`r = 0.8`"}</Math> with each other. We repeat and find the <Math inline>{"`Deltax_text(dem)`"}</Math> for every other race.

We also need to account for the correlation between a senator race and a governor race in the same state. We decided that the correlation coefficient for races in the same state was 0.6. Using this, we calculate the shift <Math inline>{"`Deltax_text(state)`"}</Math> by multiplying 0.6 to the simulated errors of the two races <Math inline>{"`t_text(senator)`"}</Math> and <Math inline>{"`t_text(governor)`"}</Math> and use that as the shifts for each race. For example, the shift due to the governor race on the senator race in state would be

<Center>
  <Math>{"`Deltax_text(state) = 0.6t_text(governor)`"}</Math>
</Center>

and vice versa. If a state has only a senator race or only a governor race, <Math inline>{"`Deltax_text(state) = 0`"}</Math>.

Thus, the total shift <Math inline>{"`Deltax`"}</Math> due to other races on a given race is equal to the sum of its shifts due to demographics and within state elections.

<Center>
  <Math>{"`Deltax = Deltax_text(dem) + Deltax_text(state)`"}</Math>
</Center>

The final estimate <Math inline>{"`mu`"}</Math> for a given race is then calculated by

<Center>
  <Math>{"`mu = text(Lean) + Deltax`"}</Math>
</Center>

### Example
Suppose that there are only states <Math inline>{"`S`"}</Math>, <Math inline>{"`A`"}</Math>, <Math inline>{"`B`"}</Math>, and <Math inline>{"`C`"}</Math>. The senator race in state <Math inline>{"`S`"}</Math> has a lean of <Math inline>{"`0.55`"}</Math>.

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
Now that we have a estimated vote percentage and variance for each race, we can now obtain the final prediction for the Democratic two-party vote percentage by taking a random value from the normal distribution <Math inline>{"`p ~ mathcal{N}(mu, sigma^2)`"}</Math>. Each iteration of our model, we find a random value <Math inline>{"`p`"}</Math> for each race. The win probability of each race is calculated by the percentage of the time that the Democratic candidate won that race. The probability of winning the Senate is calculated by the percentage of the time that the Democratic party was able to secure enough seats to control 50 seats in the Senate. Each run of our model runs one million iterations and the results are displayed on [our website](/). The implementation of our model can be found [here](https://github.com/polistat).
