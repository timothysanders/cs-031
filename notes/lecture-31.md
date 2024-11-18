# Lecture 31: Least Squares

## Reading
- 15.3: The Method of Least Squares
- 15.4 Least Squares Regression

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

little_women = Table.read_table('./data/little_women.csv')
little_women = little_women.move_to_start('Periods')
little_women.take(3)

correlation(little_women, 'Periods', 'Characters')

lw_with_predictions = little_women.with_column('Linear Prediction', fit(little_women, 'Periods', 'Characters'))
lw_with_predictions.scatter('Periods')
plots.savefig("./graphs/15_3_1_linear_prediction.png")

actual = lw_with_predictions.column('Characters')
predicted = lw_with_predictions.column('Linear Prediction')
errors = actual - predicted
lw_with_predictions.with_column('Error', errors)
lw_reg_slope = slope(little_women, "Periods", "Characters")
lw_reg_intercept = intercept(little_women, "Periods", "Characters")

def lw_rmse(slope, intercept):
    x = little_women.column("Periods")
    y = little_women.column("Characters")
    fitted = slope * x + intercept
    mse = np.mean((y - fitted) ** 2)
    #print("Root mean squared error:", mse ** 0.5)
    return mse ** 0.5

def lw_mse(any_slope, any_intercept):
    x = little_women.column("Periods")
    y = little_women.column("Characters")
    fitted = any_slope * x + any_intercept
    return np.mean((y - fitted) ** 2)

best = minimize(lw_mse)
print("slope from formula:        ", lw_reg_slope)
print("slope from minimize:       ", best.item(0))
print("intercept from formula:    ", lw_reg_intercept)
print("intercept from minimize:   ", best.item(1))

# 15.4

shotput = Table.read_table("./data/shotput.csv")
shotput.scatter("Weight Lifted")
plots.savefig("./graphs/15_4_shotput_weight.png")

shotput_slope = slope(shotput, "Weight Lifted", "Shot Put Distance")
shotput_intercept = intercept(shotput, "Weight Lifted", "Shot Put Distance")

def shotput_linear_mse(any_slope, any_intercept):
    x = shotput.column("Weight Lifted")
    y = shotput.column("Shot Put Distance")
    fitted = any_slope * x + any_intercept
    return np.mean((y - fitted) ** 2)

minimize(shotput_linear_mse)

fitted = fit(shotput, 'Weight Lifted', 'Shot Put Distance')
shotput.with_column('Best Straight Line', fitted).scatter('Weight Lifted')
plots.savefig("./graphs/15_4_best_straight_line.png")

def shotput_quadratic_mse(a, b, c):
    x = shotput.column('Weight Lifted')
    y = shotput.column('Shot Put Distance')
    fitted = a*(x**2) + b*x + c
    return np.mean((y - fitted) ** 2)

best = minimize(shotput_quadratic_mse)

x = shotput.column(0)
shotput_fit = best.item(0)*(x**2) + best.item(1)*x + best.item(2)
shotput.with_column('Best Quadratic Curve', shotput_fit).scatter(0)
```

### 15.3: The Method of Least Squares
- Does every scatter plot have a "best" line that goes through it?
- To figure this out, we need a definition of "best", which will not necessarily mean "perfect"
- Each estimate is typically off the true value by an amount, known as the *error*
- A reasonable criterion for a "best" line is for it to have the smallest possible overall error
#### 15.3.1: Error in Estimation
- For each point we predict on the scatter plot, there is an error of prediction calculated as the actual value minus the predicted value
  - This is the vertical distance between the point and the line, with a negative if the point is below the line
- ![linear prediction](/graphs/15_3_1_linear_prediction.png)
#### 15.3.2: Root Mean Squared Error
- We can use one overall measure of the rough size of the errors
- To avoid cancellation when measuring the rough size of the errors, we can take the mean of the squared errors rather than the mean of the errors themselves
- Taking the square root of the mean squared error gives the root mean squared error (RMSE)
- Taking the square root gives RMSE, which is in the same units as the variable being predicted
#### 15.3.3: Minimizing the Root Mean Squared Error
- To get estimates of $y$ based on $x$, you can use any line you want
- Every line has a root mean squared error of estimation
- "Better" lines have smaller errors, "bad" lines have larger error
- Is there a way we can minimize the RMSE among all lines?
- **The regression line is the unique straight line that minimizes the mean squared error of estimation among all straight lines**
##### 15.3.3.1: Numerical Optimization
- A line that minimizes RMSE is also a line that minimizes the squared error
- Fortunately, there are Python functions that do trial and error for us
- The `minimize()` function can be used to find arguments of a function for which the function returns its minimum value
- This function takes a function as an argument (the function must take and return numerical arguments) 
#### 15.3.4: The Least Squares Line
- The regression line is sometimes called the "least squares line" because it is the only line that minimizes mean squared error

### 15.4: Least Squares Regression
- The slope and intercept of the least squares line have the same formulas, regardless of the shape of the scatter plot
- We will use a population of collegiate shot put athletes and look at the relationship between strength and shotput distance
- ![weight lifted](/graphs/15_4_shotput_weight.png)
- **No matter what the shape of the scatter plot, there is a unique line that minimizes the mean squared error of estimation**
- **This line is called the regression line, and its slope and intercept are given by**
  - $\text{slope of the regression line} = r * \frac{\text{SD of } y}{\text{SD of } x}$
  - $\text{intercept of the regression line} = \text{average of } y - \text{slope } * \text{average of } x$
- ![best straight line](/graphs/15_4_best_straight_line.png)
#### 15.4.1: Nonlinear Regression
- Certain graphs may be best fit to a non-straight line, such as a quadratic function
- The numeric minimization follows the same principles as before
- We can use the `minimize()` function to get the best quadratic estimate
- Quadratic function form
  - $f(x) = ax^2 + bx + c$