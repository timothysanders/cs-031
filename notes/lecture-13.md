# Lecture 13: Conditionals & Iteration

## Reading
- 9: Randomness
- 9.1: Conditional Statements
- 9.2: Iteration

### 9: Randomness
- Data scientists must be able to understand randomness
- Randomness can be used to assign individuals to treatment/control groups
- Python can be used to help make choices at random, using `numpy.random` sub-module
  - Whenever you run the code, the value returned can be different
- Fundamental question about random events is whether they occur
  - Ex: Did an individual get assigned to the treatment group or not?
  - Ex: Has a poll made an accurate prediction, or not?
  - Once event has occurred, you can answer these questions
- Booleans and Comparison
  - Boolean values, named for George Boole, can only have two values, `True` or `False`

| Comparison            | Operator | True example | False Example |
|-----------------------|----------|--------------|---------------|
| Less than             | <        | 2 < 3        | 2 < 2         |
| Greater than          | >        | 3 > 2        | 3 > 3         |
| Less than or equal    | <=       | 2 <= 2       | 3 <= 2        |
| Greater than or equal | >=       | 3 >= 3       | 2 >=3         |
| Equal                 | ==       | 3 == 3       | 3 == 2        |
| Not equal             | !=       | 3 != 2       | 2 != 2        |

  - Expressions can contain multiple comparisons
- Comparing Strings
  - Strings can also be compared, the order is alphabetical
  - Shorter string is less than a longer string that begins with the shorter string
- Comparing an Array and a Value
  - If you compare an array and a single value, you will get an array of booleans with the results of the comparisons
  - `np.count_nonzero()` can be used to evaluate number of "non-zero" elements in an array
    - Remember that `True` is considered to be non-zero
```python
import numpy as np
from datascience import *

two_groups = make_array("treatment", "control")
np.random.choice(two_groups)
# Optional second argument for how many times to repeat the random choice
np.random.choice(two_groups, 10)

# compare array and single value
tosses = make_array("Tails", "Heads", "Tails", "Heads", "Heads")
res = tosses == "Heads"
# use np.count_nonzero()
np.count_nonzero(res)
```

### 9.1: Conditional Statements
- In many situations, actions/results depend on a specific set of conditions being satisfied
- *Conditional statement* is a multiline statement that allows Python to choose alternatives based on whether statement is true
- Conditional statements often appear in functions
- Conditional statements always begin with `if`
```python
def sign(x):
    if x > 0:
        return "Positive"
    elif x < 0:
        return "Negative"
    else:
        return "Neither positive nor negative"

sign(3)
```
#### 9.1.1: The General Form
- Conditional statement can have multiple clauses, but only of these bodies is ever executed
- General format is shown below
```python
if <if expression>:
    <if body>
elif <elif expression 0>:
    <elif body 0>
elif <elif expression 1>:
    <elif body 1>
...
else:
    <else body>
```
- Always only one `if` clause, but can be any number of `elif` sections
- `else` is optional and is only executed if none of the other expressions evaluate to true
  - Else clause must come at the end

### 9.2: Iteration
- Often, we want to repeat a process multiple times
- To help with this, we can use a `for` loop over the contents of a sequence
  - This is called `iteration`
#### 9.2.1: Augmenting Arrays
- Typical use of a for loop is to create an array of results
- Can use the `append()` method in Numpy to do this
```python
import numpy as np
from datascience import *

pets = make_array("Cat", "Dog")
np.append(pets, "Another Pet")
```
- Note that this call on its own does not actually change the original array!
- We can reassign the new array to our old name
```python
import numpy as np
from datascience import *

pets = make_array("Cat", "Dog")
pets = np.append(pets, "Another Pet")
```
- Capturing results of array allows us to perform other calculations on the results of our loop