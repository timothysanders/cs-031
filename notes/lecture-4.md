# Lecture 4: Data Types
- Built in `type` function can return the type of any expression
## 4.1 Numbers
- Two major different types of numbers, `int` and `float`
- Integers represent whole numbers with no fractional component
- Floating point values (float) represent real numbers which can be whole or fractional
  - Are always displayed with a decimal point
- The type of expression is the type of its final value
### 4.1.1 More about Float Values
- Floats are very flexible but have some limitations, namely that they can only represent 15-16 significant digits for any number
- Sometimes float values obtained through arithmetic may be incorrect in the last few digits
- The extra digits beyond 15 significant digits are discarded before any arithmetic is carried out
- These limitations should not majorly impact most applications
## 4.2 Strings
- String data types are used to represent text
- Certain operators can be used with strings and will have different results than numbers
  - For example, two strings can be "added", which will result in a concatenated string
- Single and double quotes can both be used to create strings and are identical in terms of what they return
- The function `str` function returns a string representation of any value
### 4.2.1 String Methods
- Strings methods are functions that operate on strings
- For example, `upper()` method returns an uppercase version of the string
- The method `replace()` replaces all instances of a substring with another
  - Note that this method does not actually modify the original string, but returns a new string
## 4.3 Comparisons
- Boolean values can often arise from comparison operators, the value True indicates that the comparison is valid

| Comparison            | Operator | True Example | False Example |
|-----------------------|----------|--------------|---------------|
| Less than             | <        | 2 < 3        | 2 < 2         |
| Greater than          | >        | 3 > 2        | 3 > 3         |
| Less than or equal    | <=       | 2 <= 2       | 3 <= 2        | 
| Greater than or equal | >=       | 3 >= 3       | 2 >= 3        |
| Equal                 | ==       | 3 == 3       | 3 == 2        |
| Not equal             | !=       | 3 != 1       | 2 != 2        |
- Expressions can contain multiple comparisons
- Strings can be compared and their order is alphabetical. Shorter strings are "less than" longer strings that begin with the shorter string

## Sequences
- Values can be grouped together into collections, which can also be called sequences
- These allow us to write code that performs computation on many pieces of data at once
- Use the function `make_array` to put multiple values into an `array`
- Collections allow us to pass multiple values into a function with a single name
## 5.1 Arrays
- Arrays can be used to create collections of numbers, strings, and other types of values, but can only contain a single kind of data
- Arrays can be used in arithmetic expressions to compute over their contents
- When array is combined with a single number, the number is combined with each element in the array
- Arrays also have `methods`, which are functions that operate on arrays
### 5.1.1 Functions on Arrays
- [Numpy reference](http://docs.scipy.org/doc/numpy/reference/)
- Some important functions are listed here, each of these takes an array as an argument and returns a single value
  - `np.prod`: multiply all elements together
  - `np.sum`: add all elements together
  - `np.all`: test whether all elements are true values (non-zero numbers are true)
  - `np.any`: test whether any elements are true values (non-zero numbers are true)
  - `np.count_nonzero`: count the number of non-zero elements
- Each of these functions takes an array as an argument and returns an array of values
  - `np.diff`: difference between adjacent elements
  - `np.round`: round each number to the nearest integer (whole number)
  - `np.cumprod`: a cumulative product: for each element, multiply all elements so far
  - `np.cumsum`: a cumulative sum: for each element, add all elements so far
  - `np.exp`: exponentiate each element
  - `np.log`: take the natural logarithm of each element
  - `np.sqrt`: take the square root of each element
  - `np.sort`: sort the elements
- Each of these functions takes an array of strings and returns an array
  - `np.char.lower`: lowercase each element
  - `np.char.upper`: uppercase each element
  - `np.char.strip`: remove spaces at the beginning or end of each element
  - `np.char.isalpha`: whether each element is only letters (no numbers or symbols)
  - `np.char.isnumeric`: whether each element is only numeric (no letters)
- Each of these functions takes both an array of strings and a search string; each returns an array
  - `np.char.count`: count the number of times a search string appears among the elements of an array
  - `np.char.find`: the position within each element that a search string is found first
  - `np.char.rfind`: the position within each element that a search string is found last
  - `np.char.startswith`: whether each element starts with the search string
## 5.2 Ranges
- a range is an array of numbers in increasing or decreasing order, separated by a regular interval
- Ranges can be defined using the `np.arange` function
  - Can take one, two, or three arguments: a start, end, and a step
  - One argument = `end` value, `start=0` and `step=1`
  - Two arguments = `start` and `end` with `step=1`
  - Three arguments = `start`, `end`, and `step`
- Range always includes its start value, but does not include its end value
  - Steps can be positive or negative values
### 5.2.1 Example: Leibniz's formula for π
- `π = 4 * (1 - 1/3 + 1/5 - 1/7 + 1/9 - 1/11 + ...)`
## 5.3 More on Arrays
- Can get a particular item from an array by using the `item()` method and passing an index
- If two arrays are the same size, it is easy to do calculations on both arrays
```python
import numpy as np
highs = np.array([13.6  , 14.387, 14.585, 15.164])
lows = np.array([2.128, 2.371, 2.874, 3.728])
highs - lows # array([11.472, 12.016, 11.711, 11.436])
```
### 5.3.1 Elementwise arithmetic on pairs of numerical arrays
- If an arithmetic operator acts on two arrays of the same size, then the operation is performed on each corresponding pair of elements in the two arrays
### 5.3.2 Example: Wallis' Formula for π
- `π = 2 * (2/1 * 2/3 * 4/3 * 4/5 * 6/5 * 6/7 * ...)`

## Lecture Video Notes
- Video link: https://www.youtube.com/watch?v=G8y-a64esvw&list=PL3juAj0fqNsLLBRFnTAby0VjByHoFb0qQ&t=448s

### Why Names?
- Names help document code
- Complex expressions can be written without names, but it can be difficult to interpret without names
- Names help document mean of particular objects
- Choose names that will help you understand your code later
- Can also use markdown or other comments to add additional context
- The role of names can also be to communicate to someone else what is going on

### Numbers
- integer
  - Integers can have arbitrary size and have exact values
  - int never has decimal point
- float (floating point value)
  - Important to remember that these decimal numbers have finite precision
  - These typically have 15/16 significant digits
  - Need to be careful, because the accumulation of small errors can end up becoming large errors
  - "Store twice as many digits of precision as you truly need"
  - IEEE 754 standard (how you represent numbers in a computer)
  - always has a decimal point

### Strings
- strings (str)
  - strings hold text, a representation of text
  - a set of characters enclosed in quotations (either double " or single ')
  - Can put pretty much anything you want, though you may have to escape certain characters
  - single quoted and double quoted strings are the same type in the end
  - strings can be concatenated by using the `+` operator
  - can multiply a string by an integer with the `*` operator

### Types
- Some built-in functions can convert between strings and numbers
- `str()`
  - Converts the input to the function to a string
  - any value can be converted to a string
- `int()`
  - Can convert an input to an integer
  - Int will convert a float to an integer
- `float()`
  - Can convert an input to a float
  - Will convert an integer into a float
- `type()`
  - Tells you the type/class of the value that is passed
  - Can also call this on functions

### Arrays
- list of values that all have the same time
- numpy is helpful for computing array operations
- When using numpy arrays, you can perform operations simply
  - `first_four = make_array(1, 2, 3, 4)`
  - `np.average(first_four)`
  - `np.sum(first_four)`
  - `first_four + 3`
- Combining arrays (only when they are the same length)
  - `second_four = make_array(5, 6, 7, 8)`
  - `first_four + second_four`
- Get a particular item in the array
  - `first_four.item(1)`
  - zero based-indexes