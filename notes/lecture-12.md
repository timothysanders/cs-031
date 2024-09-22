# Lecture 12: Table Examples

## Reading
- 8.5: Bike Sharing in the Bay Area

### 8.5: Bike Sharing in the Bay Area
- This section introduces `map_table`
#### 8.5.1: Exploring the Data with `group()` and `pivot()`
```python
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
plt.style.use('fivethirtyeight')
from datascience import *

trips = Table.read_table('data/trip.csv')
#trips
commute = trips.where("Duration", are.below(1800))
commute.hist("Duration", unit="Second")
# add more bins for more detail
commute.hist("Duration", bins=60, unit="Second")

# Use `group()` to identify most used start station
starts = commute.group("Start Station").sort("count", descending=True)
# Use `group()` to identify the most used start/end station combinations
commute.group(["Start Station", "End Station"])

# Use pivot() to get contingency table of start end stations
commute.pivot("Start Station", "End Station")
# Use pivot() to find shortest time of rides between Start/End stations
# "Duration" is the optional `values` argument and min is the function to perform on values in each cell
commute.pivot("Start Station", "End Station", "Duration", min)
```
- First argument of the `pivot()` method specifies the column labels of the pivot table
- Second argument labels the rows of the pivot table

#### 8.5.2: Drawing Maps
- Can use `Marker.map_table` to create a map from a table
- Map is created using OpenStreetMap
#### 8.5.3: More Informative Maps: An Application of `join`
```python
from datascience import *

stations = Table.read_table("station.csv") # File doesn't exist..
Marker.map_table(stations.select("lat", "long", "name").relabel("name", "labels"))

# can change points used to represent locations
sf = stations.where("landmark", are.equal_to("San Francisco"))
sf_map_data = sf.select("lat", "long", "name").relabel("name", "labels")
Circle.map_table(sf_map_data, color="green")

cities = stations.group("landmark").relabeled("landmark", "city")
colors = cities.with_column("color", make_array("blue", "red", "green", "orange", "purple"))
joined = stations.join("landmark", colors, "city")
colored = joined.select("lat", "long", "name", "color").relabel("name", "label")
Marker.map_table(colored)

# See where most bike rentals originate
starts = commute.group("Start Station").sort("count", descending=True)
station_starts = stations.join("name", starts, "Start Station")
starts_map_data = station_starts.select("lat", "long", "name").with_columns(
    "colors", "blue",
    "areas", station_starts.column("count") * 0.3
)
starts_map_data.show(3)
Circle.map_table(starts_map_data.relabel("name", "labels"))
```
