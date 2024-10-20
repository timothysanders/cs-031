# Lecture 21: Examples

## Reading
- 12.3: [Deflategate](https://inferentialthinking.com/chapters/12/3/Deflategate.html)

### 12.3 Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plots
football = Table.read_table('./data/deflategate.csv')

football = football.with_column(
    'Combined', (football.column(1)+football.column(2))/2
    ).drop(1, 2)

patriots_start = 12.5 * np.ones(11)
colts_start = 13 * np.ones(4)
start = np.append(patriots_start, colts_start)

drop = start - football.column('Combined')
football = football.with_column('Pressure Drop', drop)
football = football.drop('Combined')
football.group('Team', np.average)

# 12.3.2
observed_means = football.group('Team', np.average).column(1)
# Evalutes to 0.733522727272728 
observed_difference = observed_means.item(1) - observed_means.item(0)

def difference_of_means(table, group_label):
    reduced = table.select('Pressure Drop', group_label)
    means_table = reduced.group(group_label, np.average)
    means = means_table.column(1)
    return means.item(1) - means.item(0)

difference_of_means(football, 'Team')

# 12.3.3
shuffled_labels = football.sample(with_replacement=False).column(0)
original_and_shuffled = football.with_column('Shuffled Label', shuffled_labels)

# 12.3.4
def one_simulated_difference():
    shuffled_labels = football.sample(with_replacement = False
                                                    ).column('Team')
    shuffled_table = football.select('Pressure Drop').with_column(
        'Shuffled Label', shuffled_labels)
    return difference_of_means(shuffled_table, 'Shuffled Label')

differences = make_array()

repetitions = 10000
for i in np.arange(repetitions):
    new_difference = one_simulated_difference()
    differences = np.append(differences, new_difference)

# 12.3.5
Table().with_column(
    'Difference Between Group Averages', differences).hist(
    left_end = observed_difference
)
plots.ylim(-0.1, 1.4)
plots.scatter(observed_difference, 0, color='red', s=30, zorder=3)
plots.title('Prediction Under the Null Hypothesis')
plots.savefig("./graphs/12_3_5_empirical_histogram.png")

# Evaluates to 0.0025
empirical_p = np.count_nonzero(differences >= observed_difference) / 10000
```

### 12.3: Deflategate
- Allegations that Patriots footballs from 2015 game were underinflated, potentially providing an advantage to the players
- Pressure is measured in pounds per square inch (psi)
- NFL rules state that balls must be inflated to pressures between 12.5 psi and 13.5 psi
- Observed data showed a lower average psi for the Patriots footballs and a greater drop in psi from the start of the game
- We need to define and test our hypotheses
#### 12.3.1: The Hypotheses
- In this scenario, nothing was selected at random, but we can make a chance model
- We hypothesize that the 11 Patriots' drops look like a random sample of 11 out of all the 15 drops, with the Colts' drops being the remaining four
- This is our **null hypothesis**
- Our alternative hypothesis is that the Patriots' drops are too large, on average to resemble a random sample drawn from all the drops
#### 12.3.2: Test Statistic
- Natural test statistic is the average between the two drops
  - `Average drop for Patriots - Average drop for Colts`
  - Note we do not take the absolute value here
  - Larger values of this statistic will favor the alternative hypothesis
- A positive value of this statistic reflects that the Patriots drops were greater than that of the Colts
- We can create a function to calculate the differences
#### 12.3.3: Predicting the Statistic Under the Null Hypothesis
- If the null hypothesis is true, it shouldn't matter which footballs are labeled which team, the distributions should be the same
- We can simulate this by randomly shuffling the team labels
- When we shuffle the labels, we can see that the group averages are closer than the original labeled averages
#### 12.3.4: Permutation Test
- We can now repeatedly simulate our test statistic under the null hypothesis
#### 12.3.5: Conclusion of the Test
- When calculating our empirical p-value, we need to recall that larger drops favor the alternative hypothesis
- So, the p-value is the chance of getting a test statistic equal to our greater than our observed value
- ![empirical histogram](/graphs/12_3_5_empirical_histogram.png)
- The observed value of our statistic is quite far from the heart of the distribution
- Our empirical p-value is very small, so we end up rejecting the null hypothesis