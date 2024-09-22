# Lecture 14: Chance

## Reading
- 9.2: Iteration
- 9.3: Simulation
- 9.4: The Monty Hall Problem

### 9.3: Simulation
- Simulation is the process of using a computer to mimic a physical experiment
- Simple examples include simulating dice rolls or flipping a coin
#### 9.3.1: The Process
- **Step 1: What to Simulate**
  - Decide what quantity to simulate
  - Ex, if flipping a coin, simulated values will be heads or tails
- **Step 2: Simulating One Value**
  - Figure out how to simulate one value that was specified in step one
  - Ex, if flipping a coin, how can you simulate one toss?
  - Usually want to define a function that does this
- **Step 3: Number of Repetitions**
  - Decide how many times to simulate the quantity
- **Step 4: Simulating Multiple Values**
  - Create an empty array to collect all simulated values
  - Create a repetitions sequence, such as `np.arange(n)`
  - Create a `for` loop
### 9.4: The Monty Hall Problem
- In problems involving randomness, assumptions about randomness are important
- Given three doors, reasonable to assume 1/3 chance of door having car behind it
- **Step 1: What to Simulate**
  - For each play, simulate all three doors
    - One the contestant picks
    - One the Monty opens
    - The remaining door
- **Step 2: Simulating One Play**
  - Set up `goats` array
  - Must identify which goat is selected and which is revealed behind open door
- **Step 3: Number of Repetitions**
  - Will play the game 10,000 times
- **Step 4: Simulating Multiple Repetitions**
  - Will store values from each play as rows in a table, using the `Table.append()` method
    - This method adds a new row to the bottom of the table
- The simulation confirms that the contestants are twice as likely to win if they switch doors
```python
import numpy as np
from datascience import *

def other_goat(x):
    if x == "first goat":
        return "second goat"
    elif x == "second goat":
        return "first goat"

def monty_hall_game():
    contestant_guess = np.random.choice(hidden_behind_doors)
    if contestant_guess == "first goat":
        return [contestant_guess, "second goat", "car"]
    if contestant_guess == "second goat":
        return [contestant_guess, "first goat", "car"]
    if contestant_guess == "car":
        revealed = np.random.choice(goats)
        return [contestant_guess, revealed, other_goat(revealed)]

goats = make_array("first goat", "second goat")
hidden_behind_doors = np.append(goats, "car")

# empty collection table
games = Table(["Guess", "Revealed", "Remaining"])

for i in np.arange(10000):
    games.append(monty_hall_game())
```