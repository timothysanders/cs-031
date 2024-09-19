# Lecture 7: Charts

## Readings
- 7: Visualization
- 7.1: Categorical Distributions
- 7.2: Numerical Distributions
- 7.3: Overlaid Graphs

### 7: Visualization
- Visualizations can be helpful for interpreting large tables of numbers and can make them easier to interpret
- Scatter Plots and Line Graphs
  - `variable` is a formal name for what might be called a "feature" or "attribute" (think number of movies)
    - Variable emphasizes that a feature can have different values for different individuals
  - Variables with numerical values and that can be measured numerically are called `quantitative` variables or `numerical` variables
#### Scatter Plots
- Scatter plot displays the relation between two numerical variables
- Table method `scatter()` draws this plot
  - consists of one point for each row of the table
  - First argument is label of column to be plotted on horizontal axis
  - Second argument is label to be plotted on vertical axis
- This plot shows an `association` between variables
  - `positive association` means that high values of one variable tend to be associated with high values of the other variable
  - association is simply a statement about a broad general trend
  - The opposite, `negative association` means that high values in one variable tend to be associated with low values in the other
#### Line Plots
- Sometimes also called line graphs
- Among most common visualizations
- Often used to study chronological trends and patterns
- Table method `plot()` creates a line plot
  - First argument is column on the horizontal axis
  - Second argument is the column on the vertical axis
### 7.1: Visualizing Categorical Distributions
- Data can be in any number of forms that are not numerical
- Examples: flavors of ice cream, basketball teams, movie studios, etc.
- In a `distribution`, each individual belongs to exactly one category
  - A table showing the number of values of each categorical variable is called a `distribution table`
#### 7.1.1: Bar Chart
- Bar chart is a familiar way to visualize categorical distributions
- Bars are equally spaced and equally wide, with length of each bar being proportional to the frequency of the corresponding category
- Table method `barh()`
  - First argument is the column label of the categories
  - Second argument is the column label of the frequencies
  - If your table is just two columns, can just specify the first argument
#### 7.1.2: Design aspects of bar charts
- Compared to 
## Lecture Video Notes
- Video link: https://www.youtube.com/watch?v=ZUZ2yFs3WQI&list=PL3juAj0fqNsLLBRFnTAby0VjByHoFb0qQ&index=7

### Attribute Types
- In general in data science, you have a data set
  - The data set has multiple attributes (columns)
  - "variable" and "attribute" are both references to columns in a table
- All values in a column of a table should be both the same type and be comparable to each other in some way
- `Numerical`: each value is from a numerical scale
  - Numerical measurements are ordered
  - Differences are meaningful
- `Categorical`: each value is from a fixed inventory
  - May or may not have an ordering
  - Categories are the same or different
  - Text is almost always categorical
### "Numerical" Attributes
- Just because the values are numbers, doesn't mean the attribute is numerical
  - For example, from the Census data, the SEX codes are 0, 1, and 2
  - It doesn't make sense to perform arithmetic on these "numbers"
  - The attribute is still categorical, even those we use numbers to represent categories
### Numerical Data
### Plotting Two Numerical Variables
- Line plot: `plot`
  - Typically used for a sequence, or something over time, a trend
  - Line plot should always have some sort of "movement" from left to right
  - `table.plot(arg1, arg2)`
  - To add a title to the plot, use
    - `plots.title("title of plot");`
    - Note that you need a semicolon at the end
  - If you specify only one column, you can just specify one column
  - Can change y axis limits
    - `plots.ylim(0, 100)` 
- Scatter plot: `scatter`
  - Useful for when there is not necessarily a sequential relationship
### Line vs Scatter Plot
- `t.plot(x_label, y_label)`
- `t.scatter(x_label, y_label)`
- Use line plots for sequential quantitative data if
  - your x-axis has an order
  - sequential differences in y values are meaningful
  - there's only one y-value for each x-value
  - Often: x-axis is **time** or **distance**
- Use scatter plots for non-sequential quantitative data
  - If you are looking for associations
### Categorical and Numerical Variables
- Can plot a categorical variable with a bar chart
  - `t.barh(category_label, numeric_label)`
  - `barh` == bar horizontal
  - Lets us do a categorical variable against a numerical variable
  - `t.barh` does not actually sort any data, it is dependent on the input data
### Visualization Fundamentals
- Don't clutter graph
- Use bar charts with categorical variables
- Less can be more
  - Minimize decoration
  - Choose colors carefully
    - Minimize the number of different colors
- If data are numerical, preserve their relative values and distances between them
- 
