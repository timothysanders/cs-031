# Lecture 3: Tables

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