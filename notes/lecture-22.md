# Lecture 22: Midterm Review

## Python code
```python
import numpy as np
from datascience import *

pokemon = Table().with_columns(
    "Name", make_array("Bulbasaur", "Ivysaur", "Venusaur", "VenusaurMega Venusaur", "Charmander", "Charmeleon", "Charizard", "CharizardMegaCharizard X", "CharizardMega Charizard Y", "Squirtle", "Articuno"),
    "Type", make_array("Grass", "Grass", "Grass", "Grass", "Fire", "Fire", "Fire", "Fire", "Fire", "Water", "Ice"),
    "Total", make_array(318, 405, 525, 625, 309, 405, 534, 634, 634, 314, 485),
    "HP", make_array(45, 60, 80, 80, 39, 58, 78, 78, 78, 44, 90),
    "Attack", make_array(49, 62, 82, 100, 52, 64, 84, 130, 104, 48, 85),
    "Defense", make_array(49, 63, 83, 123, 43, 58, 78, 111, 78, 65, 100),
    "Sp. Atk", make_array(65, 80, 100, 122, 60, 80, 109, 130, 159, 50, 125),
    "Sp Def", make_array(65, 80, 100, 120, 50, 65, 85, 85, 115, 64, 125),
    "Speed", make_array(45, 60, 80, 80, 65, 80, 100, 100, 100, 43, 85),
    "Generation", make_array(1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1),
    "Legendary", make_array("False","False","False","False","False","False","False","False","False","False", "True")
)

def one_walk():
    step_proportions = make_array(0.5, 0.5)
    steps = sample_proportions(100, step_proportions)
    return abs(steps.item(0)*100 - steps.item(1)*100)/2

def one_walk():
    steps = np.random.choice([-1, 1], size = 100)
    final_position = sum(steps)
    return abs(final_position)


distances = make_array()
for i in np.arange(10000):
    new_distance = one_walk()
    distances = np.append(distances, new_distance)

p_value = sum(distances  >= 30) / len(distances)
```