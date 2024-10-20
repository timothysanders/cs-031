# Lecture 17: Assessing a Model

## Reading
- 11.1: Assessing a Model
- 11.2: Multiple Categories

### 11.1 Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plt

# 11.1.4.1
sample_size = 100
eligible_population = [0.26, 0.74]
sample_proportions(sample_size, eligible_population).item(0)

def one_simulated_count():
    return sample_size * sample_proportions(sample_size, eligible_population).item(0)

# 11.1.4.2
counts = make_array()
repetitions = 10000
for i in np.arange(repetitions):
    counts = np.append(counts, one_simulated_count())

# 11.1.5
Table().with_column(
    'Count in a Random Sample', counts
).hist(bins = np.arange(5.5, 46.6, 1))
plt.savefig("./graphs/11_1_5_count_in_random_sample.png")

# 11.1.6
Table().with_column(
    'Count in a Random Sample', counts
).hist(bins = np.arange(5.5, 46.6, 1))
plt.ylim(-0.002, 0.09)
plt.scatter(8, 0, color='red', s=30);
plt.savefig("./graphs/11_1_6_count_in_random_sample.png")
```

### 11.1: Assessing a Model
- A *model* is a set of assumptions about data, often with assumptions about chance processes to generate data
- Data scientists occasionally have to decide if a model is good

#### 11.1.1: Jury Selection
- Data can help provide evidence of things like bias in jury selection
- An important characteristic of an impartial jury is that it is *representative* of the broader population
- A panel is a group of people chosen to be potential jurors, the final jury is selected from this panel

#### 11.1.2: A Model of Random Selection
- One view of the data, or a model, is that the jury panel was selected at random and had a small number of Black panelists by chance
- The model is random selection specifies the details of the chance process
- We can simulate data based on the model, that is, drawing at a random from a population of whom 26% are Black
- Our simulation can show what panels would look like if they were selected at random
- Then compare the results of the simulation to the composition of the actual panel
- If the results of the simulation are not consistent with the composition of the panel in the trial, this is evidence against the model of random selection

#### 11.1.3: The Statistic
- First we must choose our statistic to simulate
- The model of random selection says the jury panel was selected at random, an alternative model is that there are too few Black panelists to have been drawn at random
- Our statistic in this instance can be the number/count of Black panelist in the sample

#### 11.1.4: Simulating the Statistic Under the Model
- If the model is true, how big would our statistic typically be?
- We will simulate this and look at the distribution of the results
##### 11.1.4.1: Simulating One Value of the Statistic
- Use the `sample_proportions()` function to simulate one value
- Categories in the output array are the same as the order of the categories in the input array
- Count in each category is the sample size times the corresponding proportion
##### 11.1.4.2: Simulating Multiple Values of the Statistic
- Our analysis focuses on the variation in the counts
- We can generate a number of simulations of our statistic and see how they vary
#### 11.1.5: The Prediction Under the Model of Random Selection
- The histogram shows what our model of random selection predicts
- ![count_in_random_sample.png](/graphs/11_1_5_count_in_random_sample.png)
#### 11.1.6: Comparing the Prediction and the Data
- We can redraw our histogram with our original value shown as a red dot
- ![count_in_random_sample](/graphs/11_1_6_count_in_random_sample.png)
#### 11.1.7: Conclusion of the Data Analysis
- Our graph shows the evidence of bias in the selection process, because we are very unlikely to get the low count of Black panelists that we originally observed
- This is evidence that the model of random select is *not* consistent with the data from the panel, it is highly unlikely
- The most reasonable conclusion is that the assumption of random selection is not reasonable for this jury panel
#### 11.1.8: Statistical Bias
- This analysis provides quantitative evidence of unfairness in Robert Swain's trial
- When a process produces data consistently skewed in one direction, data scientists say the data is *biased*

### 11.2 Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plt
plt.style.use('fivethirtyeight')
jury = Table().with_columns(
    'Ethnicity', make_array('Asian/PI', 'Black/AA', 'Caucasian', 'Hispanic', 'Other'),
    'Eligible', make_array(0.15, 0.18, 0.54, 0.12, 0.01),
    'Panels', make_array(0.26, 0.08, 0.54, 0.08, 0.04)
)

jury.barh("Ethnicity")
plt.savefig("./graphs/11_2_2_ethnicity.png")

# 11.2.3
eligible_population = jury.column('Eligible')
sample_distribution = sample_proportions(1453, eligible_population)
panels_and_sample = jury.with_column('Random Sample', sample_distribution)
panels_and_sample.barh('Ethnicity')
plt.savefig("./graphs/11_2_3_ethnicity.png")

# 11.2.4
# Augment the table with a column of differences between proportions
jury_with_diffs = jury.with_column(
    'Difference', jury.column('Panels') - jury.column('Eligible')
)
jury_with_diffs = jury_with_diffs.with_column(
    'Absolute Difference', np.abs(jury_with_diffs.column('Difference'))
)
jury_with_diffs.column('Absolute Difference').sum() / 2

# 11.2.5
def total_variation_distance(distribution_1, distribution_2):
    return sum(np.abs(distribution_1 - distribution_2)) / 2

sample_distribution = sample_proportions(1453, eligible_population)
total_variation_distance(sample_distribution, eligible_population)

def one_simulated_tvd():
    sample_distribution = sample_proportions(1453, eligible_population)
    return total_variation_distance(sample_distribution, eligible_population)

tvds = make_array()
repetitions = 5000
for i in np.arange(repetitions):
    tvds = np.append(tvds, one_simulated_tvd())

# 11.2.6
Table().with_column('TVD', tvds).hist(bins=np.arange(0, 0.2, 0.005))

# Plotting parameters; you can ignore this code
plt.title('Prediction Assuming Random Selection')
plt.xlim(0, 0.15)
plt.ylim(-5, 50)
plt.scatter(0.14, 0, color='red', s=30)
plt.savefig("./graphs/11_2_6_assesing_model.png")
```
### 11.2: Multiple Categories
- Now we can consider panelists in multiple racial and ethnic categories, not just two
- General process is the same as before, but we will need a new statistic to simulate
#### 11.2.1: Jury Selection in Alameda County
- ACLU report from 2010 reported certain racial and ethnic groups are underrepresented amount jury panelists
- Jury panel is supposed to be representative/selected at random
#### 11.2.2: Composition of Panels in Alameda County
- ACLU gathered data on composition of jury panels
- We can visualize this data with a bar chart
- ![ethnicities](/graphs/11_2_2_ethnicity.png)
#### 11.2.3: Comparison with Panels Selected at Random
- How do we go about comparing a random sample with the distributions of our observed and expected panels?
- To do this, we can use `sample_proportions()` and augment our existing `jury` table
- We can sample 1453 times (to get our panel) and display the distribution along with the distributions of eligible jurors/panel in data
- ![ethnicities](/graphs/11_2_3_ethnicity.png)
- We see that our bar chart more closely resembles our eligible population than it does the distribution on the panels
- We can simulate this distribution many times, but we need a statistic to assess..
#### 11.2.4: A New Statistic: The Distance between Two Distributions
- We already know how to measure how different two numbers are, by taking the absolute value of their difference
- We can do the same for a distribution by using *total variation distance*
- To calculate this, we find the difference between the two proportions in each category
- The sum of the differences in the column "Difference" is 0, because the proportions in each column add up to 0, so the give-and-take between their entries must add up to zero
- Drop the negative signs, add all the entries, then divide by two
- Our quantity is the *total variation distance* between the two distributions
- In general, this statistic measures how close distributions are, the larger the TVD, the more different the distributions are
- We can use this statistic to simulate under the assumption of random selection
- Large values of this metric are evidence against random selection
#### 11.2.5: Simulating the Statistic Under the Model
- A function can help us calculate our TVD easily in each simulation repetition
- Use the `sample_proportions()` function to generate a random sample from the eligible population
##### 11.2.5.1: Simulating One Value of the Statistic
- We can create a function that gives us one simulated total variation distance
- This can then be repeated in a `for` loop
#### 11.2.6: Assessing the Model of Random Selection
- We can look at our empirical (observed) histogram of simulated distances
- ![assessing model](/graphs/11_2_6_assesing_model.png)
- We see that our panels in the study are well outside of the tail of the histogram
- Our simulation shows that the composition of the panels is not consisten with a model of random selection
#### 11.2.7: Reasons for the Bias
- Our model cannot say *why* specifically the distributions are different
- There may be many reasons why a particular distribution does not match, such as bad software, different selection criteria, etc.
#### 11.2.8: Data Quality
- Good data science requires a thoughtful analysis of our input data and how it was gathered
- Our estimate of the eligible population is itself an estimate and could have errors in it
- Other factors, like not all potential jurors reporting for service (due to many reasons) can impact our potential panel of jurors
#### 11.2.9: Conclusion
- Because of the limitations, we must be precise about what exactly we can conclude from our analysis
- "We can conclude that the distribution provided for the panelists who reported for service does not look like a random sample from the estimated distribution in the eligible population."
