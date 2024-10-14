# Lecture 15: Probability

## Reading
- 9.5: Finding Probabilities
- 10: Sampling and Empirical Distributions
- 10.1: Empirical Distributions

### 9.5: Finding Probabilities
- In this course, probabilities are relative frequencies
- By convention, probabilities are numbers between 0 and 1
  - Events that are impossible have a probability of 0
  - Events that are certain have a probability of 1
- Simulations can provide excellent approximations of probabilities
- Notation of probability an event happens is `P(event)`
  - Terms "chance" and "probability" are interchangeable
#### 9.5.1: When an Event Doesn't Happen
- If the probability of an event is 40%, then the probability it doesn't happen is 60%
  - `P(event doesn't happen) = 1 - P(event happens)`
#### 9.5.2: When all Outcomes are Equally Likely
- If all events are equally likely (such as a die roll), then the probabilities can be calculated as a ratio
#### 9.5.3: When two events must both happen
- If you have events that must happen in sequence, then you can think about the event in stages
- Example, three tickets in box (red, blue, green), what is probability of drawing green ticket, then red? (without replacement)
  - `P(green first, then red)`
    - `P(green first) == 1/3` - drawing one of the three tickets
    - `P(red next) == 1/2` - only two tickets are left
    - `P(green first, then red) == 1/3 * 1/2 == 1/6`
- This can be called **multiplication rule**
  - `P(two events both happen) = P(one event happens) * P(the other event happens, given that the first one happened)`
- This rule can apply to more than two events
- Generally, the more conditions that have to be satisfied, the less likely they are to all be satisfied
#### 9.5.4: When an Event Can Happen in Two Different Ways
- If you have two events that need to happen, but don't care which order
- In our ticket example, you can get a red and green ticket in two different ways, one `RG` and one `GR`
  - `P(RG) = 1/6` and `P(GR) = 1/6`
- This can be called the **addition rule**
  - `P(an event happens) = P(first way it can happen) + P(second way it can happen)`
  - When an event can happen in one of two different ways, the chance that it happens is a sum of chances, and is bigger than the chance of either of the individual ways
- This rule can apply to more than two events
#### 9.5.5: At Least One Success
- Example, tossing a coin twice
  - Four outcomes, HH, HT, TH, and TT
  - `P(getting at least one head in two tosses) = 3/4`
  - `P(getting at least one head in two tosses) = 1 - P(both tails) = 1 - 1/4 = 3/4`
- Using the multiplication rule
  - P(both tails) = 1/4 = 1/2 * 1/2 = (1/2)<sup>2</sup>
  - This can be extended to any number of events
  - P(at least one head in 17 tosses) = 1 - P(all 17 are tails) = 1 - (1/2)<sup>17</sup>
- Now, finding probability that a roll of a die is 6
  - P(at least one 6 in two rolls) = 1 - P(both rolls are not six) = 1 - (5/6)<sup>2</sup>
  - P(at least one 6 in 17 rolls) = 1 - (5/6)<sup>17</sup>
- Increasing the sample size increases the chance that an individual is selected

### 10: Sampling and Empirical Distributions
- Important part of data science is making conclusions based on data in random samples
  - To do this though, you need to understand what your random sample is
```python
import numpy as np
from datascience import *


top1 = Table.read_table('./data/top_movies_2017.csv')
top2 = top1.with_column('Row Index', np.arange(top1.num_rows))
top = top2.move_to_start('Row Index')

top.set_format(make_array(3, 4), NumberFormatter)
```
- Sampling Rows of a Table
  - Each row of a table represents an "individual", in this instance, each individual is a movie
  - The contents of a row are values of different variables measured on the same individual
- Deterministic Samples
  - When specifying which elements of a set you want, without any chance, this is a *deterministic sample*
  - For example, this can be done using `top.take(make_array(3, 18, 100))` or `top.where('Title', are.containing('Harry Potter'))`
  - These are not random samples
- Probability Samples
  - `population`: set of all elements from whom a sample will be drawn
  - `probability sample`: one for which it is possible to calculate, before the sample is drawn, the chance with which any subset of elements will enter the sample
  - In probability samples, all elements are not required to have the same chance of being chosen
- Random Sampling Scheme
  - Ex, choose two people from a population that consists of three people (A, B, and C)
    - Person A has probability of 1 of being chosen
    - Persons B and C have probability of .5 of being chosen
- Systematic Sample
  - One method of sampling might be to choose a random position in the list, then evenly spaced positions after that
  - This is called a *systematic sample*
  - A systematic sample can also be a probability sample
- Random Samples Drawn With or Without Replacement
  - Random sampling with replacement is the default behavior of `np.random.choice(array)`
  - Random sampling without replacement is when sampled individuals are not replaced in the population
    - Think of dealing from a deck of cards
    - To do this, use `np.random.choice(array, replace=False)`
- Convenience Samples
  - Drawing random samples requires care and precision

### 10.1: Empirical Distributions
- In data science, the word "empirical" means "observed"
- Empirical distributions are distributions of observed data
- Example is rolling a die multiple times and keeping track of which face appears
```python
import numpy as np
from datascience import *

die = Table().with_column("Face", np.arange(1, 7, 1))

# 10.1.1 A Probability Distribution
die_bins = np.arange(0.5, 6.6, 1)
die.hist(bins=die_bins)

# 10.1.2 Empirical Distributions
die.sample(10)
def empirical_hist_die(n):
    die.sample(n).hist(bins = die_bins)

# 10.1.3 Empirical Histograms
empirical_hist_die(10)
```
#### 10.1.1: A Probability Distribution
- Every face on the die has a probability of 1/6
- Histogram shows the distribution of probabilities over all possible faces
- Because all bars represent the same chance, distribution is call *uniform on the integers 1 through 6*
- Variables with successive values separated by the same fixed amount are called *discrete*
- Probability of each face is 1/6 (16.67%), width of each bar is 1, so the height of each bar is 16.67% per unit
#### 10.1.2: Empirical Distributions
- Previously described probability distribution is theoretical and is not based on observed data
- *Empirical distribution* is a distribution of observed data
- Can use a table method, `sample()` for sampling at random (with replacement) from a table
  - Argument is the sample size, can also pass `with_replacement=False` if the sample should be drawn without replacement
#### 10.1.3: Empirical Histograms
- With a small sample size, the histogram may not look much like our probability distribution
- Increasing the sample size starts to make the histogram look more like our theoretical distribution
#### 10.1.4: The Law of Averages
- `Law of Averages`: If a chance experiment is repeated independently and under identical conditions, then, in the long run, the proportion of times that an event occurs gets closer and closer to the theoretical probability of the event
- If an experiment is repeated a large enough number of times, we should see our empirical distribution be very close to the theoretical distribution/probability of the event