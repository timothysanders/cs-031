# Lecture 10: Groups

## Reading
- 8.2: Classifying by One Variable
- 8.3: Cross-Classifying

### 8.2: Classifying by One Variable
- Data scientists often need to classify individuals into groups according to shared features, then identify characteristics of the groups
- This is about classifying individuals into categories that are not numerical
- Uses the `group()` method
#### 8.2.1: Counting the Number in Each Category
- `group()` method with a single argument counts the number of rows for each category in a column
  - Result contains one row per unique value in the grouped column
  - Resulting column of counts is called "count" by default
#### 8.2.2: Finding a Characteristic of Each Category
- Optional second argument of `group()` names the function used to aggregate values from other columns for the rows
  - `cones.group('Flavor', sum)`
  - This calculates the sum of the other column (Price, in this instance) that corresponds to each Flavor
  - Column name takes the label of the column the function operates on and adds the function being used
- The function name `sum` here can be replaced by any other function that works on arrays
#### Example: NBA Salaries
```python
from datascience import Table
import numpy as np
nba1 = Table.read_table('nba_salaries.csv')
nba = nba1.relabeled("'15-'16 SALARY", 'SALARY')
teams_and_money = nba.select('TEAM', 'SALARY')
teams_and_money.group('TEAM', sum)
nba.group('POSITION')
positions_and_money = nba.select('POSITION', 'SALARY')
positions_and_money.group('POSITION', np.mean)
```
### 8.3: Cross-Classifying by More than One Variable
- If there are multiple features, there are a number of different ways to classify them
- Ex, population of college students with a major and number of years in college
  - Can be classified by major, by year, or combination of major and year
- The `group()` method allows us to classify individuals according to multiple variables
  - This is known as *cross-classifying*
#### 8.3.1: Two Variables: Counting the Number in Each Paired Category
- Using the table that is shown here
```python
from datascience import *
more_cones = Table().with_columns(
    'Flavor', make_array('strawberry', 'chocolate', 'chocolate', 'strawberry', 'chocolate', 'bubblegum'),
    'Color', make_array('pink', 'light brown', 'dark brown', 'pink', 'dark brown', 'pink'),
    'Price', make_array(3.55, 4.75, 5.25, 5.25, 5.25, 4.75)
)
# display table
more_cones
# group by a single column, flavor
more_cones.group("Flavor")
# group by multiple columns, flavor and color, make sure to pass a list
more_cones.group(["Flavor", "Color"])
```
- To pass multiple columns, add a list of column labels
#### 8.3.2: Two Variables: Finding a Characteristic of Each Paired Category
- Using a second argument in the `group()` method aggregates all other columns that are not listed in the group columns
```python
more_cones.group(["Flavor", "Color"], sum)
```
- Can add as many variables as you want to the grouped columns, but this number of combinations can become quite large..
#### 8.3.3: Pivot Tables: Rearranging the Output of `group()`
- To display the results of the grouping differently, you can use a *pivot table*, also known as a *contingency table*
- To do this, use the `pivot()` method
```python
more_cones.pivot("Flavor", "Color")
```
- This table will show all combinations of flavor and color, even ones that don't exist in the source data
- The `pivot()` method always takes two column labels, the first to determine the columns, the second to determine the rows
  - First argument is label of column that will form new columns in output
  - Second argument is label of column that will be used for the rows
- Optional third argument called `values` indicates a column of values to replace the counts in each cell
- The fourth argument, `collect`, indicates how to aggregate the values listed in `values`
```python
more_cones.pivot("Flavor", "Color", values="Price", collect=sum)
# functionally equivalent to
more_cones.group(["Flavor", "Color"], sum)
```
- In one situation versus another, a pivot table may be easier to read
#### 8.3.4: Example: Education and Income of Californian Adults
```python
from datascience import *
import numpy as np

full_table = Table.read_table('data/ca-educational-attainment-personal-income-2008-2014.csv')
ca_2014 = full_table.where("Year", are.equal_to("01/01/2014 12:00:00 AM")).where("Age", are.not_equal_to("00 to 17"))
ca_2014
# Prepare for showing small subset of variables
educ_inc = ca_2014.select("Educational Attainment", "Personal Income", "Population Count")
educ_inc
# Group by Educational Attainment and sum Population Count
education = educ_inc.select("Educational Attainment", "Population Count")
educational_totals = education.group("Educational Attainment", sum)
educational_totals
# Define the `percents` function to convert raw numbers to percentages
def percents(array_x):
    return np.round((array_x / sum(array_x)) * 100, 2)

education_distribution = educational_totals.with_column(
    "Population Percent", percents(educational_totals.column(1))
)
education_distribution

# Use `pivot()` to get contingency table of adult Californias cross-classified by "Educational Attainment" and "Personal Income"
totals = educ_inc.pivot("Educational Attainment", "Personal Income", values="Population Count", collect=sum)
totals

# Convert to distributions
distributions = totals.select("Personal Income").with_columns(
    "Bachelor's degree or higher", percents(totals.column(1)),
    'College, less than 4-yr degree', percents(totals.column(2)),
    'High school or equivalent', percents(totals.column(3)),
    'No high school diploma', percents(totals.column(4))   
    )

distributions

# Create bar chart for different income distributions
distributions.select("Personal Income", "Bachelor's degree or higher", "No high school diploma").barh(0)
```