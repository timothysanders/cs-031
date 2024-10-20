# Lecture 18: Decisions & Uncertainty

## Readings
- 11.3: Decisions and Uncertainty
- 11.4: Error Probabilities

### 11.3 Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plt

# 11.3.3
# Evaluates to 0.8880516684607045
# Remember, this is a percent, not a proportion!
observed_statistic = np.abs( 100 * (705 / 929) - 75)

# 11.3.4
mendel_proportions = make_array(0.75, 0.25)
mendel_proportion_purple = mendel_proportions.item(0)
sample_size = 929

# This returns the percent difference, not a proportion
def one_simulated_distance():
    sample_proportion_purple = sample_proportions(929, mendel_proportions).item(0)
    return 100 * abs(sample_proportion_purple - mendel_proportion_purple)

repetitions = 10000
distances = make_array()
for i in np.arange(repetitions):
    distances = np.append(distances, one_simulated_distance())

Table().with_column(
    'Distance between Sample % and 75%', distances
).hist()
plt.title('Prediction Made by the Null Hypothesis')
plt.savefig("./graphs/11_3_4_null_hypothesis_predictions.png")

# 11.3.5
Table().with_column(
    'Distance between Sample % and 75%', distances
).hist()
plt.ylim(-0.02, 0.5)
plt.title('Prediction Made by the Null Hypothesis')
plt.scatter(observed_statistic, 0, color='red', s=40)
plt.savefig("./graphs/11_3_5_null_hypothesis_predictions.png")

different_observed_statistic = 3.2
Table().with_column(
    'Distance between Sample % and 75%', distances
).hist()
plt.ylim(-0.02, 0.5)
plt.title('Prediction Made by the Null Hypothesis')
plt.scatter(different_observed_statistic, 0, color='red', s=40)
plt.savefig("./graphs/11_3_6_non_clear_cut.png")

# 11.3.7
# Evaluates to ~0.023
np.count_nonzero(distances >= different_observed_statistic) / repetitions
```
### 11.3: Decisions and Uncertainty
- Our statistical and computational methodology of assessing models about jury selection fit into a framework of decision-making that is called *statistical tests of hypotheses*
- Using statistical tests is a standard way to make decisions in many fields, it has a standard terminology
#### 11.3.1: Mendel's Model
- For every plant, there is a 75% chance it will have purple flowers, and a 25% chance that the flowers will be white, regardless of the colors in all the other plants
- Original data set was 929 pea plants, 705 had purple flowers
#### 11.3.2: Step 1: The Hypotheses
- All statistical tests attempt to choose between two views of the world, specifically two views about how the data are generated
  - These views are called *hypotheses*
- `the null hypothesis`: this is a clearly defined model about chances, it says the data were generated at random under clearly specified assumptions about the randomness
  - The word *null* is meant to reinforce the idea that if the data look different from what the hypothesis predicts, that difference is due to *nothing* but chance
- From a practical perspective, **the null hypothesis is a hypothesis under which you can simulate data**
- In the example of Mendel's model, the null hypothesis is that the assumptions of his model are good, each plant has a 75% chance of having purple flowers, regardless of what any other plant has
- Under this hypothesis, we can simulate random sample data using the `sample_proportions()` function
- `the alternative hypothesis`: this says that some reason other than chance made the data differ from the predictions of the model in the null hypothesis
- In the example of Mendel's model, the alternative hypothesis is that his model isn't good
  - Remember, the alternative doesn't say *why or how* the model isn't good
#### 11.3.3: Step 2: The Test Statistic
- We have to choose a statistic that we can use to make a decision, this is our **test statistic**
- In this example, we want to see if the two distributions are close to each other, so we use the total variation distance (TVD)
- With just two categories, the TVD is equal to the distance between the two proportions in one category
- Our test statistic is the distance between the sample percent of purple plants and 75% (the corresponding percent in Mendel's model)
  - `|sample percent of purple-flowering plants - 75|`
  - Small values of the distance (positive or negative) will be more consistent with the null hypothesis
  - Big values of the distance (positive or negative) will be more consistent with the alternative
- To choose a test statistic in other situations, look at the alternative hypothesis
  - What values of the statistic will make you think the alternative hypothesis is a better choice than the null?
    - Your answer can be positive or negative, but "both" is bad
#### 11.3.4: Step 3: The Distribution of the Test Statistic, Under the Null Hypothesis
- The main computational aspect of a test of hypotheses is figuring out what the model in the null hypothesis predicts
- We have to figure out what the values of the test statistic might be if the null hypothesis were true
- Test statistic is simulated based on the assumptions of the model in the null hypothesis
- We simulate our statistic repeatedly to get a good sense of its possible values and which are more likely than others
  - This gives us a probability distribution of the statistic
- ![prediction distances](/graphs/11_3_4_null_hypothesis_predictions.png)
- Our horizontal axis shows us the typical values of the distance, as predicted by the model
  - A high proportion of the differences are between 0 - 1% (between 74% and 76%)
#### 11.3.5: Step 4: The Conclusion of the Test
- Choice between null and alternative hypothesis depends on the comparisons between what was calculated in steps 2 and 3
  - The observed values of the test statistic and its distribution as predicted by the null hypothesis
  - If the two are not consistent with each other, our data does not support the null hypothesis
    - We then say our test *rejects* the null hypothesis
  - If the two are consistent, the null hypothesis is supported by our data
    - We then say our data are *consistent* with the null hypothesis
- ![observed with predictions](/graphs/11_3_5_null_hypothesis_predictions.png)
- The observed statistic fits well within the typical data predicted by the null hypothesis
- Our test concludes the data is consistent with Mendel's model
#### 11.3.6: The Meaning of "Consistent"
- The examples shown so far (jury/Mendel) have been fairly clear-cut
- It's important to understand whether the observed test statistic is consistent with its predicted distribution under the null hypothesis is a matter of subjective opinion and judgement
- But what happens when we have an example that is not so clear-cut?
- ![observed non clear-cut](/graphs/11_3_6_non_clear_cut.png)
#### 11.3.7: Conventional Cut-offs and the P-value
- There are some conventions we can follow to help us make these calls
- Conventions are based on the area in the tail
- We look between the observed statistic (red dot in our example) and the direction that makes us lean towards the alternative
- In our examples, big distances favor the alternative (model is no good)
- If the area of the tail is small, the observed statistic is far away from the values most commonly predicted by the null hypothesis
- To find the area in the tail, we have to find the percent of distances that were grater than or equal to 3.2 (using our last example)
- `np.count_nonzero(distances >= different_observed_statistic) / repetitions`
- The result of this calculation tells us that there is ~2.4% chance that the test statistic is 3.2 or more
- This is not a big chance...
##### 11.3.7.1: The p-value
- The calculated chance we just found is called the *observed significance level* of the test
- More commonly referred to as the *p-value* of the test
- `p-value`: The p-value of a test is the chance, based on the model in the null hypothesis, that the test statistic will be equal to the observed value in the sample or even further in the direction that supports the alternative.
- If the p-value is small, this means that it is on the far end of what the null hypothesis predicts
  - This means the data support the alternative hypothesis more than they support the null
- By convention...
  - If the p-value is less than 5%, it is considered small and the result is called “statistically significant.”
  - If the p-value is even smaller – less than 1% – the result is called “highly statistically significant.”
- Our calculated p-value of ~2.4% is considered small, so by convention, we would reject the null hypothesis and say Mendel's model does not look good
  - Formally, the result of the test is statistically significant
- You should always provide the observed statistic and the p-value as well
#### 11.3.8: Historical Note on the Conventions
- Determination of statistical significance has become standard in statistical analyses
- Developed by Sir Ronald Fisher
- Remember that "low" is a matter of judgement with no fixed definition
- Things to keep in mind
  - Always provide the observed value of the test statistic and the p-value, so that readers can decide whether or not they think the p-value is small. 
  - Don’t look to defy convention only when the conventionally derived result is not to your liking. 
  - Even if a test concludes that the data don’t support the chance model in the null hypothesis, it typically doesn’t explain why the model doesn’t work. Don’t make causal conclusions without further analysis, unless you are running a randomized controlled trial.

### 11.4: Error Probabilities
- Our last step in deciding which of our two hypotheses is better supported by data, is a judgement about the consistency of the data and the null hypothesis
- This step may lead us astray, as chance variation can cause a sample to look quite different from what the null hypothesis predicts
#### 11.4.1: Wrong Conclusions
- If you are testing a null hypothesis against the alternative that the null isn't true, there are four ways of classifying reality and the result of the test

|                     | Test Favors the Null | Test Favors the Alternative |
|---------------------|----------------------|-----------------------------|
| Null is True        | Correct Result       | Error                       |
| Alternative is True | Error                | Correct Result              |
- One type of error occurs if the test favors the alternative hypothesis when the null hypothesis is in fact true
- The other type of error is if the test favors the null hypothesis when the alternative is true
- Since the null hypothesis is a completely specified chance model, the first type of error has a chance that we can estimate
  - This answer turns out to be the cutoff that we use for the p-value
#### 11.4.2: The Chance of an Error
- Example is testing whether a coin is fair
  - Null: the coin is fair, results are like draws made at random (with replacement) from [Heads, Tails]
  - Alternative: coin is not fair
- Test statistic is `| number of heads - 1000 |` (repetitions=2000)
  - Small values favor null hypothesis, large values favor the alternative
- Assuming we ran this experiment 2000 times and our original observed statistic was 45
  - `np.count_nonzero(statistics >= 45) / repetitions`
  - Area to the right of 45 is ~0.04654
- If we use a 5% cutoff for the p-value, our decision rule would conclude that the coin is unfair if the test statistic comes out to be 45 or more
- However, this result can actually happen
- This means that if we use our 5% cutoff, we have a 5% chance of concluding the coin is unfair, even if the coin is fair
#### 11.4.3: The Cutoff for the p-value is an Error Probability
- *If you use a p% cutoff for the p-value, and the null hypothesis happens to be true, then there is about a p% chance that your test will conclude that the alternative is true.*
- Using our table previously, the p-value is the probability of the null hypothesis being true, but the test favoring the alternative
##### 11.4.3.1: Controlling for the Error
- The 1% cutoff for p-value is more conservative and has less of a chance of concluding the alternative if the null is true
  - Randomized control trials of medical treatments usually use 1% as the cutoff for deciding between their hypotheses
    - Null: Treatment has no effect; observed differences between treatment and control are due to randomization
    - Alternative: The treatment has an effect
- However, even if you set the p-value to 1%, there is still a chance you reject the null hypothesis incorrectly
#### 11.4.4: Data Snooping and p-Hacking
- Because we can wrongly reject hypotheses, it is important that experiments are replicated
  - This means other researchers should carry out the experiment and see if they get similar results
- If you are testing many different hypotheses at once, this could lead to *data snooping* or *p-hacking*, which is "torturing the data into making a false confession"
#### 11.4.5: Technical Note: The Other Kind of Error
- Another error is concluding the treatment does nothing when it actually does something
- Remember, if you set up your test to reduce one of the two errors, you almost always increase the other one