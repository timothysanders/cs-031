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
- Scatter plots and line plots display two quantitative variables (variables on both axes are quantitative)
- Bar chart has categories on one axis and numerical quantities on the other
- Width and spacing of bars on a chart is up to the person producing the graph
- Most importantly, bars can be in any order on the chart
  - `icecream.sort('Number of Cartons', descending=True).barh('Flavor')`
#### 7.1.3: Grouping Categorical Data
- If tables do not already include aggregated data, it must be calculated prior to drawing the graph
- The table method `group()` can be used to count how frequently a category appears in a table
- `studio_distribution = movies_and_studios.group('Studio')`
- `studio_distribution.sort('count', descending=True).barh('Studio')`
#### 7.1.4: Towards Quantitative Variables
- Distributions of categorical variables can be displayed using a bar chart
- But if variable is not categorical, but quantitative, need to consider the numeric relations between values
### 7.2: Visualizing Numerical Distributions
- Many variables data scientists study are quantitative, or numerical, which means numbers on which you can perform arithmetic
  - Be careful with categorical variables that have been assigned numerical values (such as SEX = 0, 1, 2)
#### 7.2.1: Binning the Data
- Sometimes, grouping quantitative variables into intervals, known as bins, can be more interesting
  - This is called "binning"
- The table method `bin()` can be used to group quantitative values into bins
- `bin_counts = millions.bin('Adjusted Gross', bins=np.arange(300,2001,100))`
- Since bins split the number line into intervals, the bins are contiguous
  - This means that each bin, except the last, contains its left endpoint, but not its right endpoint
  - The notation *(a, b]* is used to show this
    - The bin contains all values greater than or equal to *a*, and less than *b*
  - The last bin includes the data from *both* endpoints, the last row of the table is the right endpoint of the last bin and always has a count of zero
  - By default, `bin()` splits into 10 equally wide bins
#### 7.2.2: Histogram
- Histogram is a visualization of the distribution of a quantitative variable
- Looks very similar to a bar chart, but has critical differences
- Use the `hist()` method to draw a histogram of the values in a column
  - `unit` argument is used in labels on the two axes
  - With no specified bins, will create 10 equally wide bins between min and max
- Histograms have two numerical axes
- **THE VERTICAL AXIS DOES NOT REPRESENT PERCENTS**
#### 7.2.3: The Horizontal Axis
- The `hist()` method uses the same endpoint convention as the `bin()` method
  - Includes data at left endpoint, but not at right endpoint (except for rightmost bin)
- Optional argument `bins` can be used with `hist()` to specify endpoints of bins
  - `millions.hist('Adjusted Gross', bins=np.arange(300,2001,100), unit="Million Dollars")`
- If there are a smaller number of higher values off to the right, this is called *skewed to the right* or having *long right hand tail*
#### 7.2.4: The Area Principle
- Both axes of histogram are numerical, which means you can do arithmetic on them
- "The area principle of visualization says that when we represent a magnitude by a figure that has two dimensions, such as a rectangle, then the area of the figure should represent the magnitude."
  - This means, don't multiply both the width and height by your factor
#### 7.2.5: The Histogram: General Principles and Calculation
- Histograms follow the area principle
- Two defining properties
  1. Bins are drawn to scale and are contiguous (though some may be empty), because the values on the horizontal axis are numerical and therefore have fixed positions on the number line
  2. The **area** of each bar is proportional to the number of entries in the bin
- Drawing a histogram usually means "area of bar = percent of entries in bin"
- Areas represent percents, which means height is not the percent
  - Height of bar = percent of entries in bin / width of bin
- The unit of height is "percent per unit on the horizontal axis"
  - Height is the percent in the bin relative to the width of the bin
  - This is called *density* or *crowdedness*
- When drawn like this, histogram is drawn on the *density scale*
  - Area of each bar is equal to percent of data values in corresponding bin
  - Total area of all bars in the histogram is 100%
#### 7.2.6: Vertical Axis: Density Scale
- Height of bar is percent of elements that fall into corresponding bin, relative to width of bin
- **Units**: Height of bar is **not** percent of entries in the bin
  - It is percent of entries in bin relative to the amount of space in the bin
- Vertical axis is said to on the density scale
#### 7.2.7: Why Not Simply Plot the Counts?
- Main reason for plotting density instead of counts or percents is to be able to compare histograms/approximate them with smooth curves
- Drawing histograms with density scale allows for comparisons of data sets of different sizes/different choices of bins
- If histogram has unequal bins, plotting on density scale is a requirement for interpretability
- Bins do not have to be equal
- Counts can misrepresent the shape of the distribution of data
#### 7.2.8: Flat Tops and the Level of Detail
- When grouping values into bins, you lose some level of detail, for example, how values are distributed in the bins
- When visualizing using a histogram, you are making some choices to ignore finer details to represent a rough approximation
#### 7.2.9: Computing All Heights
#### 7.2.10: Differences Between Bar Charts and Histograms
- Bar chart
  - Bar charts display one numerical quantity per category
    - Often used to display distributions of categorical variables
  - All bars in bar charts have the same width, equal amount of space between consecutive bars
    - Bars can be in any order
  - Heights of bars are proportional to the count in each category
- Histogram
  - Histograms display distributions of quantitative variables
  - Bars of histogram are contiguous, bins are drawn to scale on number line
  - Heights of bars measure densities, *area* of bar is proportional to the counts in the bins
### 7.3: Overlaid Graphs
- Common use of graphs is to compare datasets
- Overlaying plots is drawing them on a single graphic with a common pair of axes
- Graphs being overlaid must represent the same variables and be measured in same units
- Use the same table methods (`scatter()`, `plot()`, and `barh()`)
#### 7.3.1: Overlaid Scatter Plots
- To draw an overlaid scatter plot from a table with three variables, specify the common horizontal axis
  - `sons_heights.scatter('son')`
#### 7.3.2: Overlaid Line Plots
- Similar to the scatter plot, you can draw the plot by specifying the common horizontal axis
  - `children.plot('AGE')`
#### 7.3.3: Bar Charts
- As with the other graph types, specify the common axis
  - `usa_ca.barh('Ethnicity/Race')`
- Be careful about including too many different bars in charts as it can get cluttered
  - `usa_ca.select('Ethnicity/Race', 'USA All', 'CA All').barh('Ethnicity/Race')`

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
