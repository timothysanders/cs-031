# Lecture 9: Functions

## Readings
- 8: Functions and Tables
- 8.1: Applying a Function to a Column

### 8: Functions and Tables
- Defining functions allows you to give a name to a computational process that can be applied multiple times
- This can help if you want to apply the same value on every column in a table
- Defining a Function
  - Function definitions start with `def`
  - Function components
    - Signature
    - Documentation/docstring
      - Well-composed functions have names that evoke their behavior and have documentation/docstrings
      - Docstring is the first thing inside a function's body, typically defined with triple quotation marks
    - Body
    - Indentation
  - When defining a function, the code itself is not actually run
    - Think of a recipe
  - Names defined in functions are not available outside the functions execution
    - This is called "local scope"
- Multiple Arguments
  - Functions can take more than one value/argument
  - Arguments can be made "optional" by specifying a default value
- Methods are called with the "dot notation"
### 8.1: Applying a Function to a Column
- Sometimes you may want to convert the values in a column using a function that doesn't take an array as an argument
- For example, maybe a function only takes a single number as an argument
#### 8.1.1: Apply
- The table method `apply()` calls a function on each element of a column and returns a new array
- Pass the name of the function to apply (without parentheses) and the column name
  - `ages.apply(cut_off_at_100, 'Age')` <- "cut_off_at_100" is the function name here
#### 8.1.2: Functions as Values
- If you specify the function name without the parentheses, Python will return a reference to the function object
- You can specify new names for functions by assigning the function name to another (don't do this)
#### 8.1.3: Example: Prediction
- graph of averages
- regression line
- nearest neighbor prediction method