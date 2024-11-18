# Lecture 23: Confidence Intervals

## Reading
- 13: Estimation
- 13.1: Percentiles
- 13.2: The Bootstrap

### Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plots

# 13.1.2 code
sizes = np.array([12,17, 6, 9, 7])
percentile(80, sizes)
# returns 12
array_one = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
assert percentile(51, array_one) == percentile(60, array_one)
# both return 6

# 13.1.2.2
scores_and_sections = Table.read_table("./data/scores_by_section.csv")
scores_and_sections.select("Midterm").hist(bins=np.arange(-0.5, 25.6, 1))
plots.savefig("./graphs/13_1_2_scores_and_sections.png")
# find 85th percentile
scores = scores_and_sections.column(1)
percentile(85, scores) # returns 22
# manual validation
sorted_scores = np.sort(scores_and_sections.column(1))
int_conversion = int(0.85 * 359) # 305.15 -> 305
sorted_scores.item(int_conversion)

# 13.2
sf2019 = Table.read_table("./data/san_francisco_2019.csv")
sf2019.take(3)
sf2019 = sf2019.where("Salary", are.above(15000))
sf_bins = np.arange(0, 726000, 25000)
sf2019.select("Total Compensation").hist(bins=sf_bins)
plots.savefig("./graphs/13_2_2_compensation_histogram.png")

pop_median = percentile(50, sf2019.column("Total Compensation"))
# returns 135747.0

our_sample = sf2019.sample(500, with_replacement=False)
our_sample.select('Total Compensation').hist(bins=sf_bins)
plots.savefig("./graphs/13_2_3_random_sample_compensation.png")
est_median = percentile(50, our_sample.column('Total Compensation'))
resample_1 = our_sample.sample()
resample_1.select("Total Compensation").hist(bins=sf_bins)
plots.savefig("./graphs/13_2_6_resampled_compensation_hist.png")
resampled_median_1 = percentile(50, resample_1.column('Total Compensation'))

def one_bootstrap_median():
    resampled_table = our_sample.sample()
    bootstrapped_median = percentile(50, resampled_table.column('Total Compensation'))
    return bootstrapped_median

num_repetitions = 5000
bootstrap_medians = make_array()
for i in np.arange(num_repetitions):
    bootstrap_medians = np.append(bootstrap_medians, one_bootstrap_median())

resampled_medians = Table().with_column('Bootstrap Sample Median', bootstrap_medians)
median_bins=np.arange(120000, 160000, 2000)
resampled_medians.hist(bins = median_bins)
parameter_green = '#32CD32'
plots.ylim(-0.000005, 0.00014)
plots.scatter(pop_median, 0, color=parameter_green, s=40, zorder=2)
plots.savefig("./graphs/13_2_7_bootstrapped_empirical_distribution.png")

left = percentile(2.5, bootstrap_medians)
right = percentile(97.5, bootstrap_medians)
resampled_medians.hist(bins = median_bins)
plots.ylim(-0.000005, 0.00014)
plots.plot([left, right], [0, 0], color='yellow', lw=3, zorder=1)
plots.scatter(pop_median, 0, color=parameter_green, s=40, zorder=2)
plots.savefig("./graphs/13_2_8_bootstrapped_medians_w_range.png")

def bootstrap_median(original_sample, num_repetitions):
    medians = make_array()
    for i in np.arange(num_repetitions):
        new_bstrap_sample = original_sample.sample()
        new_bstrap_median = percentile(50, new_bstrap_sample.column('Total Compensation'))
        medians = np.append(medians, new_bstrap_median)
    return medians

left_ends = make_array()
right_ends = make_array()

for i in np.arange(100):
    original_sample = sf2019.sample(500, with_replacement=False)
    medians = bootstrap_median(original_sample, 5000)
    left_ends = np.append(left_ends, percentile(2.5, medians))
    right_ends = np.append(right_ends, percentile(97.5, medians))

intervals = Table().with_columns(
    'Left', left_ends,
    'Right', right_ends
)
# tells us how many rows contain the pop_median value
num_successful_intervals = intervals.where(
    'Left', are.below(pop_median)).where(
    'Right', are.above(pop_median)
).num_rows
```

### 13: Estimation
- In this chapter, will investigate methods of estimating an unknown parameter
  - Remember, parameters are associated with a population
  - If we have all of the population, we can simply calculate the parameter value
  - However, if we don't have the entire population, but perhaps just a random sample, how do we estimate?
- A statistic from a random sample can be used to make a reasonable estimate of an unknown parameter in a population

### 13.1: Percentiles
- Numerical data can be sorted in increasing/decreasing order (values have a *rank order*)
- Percentile is the value at a particular rank
- Ties have to be taken into account when defining percentiles
- Must also exercise caution when relevant index isn't clear
  - For example, what is the 87th percentile in a set of ten values?
#### 13.1.1: A Numerical Example
#### 13.1.2: The percentile function
- This function takes two arguments, a rank between 0 and 100, and an array
- Returns the corresponding percentile of the array
##### 13.1.2.1: The General Definition
- If $p$ is a number between 0 and 100, the $p$th percentile of a collection is the smallest value in the collection that is at least as large as p% of all the values
- This means that any percentile value can be computed for a particular collection of values
- If there are $n$ elements in the collection, to find the $p$th percentile
  - Sort collection in increasing order
  - Find p% of n: $k = (p/100) * n$
  - If $k$ is an integer, take the $k$th element of the sorted collection
  - If $k$ is not an integer, round up to the next integer, and take that element of the sorted collection
##### 13.1.2.2: Example
- ![midterm distribution](/graphs/13_1_2_scores_and_sections.png)

#### 13.1.3: Quartiles
- First quartile is 25th percentile, second quartile is median, third is 75th
- Distributions of scores are sometimes summarized by the "middle 50th" interval, between first and third quartiles
  - This is called the "Interquartile Range" (IQR)

### 13.2: The Bootstrap
- When calculating a statistic from a random sample, we know that this is likely not a good estimate of the overall population
  - It is one of numerous possible random samples, thus just one statistic of many
- What happens if you cannot draw another random sample from the population?
  - This is where bootstrapping comes in
- New random samples are created through a method called *resampling*
#### 13.2.1: Employee Comp in SF
- We will use compensation data to demonstrate bootstrapping/resampling
- ![total compensation histogram](/graphs/13_2_2_compensation_histogram.png)
#### 13.2.2: Population and Parameter
- Since our data set contains the entire population, we can simply calculate a parameter
- In practice, there is no reason to draw a sample to estimate the parameter, but we will use it as an example
#### 13.2.3: A Random Sample and an Estimate
- ![Random sample compensation histogram](/graphs/13_2_3_random_sample_compensation.png)
- The median of our sample will be our initial estimate
- Because we have a larger sample, the law of averages applies and the distribution of our sample resembles the distribution of the population
- We want to quantify the amount by which the estimate could vary across samples
- We need to get another random sample, without sampling again from the population
#### 13.2.4: The Bootstrap: Resampling from the Sample
- Because we have a large random sample from the population, we can resampling, or *sampling from the sample*
- Steps of the *bootstrap method*
  - Treat the original sample as if it were the population
  - Draw from the sample, at random with replacement, the same number of times as the original sample size
- Important to resample the same number of times as the original sample size
- Sampling with replacement is important as well, if we sampled without replacement, we would simply get our initial sample back
#### 13.2.5: Why the Bootstrap Works
- By the law of averages, our original sample is likely to resemble the population
- This means that the distribution of our resamples is likely to resemble the original sample (which resembles the population)
#### 13.2.6: A Resampled Median
- The `sample()` table method draws rows from a table with replacement by default
- ![Resampled compensation histogram](/graphs/13_2_6_resampled_compensation_hist.png)
- By resampling again and again, we can get a large number of estimates and an empirical distribution of the estimates
#### 13.2.7: Bootstrap Empirical Distribution of the Sample Median
- Use a `for` loop to run the function a number of times
- ![Bootstrapped empirical distribution](/graphs/13_2_7_bootstrapped_empirical_distribution.png)
- Our set of estimates should contain the population parameter
#### 13.2.8: Do the Estimates Capture the Parameter?
- To see if our population parameter is well within our empirical histogram, we can calculate the middle 95% of resampled medians
- ![bootstrap with ranges](/graphs/13_2_8_bootstrapped_medians_w_range.png)
- To see how frequently the interval contains the parameter, we need to run this whole process a number of times
- We will see approximately 95 of the intervals contain the parameter
- Simulation steps
  - Draw a large random sample from the population
  - Bootstrap your random sample and get an estimate from the new random sample
  - Repeat the bootstrap step thousands of times to get thousands of estimates
  - Pick the middle 95% interval of the estimates
- This process of estimation captures the parameter about 95% of the time
