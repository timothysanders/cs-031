# Lecture 29: Correlation

## Reading
- 15: Prediction
- 15.1: Correlation

### Python Code
```python
import numpy as np
from datascience import *
import matplotlib.pyplot as plots

original = Table.read_table('./data/family_heights.csv')

heights = Table().with_columns(
    'MidParent', np.mean(original.column('father')),
    'Child', original.column('childHeight')
    )

# 15.1
hybrid = Table.read_table("./data/hybrid.csv")
hybrid.scatter("acceleration", "msrp")
plots.savefig("./graphs/15_1_acceleration_scatterplot.png")

hybrid.scatter('mpg', 'msrp')
plots.savefig("./graphs/15_1_mpg_scatterplot.png")

def standard_units(any_numbers):
    return (any_numbers - np.mean(any_numbers))/np.std(any_numbers) 

suv = hybrid.where('class', 'SUV')
Table().with_columns(
    'mpg (standard units)',  standard_units(suv.column('mpg')), 
    'msrp (standard units)', standard_units(suv.column('msrp'))
).scatter(0, 1)
plots.xlim(-3, 3)
plots.ylim(-3, 3)

# calculating r
x = np.arange(1, 7, 1)
y = make_array(2, 3, 1, 5, 2, 7)
t = Table().with_columns(
        'x', x,
        'y', y
    )
# convert variables to standard units
t_su = t.with_columns(
        'x (standard units)', standard_units(x),
        'y (standard units)', standard_units(y)
    )
# multiply each pair of standard units
t_product = t_su.with_column('product of standard units', t_su.column(2) * t_su.column(3))
# r is the average of the products
r = np.mean(t_product.column(4))

# the correlation function
def correlation(t, x, y):
    return np.mean(standard_units(t.column(x))*standard_units(t.column(y)))

correlation(t, "x", "y")
correlation(suv, "mpg", "msrp")

new_x = np.arange(-4, 4.1, 0.5)
nonlinear = Table().with_columns(
        'x', new_x,
        'y', new_x**2
    )
nonlinear.scatter('x', 'y', s=30, color='r')
plots.savefig("./graphs/15_1_6_nonlinear_association.png")
```

### 15: Prediction
- Important aspect of data science is what can data tell us about the future?
- Data scientists have a number of methods for making predictions
- One of the most common is a *regression*, this is where our prediction lies approximately at the vertical center of points

### 15.1: Correlation
- *Linear association* is a measure of how tightly clustered a scatter diagram is about a straight line
- ![acceleration vs msrp](/graphs/15_1_acceleration_scatterplot.png)
- These data points show a positive association, the line is sloping upwards, indicating cars with greater acceleration tend to cost more
- A scatter plot of MSRP vs mileage shows a negative association
- ![mpg vs msrp](/graphs/15_1_mpg_scatterplot.png)
- We can attain useful information about the general orientation and shape of a diagram even without paying attention to the units
- When we change data to standard units, we will still see the same associations, but the linear relation may not be exactly the same
#### 15.1.1: The correlation coefficient
- The *correlation coefficient* (usually denoted as $r$) is a number between -1 and 1
  - Measures the extent to which a scatter plot clusters around a straight line
  - $r$ = 1 if the scatter diagram is a perfectly straight line sloping upwards, $r$ = -1 if the scatter plot is a perfectly straight line sloping downwards
  - When $r$ = 0, we say the variables are *uncorrelated*
#### 15.1.2: Calculating $r$
- Formula for $r$
  - $r$ is the average of the products of the two variables, when both variables are measured in standard units
  - See code above for implementation
#### 15.1.3: Properties of $r$
- $r$ is a number with no units
- $r$ is unaffected by changing the units on either axis
- $r$ is unaffected by switching the axes
#### 15.1.5: Association is not Causation
- Correlation only measures association, it does not imply causation
- For example, there may be a correlation between weight and math ability, but this does not mean that doing math makes children heavier
- There may be confounding variables at play
#### 15.1.6: Correlation Measures *Linear* Association
- Correlation only measures linear association, there are other types of non-linear association, such as variables with a quadratic relation
- ![non-linear association](/graphs/15_1_6_nonlinear_association.png)
#### 15.1.7: Correlation is Affected by Outliers
- Outliers have a big effect on correlation
#### 15.1.8: Ecological Correlations Should be Interpreted with Care
- Correlations based on aggregated data can be misleading
- Correlations based on aggregates and averages are called *ecological correlations* and are frequently reported