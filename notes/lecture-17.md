# Lecture 17: Assessing a Model

## Reading
- 11.1: Assessing a Model
- 11.2: Multiple Categories

## Python Code
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
