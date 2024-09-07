# Lecture 4: Data Types

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