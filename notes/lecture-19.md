# Lecture 19: A/B Testing

## Reading
- 11.4: Error Probabilities
- 12.1: A/B Testing

### 12.1 Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plt

births = Table.read_table('./data/baby.csv')
smoking_and_birthweight = births.select('Maternal Smoker', 'Birth Weight')
# get counts by maternal smoker values
smoking_and_birthweight.group('Maternal Smoker')
# use .hist() with group argument (column label or index)
smoking_and_birthweight.hist('Birth Weight', group = 'Maternal Smoker')
plt.savefig("./graphs/12_1_1_weights_maternal_smoker.png")

# 12.1.3
means_table = smoking_and_birthweight.group('Maternal Smoker', np.average)
means = means_table.column(1)
# Evaluates to about -9.27 ounces
observed_difference = means.item(1) - means.item(0)

def difference_of_means(table, group_label):
    """Takes: name of table,
    column label that indicates the group to which the row belongs
    Returns: Difference of mean birth weights of the two groups"""
    reduced = table.select('Birth Weight', group_label)
    means_table = reduced.group(group_label, np.average)
    means = means_table.column(1)
    return means.item(1) - means.item(0)

# Evaluates to the same as `observed_difference` above
difference_of_means(births, 'Maternal Smoker')

# 12.1.4
shuffled_labels = smoking_and_birthweight.sample(with_replacement = False).column(0)
original_and_shuffled = smoking_and_birthweight.with_column('Shuffled Label', shuffled_labels)
shuffled_only = original_and_shuffled.select('Birth Weight','Shuffled Label')
shuffled_group_means = shuffled_only.group('Shuffled Label', np.average)

# Evaluates to -0.61635...
difference_of_means(original_and_shuffled, "Shuffled Label")
# Evaluates to -9.26614...
difference_of_means(original_and_shuffled, "Maternal Smoker")

def one_simulated_difference_of_means():
    """Returns: Difference between mean birthweights
    of babies of smokers and non-smokers after shuffling labels"""
    
    # array of shuffled labels
    shuffled_labels = births.sample(with_replacement=False).column('Maternal Smoker')
    
    # table of birth weights and shuffled labels
    shuffled_table = births.select('Birth Weight').with_column(
        'Shuffled Label', shuffled_labels)
    
    return difference_of_means(shuffled_table, 'Shuffled Label')

# 12.1.5
differences = make_array()

repetitions = 5000
for i in np.arange(repetitions):
    new_difference = one_simulated_difference_of_means()
    differences = np.append(differences, new_difference)

# 12.1.6
Table().with_column('Difference Between Group Means', differences).hist()
plt.title('Prediction Under the Null Hypothesis')
plt.savefig("./graphs/12_1_6_prediction_histogram.png")
print('Observed Difference:', observed_difference)

# Evaluates to 0.0
empirical_p = np.count_nonzero(differences <= observed_difference) / repetitions

# 12.1.7
smoking_and_age = births.select('Maternal Smoker', 'Maternal Age')
smoking_and_age.hist('Maternal Age', group = 'Maternal Smoker')
plt.savefig("./graphs/12_1_7_age_maternal_smoker.png")

def difference_of_means(table, group_label):
    """Takes: name of table,
    column label that indicates the group to which the row belongs
    Returns: Difference of mean ages of the two groups"""
    reduced = table.select('Maternal Age', group_label)
    means_table = reduced.group(group_label, np.average)
    means = means_table.column(1)
    return means.item(1) - means.item(0)

observed_age_difference = difference_of_means(births, 'Maternal Smoker')

def one_simulated_difference_of_means():
    """Returns: Difference between mean ages
    of smokers and non-smokers after shuffling labels"""
    
    # array of shuffled labels
    shuffled_labels = births.sample(with_replacement=False).column('Maternal Smoker')
    
    # table of ages and shuffled labels
    shuffled_table = births.select('Maternal Age').with_column(
        'Shuffled Label', shuffled_labels)
    
    return difference_of_means(shuffled_table, 'Shuffled Label')

age_differences = make_array()

repetitions = 5000
for i in np.arange(repetitions):
    new_difference = one_simulated_difference_of_means()
    age_differences = np.append(age_differences, new_difference)

Table().with_column(
    'Difference Between Group Means', age_differences).hist(
    right_end = observed_age_difference)
# Plotting parameters; you can ignore the code below
plt.ylim(-0.1, 1.2)
plt.scatter(observed_age_difference, 0, color='red', s=40, zorder=3)
plt.title('Prediction Under the Null Hypothesis')
plt.savefig("./graphs/12_1_7_age_prediction_histogram.png")
print('Observed Difference:', observed_age_difference)
```

### 12.1 A/B Testing
- Deciding whether two numerical samples come from the same distribution is called A/B testing
#### 12.1.1: Smokers and Nonsmokers
- The aim of a particular study is to see whether maternal smoking is associated with birth weight
- Can start with two variables, "Birth Weight" and "Maternal Smoker"
- ![birth weight maternal smoker](/graphs/12_1_1_weights_maternal_smoker.png)
- Based on our histogram data, the distribution for mothers who smoked appears to be biased slightly to the left of non-smoking mothers
- Is this change due to chance or due to a difference in the larger population?
#### 12.1.2: The Hypotheses
- The chance model says there is no underlying difference, any variations in samples are just due to chance
  - This is what we call the *null hypothesis*
- Our alternative hypothesis is that babies of mothers who smoke have lower birth weights, on average
#### 12.1.3: Test Statistic
- Our statistic is to use the difference between the two group birth weight means
- We will subtract the average weight of smoking group from the average weight of the non-smoking group
- This will be formatted as `average weight of the smoking group - average weight of the non-smoking group`
  - Larger negative values will favor the alternative hypothesis
  - Note the directionality, it is important in this particular example
- We can create a function to compute our statistic
#### 12.1.4: Predicting the Statistic Under the Null Hypothesis
- We want to test our prediction against the null hypothesis, so we can create *random permutations* by randomly shuffling the labels among all mothers
  - If there is no difference in the underlying population, then we should see no difference in the average with the shuffled labels
- Shuffling ensures the count of True & False labels does not change
- Our simulated statistic is the difference between the mean weight of babies of mothers who were randomly assigned as "smokers" and the mean weight of mothers who were randomly assigned as "non-smokers"
- Use the `sample()` method with `with_replacement=False` to create a set of randomly selected column labels
  - By default, `sample()` draws as many times as there are rows in the table
- Calculate our statistic to see if there are differences between the statistic of the shuffled labels and the original labels
- We need to simulate our statistic many times
#### 12.1.5: Permutation Test
- Tests based on random permutations are called *permutation tests*
- We want to simulate our test statistic many times and collect the results of all of our differences in an array
#### 12.1.6: Conclusion of the Test
- ![prediction histogram](/graphs/12_1_6_prediction_histogram.png)
- Our distribution is roughly centered around 0, while our observed statistic is about -9.27
- This means our observed value and the predicted behavior of the statistic under the null hypothesis are inconsistent
- We can compute our p-value as well, low values of the statistic favor the alternative hypothesis
  - `empirical_p = np.count_nonzero(differences <= observed_difference) / repetitions`
- None of our 5,000 permuted samples had a difference of -9.27 or lower, but this is an approximation, the exact chance of getting that value is not 0 (but it is vanishingly small)
- Therefore, we reject the null hypothesis
#### 12.1.7: Another Permutation Test
- We can use the same permutation test to compare other attributes of the smokers and non-smokers, such as age
- ![age and smoking status](/graphs/12_1_7_age_maternal_smoker.png)
- We can rewrite our code to now compare ages of smokers and non-smokers
- Our data shows that smokers are younger on average, but is this due to chance or some underlying difference in the population?
- We can use a permutation test to answer this question, following the same process as before
- ![age prediction histogram](/graphs/12_1_7_age_prediction_histogram.png)
- Our empirical p-value is statistically significant and ends up showing that our test supports the alternate hypothesis that smokers were younger on average