# Lecture 16: Models

## Reading
- 10.2: Sampling from a Population
- 10.3: Empirical Distribution of a Statistic
- 10.4: Random Sampling in Python

### 10.2: Sampling from a Population
- Law of averages holds when random sample is drawn from individuals in a large population
- Use the "united.csv" file (containing flight delays for united airlines) to represent a population
### Python Code
```python
import numpy as np
from datascience import *

united = Table.read_table('cs-031/data/united.csv')

# show minimum delay
united.column("Delay").min()

# show maximum delay
united.column("Delay").max()

# histogram of delays
delay_bins = np.append(np.arange(-20, 301, 10), 600)
united.hist('Delay', bins = delay_bins, unit = 'minute')
plots.title('Population')

# 10.2.1 Empirical Distribution of the Sample
def empirical_hist_delay(n):
    united.sample(n).hist('Delay', bins = delay_bins, unit = 'minute')

# 10.3 Empirical distribution of a statistic
sample_1000 = united.sample(1000)
sample_1000.hist('Delay', bins = delay_bins, unit = 'minute')
plots.title('Sample of Size 1000')

# 10.3.3 Simulating a statistic
def random_sample_median():
    return np.median(united.sample(1000).column('Delay'))

medians = make_array()
for i in np.arange(5000):
    medians = np.append(medians, random_sample_median())
```

#### 10.2.1: Empirical Distribution of the Sample
- As the sample size increases, the empirical (observed) histogram more closely represents the population's histogram
- Most consistently visible discrepancies are among values that are rare in the population
#### 10.2.2: Convergence of the Empirical Histogram of the Sample
- "For a large random sample, empirical histogram of the sample represents the histogram of the population, with high probability"
- We can use this to justify large random samples in statistical inference
- Since large random samples likely represent populations from which they are drawn, quantities computed from these samples are likely to be close to the corresponding quantities in the population

### 10.3: Empirical Distribution of a Statistic
- Law of Averages implies that with high probability, empirical distribution of a large random sample will resemble the distribution of the population from which the sample was drawn
#### 10.3.1: Parameter
- Often we are interested in numerical quantities associated with a population
- Numerical quantities associated with a population are called *parameters*
- For example, the median delay for the population of flights is a parameter
#### 10.3.2: Statistic
- A *statistic* is a number computed from the data in a sample (versus a *parameter* in a population)
- The median delay of a sample of flights is a statistic
- However, this means the statistic from another sample **could be different**
- How different could the statistic be? One way to determine is to simulate the statistic many times and track the values
  - We can then use a histogram to display the distribution
#### 10.3.3: Simulating a Statistic
- Steps to simulate a statistic
  1. Decide which statistic to simulate
  2. Define a function that returns one simulated value of the statistic
  3. Decide how many simulated values to generate
  4. Use a **for** loop to generate an array of simulated values
#### 10.3.4: Visualization
- A histogram of our simulated data is called an *empirical histogram of the statistic*
- It displays the empirical (observed) distribution of the statistic
- Simulating a statistic can provide a good estimate of a parameter
#### 10.3.5: The Power of Simulation
- Due to computing limitations, we cannot generate every possible sample of 1000 flights
- The law of averages helps us know that the empirical histogram of a statistic is likely to closely resemble the probability histogram of the statistic
  - The sample must be large enough though and you must repeat the process numerous times
- Simulating repeatedly is a way to approximate probability distributions without figuring out probabilities mathematically or generating all possible samples
- These simulations are a powerful tool in data science

### 10.4: Random Sampling in Python
#### 10.4.1: Review: Sampling from a Population in a Table
- If sampling from a population of individuals in a table, can easily use `sample()` method
- By default, sample draws uniformly with replacement
- To sample without replacement, use `with_replacement=False`
- The `sample()` method gives you the whole sample in the order the rows were selected as a new table
```python
import numpy as np
from datascience import *

faces = np.arange(1, 7)
die = Table().with_columns('Face', faces)
die.sample(7)

actors = Table.read_table('actors.csv')
actors.sample(5, with_replacement=False)
```
#### 10.4.2: Review: Sampling from a Population in an Array
- If sampling from population of individuals with data as an array, use Numpy's `np.random.choice()`
- This method randomly selects elements from an array, default is to select with replacement
- Argument `replace=False` is used to draw the sample without replacement
- Function `np.random.choice()` gives you an array of the entire sequence of sampled elements
```python
import numpy as np
faces = np.array([1, 2, 3, 4, 5, 6])
# simulate 7 rolls of a die
np.random.choice(faces, 7)
```
#### 10.4.3: Sampling from a Categorical Distribution
- In some cases, we need proportions of sampled individuals in different categories
- If you have the entire sample, you can calculate these proportions
- Use the function `sample_proportions()` from the `datascience` library for this
- Meant for sampling with replacement from a categorical distribution
- Function takes two arguments
  - Sample size
  - Distribution of categories in the population, as a list or an array of proportions that adds up to 1
- The values returned by `sample_proportions()` are in the same order as the proportions passed to the function
```python
from datascience import *

# flower color proportions for red, pink, and white
species_proportions = [0.25, 0.5, .25]

sample_size = 300

# Distribution of sample
sample_distribution = sample_proportions(sample_size, species_proportions)
```