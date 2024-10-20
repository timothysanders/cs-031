# Lecture 20: Causality

## Reading
- 12.2: [Causality](https://inferentialthinking.com/chapters/12/2/Causality.html)

### 12.2 Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plots

bta = Table.read_table('./data/bta.csv')
bta.group('Group', np.sum)
bta.group('Group', np.average)

# 12.2.4
observed_proportions = bta.group('Group', np.average).column(1)
# Evaluates to 0.475
observed_distance = abs(observed_proportions.item(0) - observed_proportions.item(1))
def distance(table, group_label):
    reduced = table.select('Result', group_label)
    proportions = reduced.group(group_label, np.average).column(1)
    return abs(proportions.item(1) - proportions.item(0))

distance(bta, "Group")

# 12.2.5
shuffled_labels = bta.sample(with_replacement=False).column(0)
bta_with_shuffled_labels = bta.with_column('Shuffled Label', shuffled_labels)
distance(bta_with_shuffled_labels, 'Shuffled Label')

def one_simulated_distance():
    shuffled_labels = bta.sample(with_replacement = False
                                                    ).column('Group')
    shuffled_table = bta.select('Result').with_column(
        'Shuffled Label', shuffled_labels)
    return distance(shuffled_table, 'Shuffled Label')

distances = make_array()

repetitions = 20000
for i in np.arange(repetitions):
    new_distance = one_simulated_distance()
    distances = np.append(distances, new_distance)

# 12.2.6
Table().with_column('Distance', distances).hist(
    bins = np.arange(0, 0.7, 0.1), left_end = observed_distance)
# Plotting parameters; you can ignore the code below
plots.ylim(-0.1, 5.5)
plots.scatter(observed_distance, 0, color='red', s=40, zorder=3)
plots.title('Prediction Under the Null Hypothesis')
plots.savefig("./graphs/12_2_6_empirical_histogram.png")
# Evaluates to 0.00845
empirical_p = np.count_nonzero(distances >= observed_distance) / repetitions
```
### 12.2: Causality
- Methods for comparing two samples are extremely useful in analysis of randomized controlled experiments
- If observed differences are more marked than what we would predict purely due to chance, we have evidence of *causation*
#### 12.2.1: Treating Chronic Back Pain: A Randomized Controlled Trial
- In our example, 31 patients were randomized into treatment and control groups (15 treatment/16 control)
- Control group was given saline, trials were run double-blind
  - "Double-blind" means that both the doctors and the patients do not know which group people are in
- Though our data looks to show a positive result, we need to consider the possibility that our results are purely due to chance
#### 12.2.2: Potential Outcomes
- A *potential outcome* for each patient is that they are assigned to the treatment group or to the control group
#### 12.2.3: The Hypotheses
- Our overall question is whether the treatment does anything
- **Null hypothesis**: distribution of all 31 potential "treatment" outcomes is the same as that of all 31 potential "control" outcomes, the difference in the two samples is due to chance
- **Alternative hypothesis**: distribution of 31 potential "treatment" outcomes is different from that of the 31 control outcomes, meaning the treatment does something different than the control
  - Note that the alternative does not say what specifically causes the difference or that the treatment helps, just that it is different
- We will randomly permute our 31 values, with 16 in one group, 15 in the other, and see what the differences are
- This process is the same as what we did for A/B testing
#### 12.2.4: The Test Statistic
- If the two group proportions are very different from each other, then this is evidence for the alternative hypothesis
- Our test statistic will be the distance between the two group proportions, or the absolute value of the difference between them
- Large values of the statistic favor the alternative hypothesis
- Test statistic `| mean of treatment group - mean of control group |`
- We can define a function that does this calculation for us
#### 12.2.5: Predicting the Statistic Under the Null Hypothesis
- We will start by randomly permuting all group labels then adding them back to the original table
- After shuffling the labels, we can find the distance between the two proportions
- Now we can define a function that allows us to run our test any number of times
- We use a for loop to calculate the test statistic 20,000 times
- These are the same steps as what we did for the A/B testing
#### 12.2.6: Conclusion of the Test
- Our array `distances` contains the values of our test statistic simulated under the null hypothesis
- To visualize, we can create an empirical histogram
- ![empirical histogram](/graphs/12_2_6_empirical_histogram.png)
- To find our empirical p-value, we need to find the proportion of simulated statistics that were equal to or larger than our observed statistic
  - `empirical_p = np.count_nonzero(distances >= observed_distance) / repetitions`
- Our result is statistically significant and favors the alternative hypothesis over the null
- Our evidence supports the hypothesis that the treatment is doing something
#### 12.2.7: Causality
- Because the trial was randomized, our test is evidence that the treatment *causes* the difference
- The random assignment of patients to the two groups ensures there are no confounding variables to affect the conclusion of causality
- Without randomization, our test would point to an *association*, but it could not imply that the treatment had caused the change
#### 12.2.8: A Meta-Analysis
- Our results are valid for the 31 patients in the study, but we are interested in the population of *all possible patients*
- A meta-analysis is a process by which you identify all available studies on a treatment and then summarize the collated results
