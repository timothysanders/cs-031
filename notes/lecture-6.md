# Lecture 6: Census

## Readings
- 6.3
- 6.4

## Lecture Video Notes
- Video link: https://www.youtube.com/watch?v=NH09Fjdl56A&list=PL3juAj0fqNsLLBRFnTAby0VjByHoFb0qQ&index=6

### Table Structure
- Table is a sequence of labeled columns
- Labels are always strings
- Columns are arrays, all with the same length
  - Data type is enforced for a single column
  - Cannot have some rows as integers, some as strings
### Table Methods
- Creating and extending tables
  - `Table.read_table()` and `Table().with_columns()`
- Finding the size: `num_rows` and `num_columns`
- Referring to columns: by labels or indices
  - Columns are in specific order, with indices starting at 0
  - Labels are case sensitive
- Accessing data in a column
  - `column` takes a label or index and **returns an array**
- Using array methods to work with data in columns
  - `item`, `sum`, `min`, `max`, and so on
- Creating new tables containing some of the original columns
  - `select`, `drop`
### Manipulating Rows
- `t.sort(column)` sorts the rows in increasing order
- `t.sort(column, descending=True)` sorts the rows in decreasing order
- `t.take(row_numbers)` keeps the numbered rows
- `t.where(column, are.condition)` keeps all rows for which a column's value satisfies a condition
- `t.where(column, are.equal_to(value))` keeps all rows for which a column's value equals some particular value
  - Shorter form `t.where(column, value)`
- Can use `np.count_nonzero()` to count the number of True values in an array
- Can also use `np.sum()`
- Can use `t.where(column, are.contained_in([value1, value2])` to filter on multiple values for a single column
  - http://www.data8.org/datascience/reference-nb/datascience-reference.html#are.contained_in()
### The Decennial Census
- Conducted every ten years, counting how many people are in the US
- In between censuses, the Bureau estimates how many people there are each year
### Census Table Description
- Values have column-dependent interpretations
  - SEX column: 1 is Male, 2 is Female (only two categories)
  - POPESTIMATE2010 column: 7/1/2010 estimate
  - AGE
- In the table, some rows are sums of other rows
  - The SEX column 0 is Total (of Male and Female categories)
  - The AGE column 999 is Total of all ages
### Analyzing Census Data
- `t.relabeled(index, "new_name")` can be used to rename a column of a table
- Binary code in SEX column
  - Pretty much the same question since 1790
- Sometimes historical forms of questions can lead to
  - Non-response
  - Inaccurate response
- This can make the census data hard to interpret and use