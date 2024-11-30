# Midterm Review

## Tables
```python
import numpy as np
from datascience import *

# Create the table with the provided data
teams = Table().with_columns(
    'name', ['Celtics', 'Lakers', 'Nets', 'Pistons', 'Rockets'],
    'division', ['Atlantic', 'Pacific', 'Atlantic', 'Central', 'Southwest'],
    'conference', ['Eastern', 'Western', 'Eastern', 'Eastern', 'Western'],
    'arena', [18642, 18997, 17732, 20491, 18055]
)

player_names = make_array(
        'Stephen Curry', 'Dwight Howard', 'Zion Williamson',  # Original 3 players from the image
        'LeBron James', 'Kevin Durant', 'James Harden', 'Russell Westbrook', 'Giannis Antetokounmpo',
        'Kawhi Leonard', 'Paul George', 'Chris Paul', 'Damian Lillard', 'Jimmy Butler', 
        'Anthony Davis', 'Kyrie Irving', 'Jayson Tatum', 'Devin Booker', 'Joel Embiid', 'Luka Dončić')
salary_2019 = make_array(37457154, 5337000, 0,  # 2019 salary from the image
        37436858, 37371462, 37201500, 38506482, 25842697,  # Additional players
        32742550, 33005496, 35680728, 29802946, 32786880, 27093019, 31612500, 32492091, 
        27285000, 30274200, 8100000)
salary_2020 = make_array(40231758, 1620564, 9757440,  # 2020 salary from the image
39219565, 40108950, 40824000, 41006013, 27500000,  # Additional players
34837500, 35402910, 38506482, 31605460, 34675280, 28264448, 33160000, 33492675,
29302600, 31273800, 10250000)
team_2019 = make_array('Warriors', 'Wizards', 'No Team',  # 2019 teams from the image
    'Lakers', 'Nets', 'Rockets', 'Thunder', 'Bucks',  # Additional players
    'Clippers', 'Thunder', 'Rockets', 'Blazers', '76ers', 'Pelicans', 'Celtics', 'Suns', 
    '76ers', 'Mavericks', 'Mavericks')
team_2020 = make_array('Warriors', 'Warriors', 'Pelicans',  # 2020 teams from the image
    'Lakers', 'Nets', 'Nets', 'Wizards', 'Bucks',  # Additional players
    'Clippers', 'Clippers', 'Suns', 'Blazers', 'Heat', 'Lakers', 'Nets', 'Celtics',
    'Suns', '76ers', 'Mavericks')

players = Table().with_columns(
    'name', player_names,
    '2019', salary_2019,
    '2020', salary_2020,
    '19team', team_2019,
    '20team', team_2020
)

# Sorting
sorted_teams = teams.sort("arena", descending=False)
assert sorted_teams.column("name").item(0) == "Nets"

# Grouping
# "One row per division in the Eastern conference"
teams.where("conference", "Eastern").group("division")
# "Sum of 2020 salaries by division"
teams.join("name",players,"20team").select("division", "2020").group("division", np.sum)
```
### Sorting
### Select / Drop
