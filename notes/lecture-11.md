# Lecture 11: Pivots & Joins

## Reading
- 8.4: Joining Tables by Columns

### 8.4: Joining Tables by Columns
- Often data about members of a population is maintained in multiple tables
- If we have a common column, we can put the tables together, baed on the matching column
- Use the method `join()` to create a new table
- ex: `table1.join(table1_column_for_joining, table2, table2_column_for_joining)`
- The resulting table will only contain information about items that appear in both tables
```python
from datascience import *

# Create first table
cones = Table().with_columns(
    'Flavor', make_array('strawberry', 'vanilla', 'chocolate', 'strawberry', 'chocolate'),
    'Price', make_array(3.55, 4.75, 6.55, 5.25, 5.75)
)
cones

# Create second table
ratings = Table().with_columns(
    'Kind', make_array('strawberry', 'chocolate', 'vanilla'),
    'Stars', make_array(2.5, 3.5, 4)
)
ratings

# Joined tables
rated = cones.join("Flavor", ratings, "Kind")

# Calculate the "price per star"
rated.with_column('$/Star', rated.column('Price') / rated.column('Stars')).sort(3)
```