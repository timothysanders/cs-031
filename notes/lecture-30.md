# Lecture 30: Linear Regression

## Reading
- 15.2: The Regression Line

### Python code
```python
import numpy as np
import matplotlib.pyplot as plots
from datascience import *

def standard_units(any_numbers):
    return (any_numbers - np.mean(any_numbers))/np.std(any_numbers)

def correlation(t, label_x, label_y):
    return np.mean(standard_units(t.column(label_x))*standard_units(t.column(label_y)))

def slope(t, label_x, label_y):
    r = correlation(t, label_x, label_y)
    return r*np.std(t.column(label_y))/np.std(t.column(label_x))

def intercept(t, label_x, label_y):
    return np.mean(t.column(label_y)) - slope(t, label_x, label_y)*np.mean(t.column(label_x))

def fit(table, x, y):
    """Return the height of the regression line at each x value."""
    a = slope(table, x, y)
    b = intercept(table, x, y)
    return a * table.column(x) + b

baby = Table.read_table('./data/baby.csv')
baby.scatter('Maternal Height', 'Maternal Pregnancy Weight', fit_line=True)
plots.savefig("./graphs/15_2_8_maternal_pregnancy_weight.png")
slope(baby, 'Maternal Height', 'Maternal Pregnancy Weight')
```

### 15.2: The Regression Line
- The correlation coefficient helps identify the straight line about which points are clustered
#### 15.2.1: Measuring in Standard Units
- Linear association doesn't depend on units of measurement, so variables can be measured in standard units
#### 15.2.2: Identifying the Line in Standard Units
- The slope of the line for our linear regression is equal to $r$
- The formula for $r$ (correlation) is as follows
  - $r = \frac{\text{Cov}(X, Y)}{\sigma_X \cdot \sigma_Y}$
- The version of the formula shown below shows the full steps to calculating covariance
  - $r = \frac{1}{n} \frac{ \sum\limits_{i=1}^n \left( x_i - \bar{x} \right) \left( y_i - \bar{y} \right)}{\sigma_X \cdot \sigma_Y}$
- In non-formula terms the equation is
  1. Compute the means $\bar{x}$ and $\bar{y}$
  2. Subtract the mean of $X$ from each value in $X$, and the mean of $Y$ from each value in $Y$, to get the deviations from the means.
  3. Multiply the corresponding deviations of $X$ and $Y$.
  4. Take the sum of these products.
  5. Divide the sum by $n$ (the number of data points).
#### 15.2.3: The Regression Line, in Standard Units
#### 15.2.4: The Regression Effect
- In general, individuals who are away from the average on one variable are expected to be not quite as far away from average on the other
  - This is called the *regression effect*
- The regression effect is a statement about averages
#### 15.2.5: The Equation of the Regression Line
- In regression, we use the value of one variable ($x$) to predict the value of another ($y$)
- When measured in standard units, the regression line for predicting $y$ based on $x$ has a slope of $r$ and passes through the origin
- In equation form, this is
  - $\text{estimate of y} = r * x$ when both variables are measured in standard units
- In original units, this is 
  - $\frac{\text{estimate of } y - \text{average of } y}{\text{SD of } y} = r \times \frac{\text{the given } x - \text{average of } x}{\text{SD of } x}$
- The slope and intercept of the regression line are defined as
  - $\text{slope of regression line} = r * \frac{\text{SD of } y}{\text{SD of } x}$
  - $\text{intercept of the regression line} = \text{average of } y - \text{slope} * \text{average of } x$
#### 15.2.6: The Regression Line in the Units of the Data
- The equation of the regression line in original units (predicting $y$ based on $x$)
  - estimate of y = slope * x + intercept
  - If slope is $m$ and intercept is $b$
  - $y = mx + b$
#### 15.2.7: Fitted Values
- The predictions all lie on the line and are known as *fitted values*
- Can use the `fit_line=True` parameter on the table method `scatter()`
#### 15.2.8: Units of Measurement of the Slope
- ![maternal pregnancy weight](/graphs/15_2_8_maternal_pregnancy_weight.png)
- The slope of the diagram above is **3.57 pounds per inch**
  - This means that someone how is one inch taller would be expected to be 3.57 pounds heavier
#### 15.2.9: Example
- Suppose observed correlation $r$ is 0.5 and summary stats are as shown below

|        | average   | SD        |
|--------|-----------|-----------|
| height | 14 inches | 2 inches  |
| weight | 50 pounds | 5 pounds  |
- To calculate the equation of the regression line, we need the slope and the intercept
- Our equation ends up being
  - $estimated height = 0.2 * given weight + 4$
- In general the slope of the regression line can be interpreted as the average increase in $y$ per unit increase in $x$
- If the slope is negative, then for every unit increase in $x$, the average of $y$ decreases
