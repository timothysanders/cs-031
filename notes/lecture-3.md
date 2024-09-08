# Lecture 3: Tables

## 3.1 Expressions
- Python programs are made up of expressions,which is how computers combine pieces of data
- These expressions are then evaluated by the computer
- If you pass incorrect syntax to the computer, it will not attempt to evaluate it, but will raise an error
  - Syntax is the set of grammar rules
- Symbols like `*` and `**` are called operators and the values they combine are `operands`
- Expressions follow the same orders of operations as in algebra and parentheses can be uses to group expressions
## 3.2 Names
- Names are given to values in Python using an assignment statement, where a name is followed by `=`, which is followed by an expression
  - That value of the expression will then be substituted for the name in future expressions
- Only the current value of an expression will be assigned to a name
- Names must start with a letter, but can contain letters and numbers and underscores
  - Cannot contain spaces
### 3.2.1 Example: Growth Rates
- Relationship between two measurements of the same quantity taken at different times is often expressed as a *growth rate*
## 3.3 Call Expressions
- *Call expressions* invoke functions, which are named operations
- The name of the function comes first, followed by parentheses
- *Arguments* are passed to functions and then the function can *return* some final value
  - Not every function returns something though
- A collection of functions can be created that is called a *module*, which are then imported to be used
- Learning how functions work is an important part of programming
- To learn how a function works, in a jupyter notebook, you can put a `?` after the name
  - In the CLI, you can use the `help()` function to get more details
- Python has a number of [built-in functions](https://docs.python.org/3/library/functions.html)
## 3.4 Introduction to Tables
- Tables are a fundamental way of storing and representing data sets
- Tables can be viewed in two ways
  - a sequence of named columns that each describe a single attribute of all entries in a data set 
  - a sequence of rows that each contain all information about a single individual in a data set.
- A method is very similar to a function, but it must operate on its particular class (in this instance, a table)
### 3.4.1 Choosing Sets of Columns
- The method `select` creates a new table with only the specified columns
  - Ex: `cones.select('Flavor', 'Price')`
- Can also use the method `drop` to return a table without the specified columns
  - Ex: `cones.drop('Color')`
- Both of these methods leave the original table unchanged
### 3.4.2 Sorting Rows
- The `sort` method creates a new table by arranging rows in ascending order of the specified column
  - Ex: `cones.sort('Price')`
- This method has an optional argument, named `descending`
  - Ex: `cones.sort('Price', descending=True)`
### 3.4.3 Selecting Rows that Satisfy a Condition
- The `where` method creates a new table of only rows that satisfy the given condition
  - Ex: `cones.where('Flavor', 'chocolate')`
  - Note that values passed are case-sensitive, so `chocolate != Chocolate`


## Lecture Video Notes
- Video: https://www.youtube.com/watch?v=oTPPF5axPck&list=PL3juAj0fqNsLLBRFnTAby0VjByHoFb0qQ&t=373s

### Python
- Popular for data science and general software development
- Best to learn a language through practice
- Syntax is the form of a programming language
### Concepts
- Expression evaluates to a value
- Values can be numbers or strings
- The syntax of the language is rigid
- Particular behavior associated with built-in operators that one needs to learn
### Names
- Assignment statements give a value to a name
- Names can be used to refer to values, helps you organize programs that you write
- Expressions can be assigned to a particular name
- Names cannot have spaces, must start with a letter
### Functions
- Function call expressions `f(27)` where f is the function, 27 is the argument
- Argument is the input
- Functions can have more than one input, there are many "built-in" functions
- Some additional functions can be found in "modules" (such as the math module)
- Some functions have multiple arguments and those arguments can be named
### Tables
- Table Structure
  - A table is a sequence of labeled columns
  - Each row represents one individual
  - Data within a column represents on attribute of the individual
- Use the datascience module to work with tables in this class
  - This is a teaching module, not an industry module
  - Very similar to pandas, but gentler learning curve
- No constraints on what can be in a table, each row does not have to be distinct
- Use the following syntax to create a table and show the first three rows
- Using the select() method on the table allows you to just see one column
```python
from datascience import *
cones = Table.read_table("cones.csv")
cones.show(3)
cones.select("Flavor")
```
- You can pass multiple column names into the select method
  - Ex: `select("Flavor", "Color")`
- Use the `drop()` method with a column name to show the table without that column
- Select particular rows by using the `where()` method
  - Ex: `cones.where("Flavor", "strawberry")`
- Can sort using the `sort()` method
  - `cones.sort("Price")`
  - `cones.sort("Price", descending=True)`
- Can chain together the `where()` method to apply multiple filters
  - `nba.where("position", "PG").where("season", 2020)`
- Some table operations
  - `t.select(label)`: constructs a new table with just the specified columns
  - `t.drop(label)`: constructs a new table in which the specified columns are omitted
  - `t.sort(label)`: constructs a new table with rows sorted by the specified column
  - `t.where(label, condition)`: constructs a new table with just the rows that match the condition