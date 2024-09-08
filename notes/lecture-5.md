# Lecture 5: Building Tables

## 6: Tables
- Tables are a fundamental object type for representing data sets
- Tables can be viewed in two ways
  - a sequence of named columns that each describe a single aspect of all entries in a data set
  - a sequence of rows that each contain all information about a single entry in a data set
- Using the `datascience` module, the `Table()` function can be used to create an empty table
- The `with_columns()` method can be used to add additional labeled columns, each column is an array
- Multiple columns can be specified, but the arrays of values must be the same length, or an error will occur
- Tables can be assigned to a variable and then can have additional columns added to them
```python
from datascience import *
flowers = Table().with_columns(
    'Number of petals', make_array(8, 34, 5),
    'Name', make_array('lotus', 'sunflower', 'rose')
)
flowers.with_columns(
    'Color', make_array('pink', 'yellow', 'red')
)
```
- Note that `with_columns()` returns a new copy of a table each time
- Tables can be easily created from CSVs with the `read_table()` method
```python
from datascience import *
minard = Table.read_table('file/path/' + 'minard.csv')
```
#### Size of a table
- Size of the table can be obtained from the `num_columns` and `num_rows` methods
#### Column Labels
- Column labels can be listed with the `labels` method
  - These column labels can be changed using the `relabeled` method
  - `minard.relabeled('City', 'City Name')`
- Note that these return a new table and do not modify the existing table
- Common pattern can be to reassign the original name to the new table
#### Accessing the Data in a Column
- Use a column's label to get an array of the data in the column
  - Ex: `minard.column('Survivors')`
  - You can also pass a column's index, instead of the name
#### Working with the Data in a Column
- Because columns are arrays, you can use all the array methods discussed previously
- Data can be formatted using the `set_format()` method, which takes `Formatter` objects
  - Can have formatter objects for dates, currencies, numbers, and percentages
#### Choosing Sets of Columns
- The method `select` creates a new table with only the specified columns
  - Can also specify column indices instead of names
  - This is distinct from the `column()` method which returns an array
- Can use the `drop()` method to specify a new table without the listed columns
## 6.1: Sorting Rows
- The `show()` method allows us to specify a number of rows to display, default is all rows of the table
- The `sort()` method takes a column and sorts the table based on that column's data in ascending order
  - To sort in descending order, set the `descending` parameter to True
  - Ex: `nba.sort('SALARY', descending=True)`
### 6.1.1 Named Arguments
- the `descending=True` part of the call expression is a *named argument*
- When a function or method is called, each argument has a position and a name
- It is a good convention to use the argument name so that it is more obvious what the value means
## 6.2: Selecting Rows
- Sometimes we want rows from a table that only have a particular feature
### 6.2.1: Specified Rows
- Use the Table method `take()` to get a specified set of rows
  - The method creates a new table based on a row index or an array of indices
  - Ex: `nba.take(0)` or `nba.take(np.arange(3, 6))`
  - Can get the top 5 plays with the highest salary, `nba.sort('SALARY', descending=True).take(np.arange(5))`
### 6.2.2 Rows Corresponding to a Specified Feature
- More often, we don't know the indices, but know a particular feature we want, for example, only salaries over 10 million
- The method `where()` can be used for this, it outputs a new table with the same columns as the original, but only the rows that meet the given criteria
  - First argument is the column, second argument is the way we specify the feature
  - Ex: `nba.where('SALARY', are.above(10))`
  - Ex: `nba.where('PLAYER', are.equal_to('Stephen Curry'))`
  - If you do not specify a way to compare the feature, the default is `are.equal_to`
### 6.2.3 Multiple Features
- You can chain the `where()` method together multiple times to apply multiple conditions
### 6.2.4 General Form
- Note that when using the `are.between` condition, the table with include the start value, but not the end value
- If a condition is specified without any rows, you will get an empty table
### 6.2.5 Some More Conditions
| Predicate                       | Description                                                 |
|---------------------------------|-------------------------------------------------------------|
| `are.equal_to(Z)`               | Equal to `Z`                                                |
| `are.above(x)`                  | Greater than `x`                                            |
| `are.above_or_equal_to(x)`      | Greater than or equal to `x`                                |
| `are.below(x)`                  | Less than `x`                                               |
| `are.below_or_equal_to(x)`      | Less than or equal to `x`                                   |
| `are.between(x, y)`             | Greater than or equal to `x`, and less than `y`             |
| `are.strictly_between(x, y)`    | Greater than `x` and less than `y`                          |
| `are.between_or_equal_to(x, y)` | Greater than or equal to `x`, and less than or equal to `y` |
| `are.containing(S)`             | Contains the string `S`                                     | 
- Can also specify the negation for any of these conditions by using `.not_` before the condition

## Lecture Video Notes
- Video link: https://www.youtube.com/watch?v=PKyK1fd_81s&list=PL3juAj0fqNsLLBRFnTAby0VjByHoFb0qQ&index=5

### Columns
- Columns of tables are arrays
  - Tables contain one or more columns, each has a label
- Array is a sequence of numbers/strings/other values
- For a table `t`, use `t.column(label)` or `t.column(index)`
  - This will return an array of the column
- For an array `s`
  - `s.item(index)` gives the value at that index (starting at 0)
  - Two ways to aggregate values in an array
    - `np.mean(s)`, `np.sum(s)`, `np.max(s)`, `np.min(s)`
    - `s.mean()`, `s.sum()`, `s.max()`, `s.min()`
- Can relabel a column using the `relabeled()` method
- Common pattern is to chain together a number of different methods
  - Can break up long lines by putting parentheses around the line and then putting new methods on new lines
- The `column()` method returns an array, it is no longer a table
- The `select()` method allows you to return multiple columns, but note that `select()` returns a table
### Ranges
- Range is an array of numbers with some sequential order
- `np.arange(7)`
  - Note that the end number is not included in the range
  - If one argument is passed, you get a range from 0 up to the end (passed number)
  - If two arguments are passed, you have the start and end of the range
  - If you pass three arguments, you have the start, end, and step size
- The `take()` method is used to get specific rows, can pass `np.arange()` to get a specific list of rows
  - When using take, always passing in numbers
- When using `select()` typically passing in labels of columns (but can also pass column indices)
- The `where()` method can be used to specify conditions to make a new table from an existing table
### Ways to create a table
- `Table.read_table(filename)` - reads a table from a file, like a CSV
- `Table()` returns an empty table to which columns are added
- Methods like `select`, `where`, `sort`, `drop`, etc. all return new tables
- Can also use the method `with_row()` to create a new table with the new row that has been specified
