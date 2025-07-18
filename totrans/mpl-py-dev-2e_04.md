

# Advanced Matplotlib

In previous chapters, we have learnt the versatile usage of basic Matplotlib APIs to create and customize various plot types. In order to create more suitable visuals for our data, there are more advanced techniques to make more refined figures. In fact, we can leverage not only the native Matplotlib functionalities but also a number of third-party packages built on top of Matplotlib. They provide easy ways to create more advanced plots that are also aesthetically styled by default. We can then make use of Matplotlib techniques to refine our data plots.

In this chapter, we would further explore the advanced usage of Matplotlib. We would learn how to group multiple relevant plots into subplots in one figure, using non-linear scale axis scales, plotting images, and creating advanced plots with the help of some popular third-party packages. Here are the detailed list of topics we would cover:

*   Drawing subplots
*   Using non-linear axis scales
*   Plotting images
*   Using Pandas-Matplotlib plotting integration
    *   Hexbin plots on bivariate datasets
*   Using Seaborn to construct:
    *   Kernel density estimation plots for bivariate data
    *   Heatmaps with and without hierarchical clustering
    *   `mpl_finance` to plot finance data
*   3D plotting with `Axes3D`
*   Using Basemap and GeoPandas to visualize geographical data

# Drawing Subplots

In designing layouts of visual aids, it is often necessary to organize multiple relevant plots into panels in the same figure, such as when illustrating different aspects of the same dataset. Matplotlib provides a few ways to create figures with multiple subplots.

# Initiating a figure with plt.figure()

The `plt.figure()` API is the API that is used to initiate a figure that serves as the base canvas. It takes in arguments that determines the number of figures and parameters such as size and background color of the plot image. It displays a new area as the canvas for plotting `axes` when called. We wouldn't obtain any graphical output unless we add other plotting elements. If we were to call `plt.show()` at this point, we would see a Matplotlib `figure` object being returned, as shown in the following screen capture:

![](img/20e628d4-18e2-4959-8b51-be612a5c1160.png)

When we are plotting simple figures that involve only a single plot, without the need for multiple panels, we can omit calling `plt.figure()`. If `plt.figure()` is not called or if no argument is supplied to `plt.figure()`, then a single figure is initiated by default, equivalent to `plt.figure(1)`. If the dimension ratio of a figure is crucial, we should adjust it by passing a tuple of `(width, height)` as the `figsize` argument here.

# Initiating subplots as axes with plt.subplot()

To initiate the axes plot instances that actually frame each plot, we can use `plt.subplot()`. It takes three parameters: number of rows, number of columns, and plot number. When the total number of plots is less than 10, we can omit the delimiting commas in the input arguments. Here is a code snippet example:

```py
import matplotlib.pyplot as plt
# Initiates a figure area for plotting
fig = plt.figure()

# Initiates six subplot axes
ax1 = plt.subplot(231)
ax2 = plt.subplot(232)
ax3 = plt.subplot(233)
ax4 = plt.subplot(234)
ax5 = plt.subplot(235)
ax6 = plt.subplot(236)

# Print the type of ax1
print(type(ax1))

# Label each subplot with corresponding identities
ax1.text(0.3,0.5,'231',fontsize=18)
ax2.text(0.3,0.5,'232',fontsize=18)
ax3.text(0.3,0.5,'233',fontsize=18)
ax4.text(0.3,0.5,'234',fontsize=18)
ax5.text(0.3,0.5,'234',fontsize=18)
ax6.text(0.3,0.5,'236',fontsize=18)

plt.show()
```

The preceding code generates the following figure. Note how the subplots are ordered from left to right, top to bottom. When adding actual plot elements, it is essential to place them accordingly:

![](img/5a8d73b4-9e93-4dc9-b41b-33b80268e61a.png)

Also note that printing the type of one of the axes returns `<class 'matplotlib.axes._subplots.AxesSubplot'>` as a result.

# Adding subplots with plt.figure.add_subplot()

There is an `add_subplot()` function similar to `plt.subplot()` under `plt.figure()` that allows us to create additional subplots under the same figure. Similar to `plt.subplot()`, it takes the row number, column number, and plot number as input arguments and allows arguments without commas for fewer than 10 plots.

We can also initiate the first subplot using this function. This code is a quick example:

```py
import matplotlib.pyplot as plt

fig = plt.figure()
ax = fig.add_subplot(111) 

plt.show()
```

This creates an empty plot area enclosed by four spines containing the *x* axis and *y* axis, as shown below. Note that we must call the `add_subplot()` function under a `figure` but not by `plt`:

![](img/737ac4e6-a17f-4f9c-9076-00981dc8809a.png)

Let us further compare the differences between `fig.add_subplot()`. and  `plt.subplot()`. Here, we would be creating three empty subplots with different sizes and face colors.

We will first use try using `fig.add_subplot()`:

```py
import matplotlib.pyplot as plt

fig = plt.figure()
ax1 = fig.add_subplot(111,facecolor='red')
ax2 = fig.add_subplot(121,facecolor='green')
ax3 = fig.add_subplot(233,facecolor='blue')

plt.show()
```

We get three overlapping subplots on the same figure, as shown below:

![](img/eb143e39-1165-42ec-92a5-14be5611bee0.png)

Next, we replace `fig.add_subplot()` with `plt.subplot()`:

```py
import matplotlib.pyplot as plt

fig = plt.figure() # Note this line is optional here
ax1 = plt.subplot(111,facecolor='red')
ax2 = plt.subplot(121,facecolor='green')
ax3 = plt.subplot(233,facecolor='blue')

plt.show()
```

Notice in the following image that the red `ax1` subplot cannot be displayed:

![](img/9d671846-baab-4884-8ffe-0eae4a0f5458.png)

If we have already plotted the first subplot using `plt.subplot()` and would like to create additional subplots, we can call the `plt.gcf()` function to retrieve the `figure` object and store it as a variable. Then, we can call `fig.add_subplot()` as shown in the example before.

Hence, the following code is an alternative way to generate the three overlapping subplots:

```py
import matplotlib.pyplot as plt

ax1 = plt.subplot(111,facecolor='red')
fig = plt.gcf() # get current figure
ax2 = fig.add_subplot(121,facecolor='green')
ax3 = fig.add_subplot(233,facecolor='blue')

plt.show()
```

# Initiating an array of subplots with plt.subplots()

When we need to create a larger number of subplots of the same size, it can be quite inefficient to generate them one by one with the `plt.subplot()` or `fig.add_subplot()` function. In this case, we can call `plt.subplots()` to generate an array of subplots at once.

`plt.subplots()` takes in the number of rows and columns as input parameters, and returns a `Figure` together with a grid of subplots stored in a NumPy array. When there is no input parameter, `plt.subplots()` is equivalent to `plt.figure()` plus `plt.subplot()` by default.

Here is a code snippet for demonstration:

```py
import matplotlib.pyplot as plt

fig, axarr = plt.subplots(1,1)
print(type(fig))
print(type(axarr))

plt.show()
```

From the resultant screenshot , we can observe that `plt.subplots()` also returns the `Figure` and `AxesSubplot` objects:

![](img/f8efed31-c92c-4601-98be-2a1a6465223f.png)

The next example illustrates a more useful case of  `plt.subplots()`.

This time, we will create a figure of 3x4 subplots and label each in a nested `for` loop:

```py
import matplotlib.pyplot as plt

fig, axarr = plt.subplots(3,4)
for i in range(3):
    for j in range(4):
        axarr[i][j].text(0.3,0.5,str(i)+','+str(j),fontsize=18)

plt.show()
```

Again, we can observe from this figure that the subplots are ordered in rows and then columns, as seen in the preceding examples:

![](img/dfa8130b-a90d-405b-9650-462c71a1921b.png)

It is also possible to supply only one input parameter to `plt.subplots()`, which will be interpreted as the specified number of plots vertically stacked in rows. As the  `plt.subplots()` function has essentially incorporated the `plt.figure()` function, we can also specify the figure dimensions by providing input to the `figsize` argument:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(0.0, 1.0, 0.01)
y1 = np.sin(8*np.pi*x)
y2 = np.cos(8*np.pi*x)

# Draw 1x2 subplots
fig, axarr = plt.subplots(2,figsize=(8,6))

axarr[0].plot(x,y1)
axarr[1].plot(x,y2,'red')

plt.show()
```

Note that the type of  `axarr` is `<class 'numpy.ndarray'>`.

The preceding code results in the following figure with two rows of subplots:

![](img/5c8472ca-408c-4f3b-a64d-5cab0b0bc860.png)

# Shared axes

When using `plt.subplots()`, we can specify that the subplots should share the *x* axis and/or *y* axis to avoid cluttering.

Returning to the 3x4 subplots example earlier, suppose we turn on the shared axes option in `plt.subplots()` by supplying `sharex=True` and `sharey=True` as arguments, as in: 

```py
fig, axarr = plt.subplots(3,4,sharex=True,sharey=True)
```

We now obtain the following figure. Compared to the preceding example, it looks neater with the subplot axis labels removed except for the leftmost and bottom ones:

![](img/1bbd290b-2f28-4b58-a008-52a1d354593c.png)

# Setting the margin with plt.tight_layout()

Next, we can adjust the alignment. We may want to adjust the margin between each subplot, or leave no margin instead of having rows and columns of discrete boxes. In this case, we can use the `plt.tight_layout()` function. By default, it fits all subplots into the figure area when no parameters are supplied. It takes the keyword arguments `pad`, `w_pad`, and `h_pad` to control the padding around subplots. Let's look at the following code example:

```py
import matplotlib.pyplot as plt

fig, axarr = plt.subplots(3,4,sharex=True,sharey=True)
for i in range(3):
    for j in range(4):
        axarr[i][j].text(0.3,0.5,str(i)+','+str(j),fontsize=18)

plt.tight_layout(pad=0, w_pad=-1.6, h_pad=-1)
```

We see from the following figure that the now there is no space between subplots, but there's some overlap of axis ticks.

We will learn how to adjust tick properties or to remove ticks in a later section:

![](img/a308847c-9176-4fd1-b595-41a3bb6f6547.png)

# Aligning subplots of different dimensions with plt.subplot2grid()

While `plt.subplots()` provides a handy way to create grids of same-sized subplots, at times we may need to combine subplots of different sizes in a group. This is when `plt.subplot2grid()` comes into use.

`plt.subplot2grid()` takes in three to four parameters. The first tuple specifies the overall dimensions of the grid. The second tuple determines where in the grid the top left corner of a subplot starts. Finally we describe the subplot dimensions using the `rowspan` and `colspan` arguments.

Here is a code example to showcase the usage of this function:

```py
import matplotlib.pyplot as plt

axarr = []
axarr.append(plt.subplot2grid((3,3),(0,0)))
axarr.append(plt.subplot2grid((3,3),(1,0)))
axarr.append(plt.subplot2grid((3,3),(0,2), rowspan=3))
axarr.append(plt.subplot2grid((3,3),(2,0), colspan=2))
axarr.append(plt.subplot2grid((3,3),(0,1), rowspan=2))

axarr[0].text(0.4,0.5,'0,0',fontsize=16)
axarr[1].text(0.4,0.5,'1,0',fontsize=16)
axarr[2].text(0.4,0.5,'0,2\n3 rows',fontsize=16)
axarr[3].text(0.4,0.5,'2,0\n2 cols',fontsize=16)
axarr[4].text(0.4,0.5,'0,1\n2 rows',fontsize=16)

plt.show()
```

The following is the resultant plot. Notice how the subplots of different sizes are aligned:

![](img/9971f73e-b74f-44e1-90d7-29782d2abb8b.png)

# Drawing inset plots with fig.add_axes()

It is not a must for subplots to align side by side. In some occasions, such as when zooming in or out, we can also embed subplots on top of the parent plot layer. This can be done by `fig.add_axes()`. To add a subplot, here is the basic usage: 

```py
fig = plt.figure() # or fig = plt.gcf()
fig.add_axes([left, bottom, width, height])
```

The `left`, `bottom`, `width`, and `height` parameters are specified relative to the parent figure in terms of `float`. Note that `fig.add_axes()` returns an axes object, so you may store it as a variable such as `ax = fig.add_axes([left, bottom, width, height])` for further adjustments.

The following is a complete example where we try to plot the overview in a smaller embedded subplot:

```py
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure()
np.random.seed(100)
# Prepare data
x = np.random.binomial(1000,0.6,1000)
y = np.random.binomial(1000,0.6,1000)
c = np.random.rand(1000)

# Draw the parent plot
ax = plt.scatter(x,y,s=1,c=c)
plt.xlim(580,650)
plt.ylim(580,650)

# Draw the inset subplot
ax_new = fig.add_axes([0.6, 0.6, 0.2, 0.2])
plt.scatter(x,y,s=1,c=c)
plt.show()
```

Let's check out the result in the figure:

![](img/bd328180-6a7d-461c-bb03-66dc7e0a5485.png)

# Adjusting subplot dimensions post hoc with plt.subplots_adjust

We can adjust the dimensions of a subplot with `plt.subplots_adjust()`, which takes in any combinations of parameters—`left`, `right`, `top`, and `bottom`—each defined relative to the parent axes.

# Adjusting axes and ticks

In data visualization, it is often not enough to only display the trend in a relative sense. An axis scale is essential to facilitate value estimation for proper interpretation. Ticks are markers on an axis that denote the scale for this purpose. Depending on the nature of data and figure layout, we often need to adjust the scale and tick spacing to provide enough information without clutter. In this section, we are going to introduce the customization methods.

# Customizing tick spacing with locators

There are two sets of ticks to mark coordinates on each axis: major and minor ticks. By default, Matplotlib tries to  automatically optimize the tick spacing and format based on the data input. Wherever manual adjustment is needed, it can be done through setting these four locators: `xmajorLocator`, `xminorLocator`, `ymajorLocator`, `yminorLocator` via the function `set_major_locator`, or `set_minor_locator` on the respective axis. The following is a usage example, where `ax` is an axes object:

```py
ax.xaxis.set_major_locator(xmajorLocator)    
ax.xaxis.set_minor_locator(xminorLocator)
ax.yaxis.set_major_locator(ymajorLocator)
ax.yaxis.set_minor_locator(yminorLocator)
```

Here, we list the common locators and their usage.

# Removing ticks with NullLocator

When `NullLocator` is used, ticks are removed from view.

# Locating ticks in multiples with MultipleLocator

As the name implies, `MultipleLocator` generates ticks in multiples of a user-specified base. For example, if we would like our ticks to mark integers instead of floats, we can initialize the base by `MultipleLocator(1)`.

# Locators to display date and time

For time series plotting, Matplotlib provides a list of tick locators to serve as datetime markers:

*   `MinuteLocator`
*   `HourLocator`
*   `DayLocator`
*   `WeekdayLocator`
*   `MonthLocator `
*   `YearLocator`
*   `RRuleLocator`, which allows arbitrary date tick specification
*   `AutoDateLocator`
*   `MultipleDateLocator`

To plot time series, we can also use Pandas to specify the datetime format for data in the *x* axis.

Time series data can be resampled by aggregation methods such as  `mean()`, `sum()`, or a custom function.

# Customizing tick formats with formatters

Tick formatters control the formats of tick labels. It is used similarly to tick locators, as follows:

```py
ax.xaxis.set_major_formatter(xmajorFormatter)    
ax.xaxis.set_minor_formatter(xminorFormatter)
ax.yaxis.set_major_formatter(ymajorFormatter)
ax.yaxis.set_minor_formatter(yminorFormatter)
```

# Using a non-linear axis scale

Depending on the distribution of our data, a linear scale may not be the best way to fit in all useful data points in a figure. In this case, we may need to modify the scale of the axes into a log or symmetric log scale. In Matplotlib, this can be done by `plt.xscale()` and `plt.yscale()` before defining the axes, or by `ax.set_xscale()` and `ax.set_yscale()` after an axis is defined.

We do not need to change the scale of the entire axis. To display a part of the axis in linear scale, we adjust the linear threshold with the argument `linthreshx` or `linthreshy`. To obtain a smooth continuous line, we can also mask the non-positive numbers with the argument `nonposx` or `nonposy`.

The following code snippet is an example of the different axis scales. For a simpler illustration, we only change the scale in the `y` axis. Similar operations can be applied to the `x` axis:

```py
import numpy as np
import matplotlib.pyplot as plt

# Prepare 100 evenly spaced numbers from -200 to 200
x = np.linspace(-1000, 1000, 100)
y = x * 2
# Setup subplot with 3 rows and 2 columns, with shared x-axis.
# More details about subplots will be discussed in Chapter 3.
f, axarr = plt.subplots(2,3, figsize=(8,6), sharex=True)
for i in range(2):
    for j in range(3):
        axarr[i,j].plot(x, y)
        # Horizontal line (y=10)
        axarr[i,j].scatter([0], [10])

# Linear scale
axarr[0,0].set_title('Linear scale')

# Log scale, mask non-positive numbers
axarr[0,1].set_title('Log scale, nonposy=mask')
axarr[0,1].set_yscale('log', nonposy='mask')

# Log scale, clip non-positive numbers
axarr[0,2].set_title('Log scale, nonposy=clip')
axarr[0,2].set_yscale('log', nonposy='clip')

# Symlog
axarr[1,0].set_title('Symlog scale')
axarr[1,0].set_yscale('symlog')

# Symlog scale, expand the linear range to -100,100 (default=None)
axarr[1,1].set_title('Symlog scale, linthreshy=100')
axarr[1,1].set_yscale('symlog', linthreshy=100)

# Symlog scale, expand the linear scale to 3 (default=1)
# The linear region is expanded, while the log region is compressed.
axarr[1,2].set_title('Symlog scale, linscaley=3')
axarr[1,2].set_yscale('symlog', linscaley=3)
plt.show()
```

Let's compare the results of each axis scale in the following figure:

![](img/f251b6f4-a95a-42bc-9abd-6e3dd65e6920.png)

# More on Pandas-Matplotlib integration

Pandas provides the DataFrame data structure commonly used in handling multivariate data. When we usually use the Pandas package for data I/O, storage, and preprocessing, it also provides a number of native integrations with Matplotlib for quick visualization. 

To create these plots, we can call `df.plot(kind=plot_type)`, `df.plot.scatter()`, and so on. Here is a list of available plot types:

*   `line`: Line plot (default)
*   `bar`: Vertical bar plot
*   `barh`: Horizontal bar plot
*   `hist`: Histogram
*   `box`: Boxplot
*   `kde`: **Kernel Density Estimation** (**KDE**) plot
*   `density`: The same as `kde`
*   `area`: Area plot
*   `pie`: Pie plot

We have created some of the simpler graphs in the previous chapters. Here, we will take the density plot as an example for discussion.

# Showing distribution with the KDE plot

Similar to a histogram, the KDE plot is a method to visualize the shape of data distribution. It uses kernel smoothing to create smooth curves and is often combined with a histogram. It is useful in exploratory data analysis.

In the following example, we will compare the income in various age groups across different countries, with data obtained from surveys binned with different age groupings.

Here is the code for data curation:

```py
import pandas as pd
import matplotlib.pyplot as plt

# Prepare the data
# Weekly earnings of U.S. wage workers in 2016, by age
# Downloaded from Statista.com
# Source URL: https://www.statista.com/statistics/184672/median-weekly-earnings-of-full-time-wage-and-salary-workers/
us_agegroups = [22,29.5,39.5,49.5]
# Convert to a rough estimation of monthly earnings by multiplying 4
us_incomes = [x*4 for x in [513,751,934,955]]

# Monthly salary in the Netherlands in 2016 per age group excluding overtime (Euro)
# Downloaded from Statista.com 
# Source URL: https://www.statista.com/statistics/538025/average-monthly-wage-in-the-netherlands-by-age/
# take the center of each age group
nl_agegroups = [22.5, 27.5, 32.5, 37.5, 42.5, 47.5, 52.5]
nl_incomes = [x*1.113 for x in [1027, 1948, 2472, 2795, 2996, 3069, 3070]]

# Median monthly wage analyzed by sex, age group, educational attainment, occupational group and industry section
# May-June 2016 (HKD)
# Downloaded form the website of Censor and Statistics Department of the HKSAR government
# Source URL: https://www.censtatd.gov.hk/fd.jsp?file=D5250017E2016QQ02E.xls&product_id=D5250017&lang=1
hk_agegroups = [19.5, 29.5, 39.5, 49.5]
hk_incomes = [x/7.770 for x in [11900,16800,19000,16600]]
```

Let's now draw the KDE plots for comparison. We have prepared a reusable function to plot the three pieces of data with less repetition in the code:

```py
import seaborn as sns
def kdeplot_income_vs_age(agegroups,incomes):
    plt.figure()
    sns.kdeplot(agegroups,incomes)
    plt.xlim(0,65)
    plt.ylim(0,6000)
    plt.xlabel('Age')
    plt.ylabel('Monthly salary (USD)')
    return

kdeplot_income_vs_age(us_agegroups,us_incomes)
kdeplot_income_vs_age(nl_agegroups,nl_incomes)
kdeplot_income_vs_age(hk_agegroups,hk_incomes)
```

Now we can look at the results, which are from top to bottom for the US, the Netherlands, and Hong Kong, respectively:

![](img/a4482080-2a6e-43fc-85d1-fa49f955b6fc.png)  ![](img/b4c0fe40-280e-4823-a56c-eb7af4e5b34a.png)![](img/bac62f69-6f70-4e25-b22f-8b3bfa51d3de.png)

Of course, the figure is not a very accurate reflection of the original data, as extrapolation was involved before any tweaking (for instance, we do not have child labor data here, but the contours extend even to children below age **10**). Yet, we can observe a general difference in the pattern of income structures between ages **20** and **50** across the three economies, and to what extent the downloaded public data is comparable. We may then be able to suggest surveys with more useful groupings and perhaps to get more raw data points to suit our analyses.

# Showing the density of bivariate data with hexbin plots

Scatter plot is a common method to show the distribution of data in a more raw form. But when data density goes over a threshold, it may not be the best visualization method as points can overlap and we lose information about the actual distribution.

A hexbin map is a way to improve the interpretation of data density, by showing the data density in an area by color intensity.

Here is an example to compare the visualization of the same dataset that aggregates in the center:

```py
import pandas as pd
import numpy as np
# Prepare 2500 random data points densely clustered at center
np.random.seed(123)

df = pd.DataFrame(np.random.randn(2500, 2), columns=['x', 'y'])
df['y'] = df['y'] = df['y'] + np.arange(2500)
df['z'] = np.random.uniform(0, 3, 2500)

# Plot the scatter plot
ax1 = df.plot.scatter(x='x', y='y')
# Plot the hexbin plot
ax2 = df.plot.hexbin(x='x', y='y', C='z', reduce_C_function=np.max,gridsize=25)

plt.show()
```

This is the scatter plot in `ax1`. We can see that many data points are overlapping:

![](img/034ece3e-1b74-4f8b-bfe6-f1cba0a872d2.png)

As for the hexbin map in `ax2`, although not all discrete raw data points are shown, we can clearly see the variation of data distribution in the center:

![](img/03d572e0-8ea2-4e36-b963-bce115f3621e.png)

# Expanding plot types with Seaborn 

To install the Seaborn package, we open the terminal or command prompt and call `pip3 install --user seaborn`. For each use, we import the library by `import seaborn as sns`, where `sns` is a commonly used shorthand to save typing.

# Visualizing multivariate data with a heatmap

A heatmap is a useful visualization method to illustrate multivariate data when there are many variables to compare, such as in a big data analysis. It is a plot that displays values in a color scale in a grid. It is among the most common plots utilized by bioinformaticians to display hundreds or thousands of gene expression values in one plot.

With Seaborn, drawing a heatmap is just one line away from importing the library. It is done by calling `sns.heatmap(df)`, where `df` is the Pandas DataFrame to be plotted. We can supply the `cmap` parameter to specify the color scale `("colormap")` to be used. You can revisit the previous chapter for more details on colormap usage.

To get a feel for heatmap, in the following example, we demonstrate the usage with the specification of the *7^(th)* and *8^(th)* generations of Intel Core CPUs, which involves dozens of models and four chosen metrics. Before looking at the plotting code, let's look at the structure of the Pandas DataFrame that stores the data:

```py
# Data obtained from https://ark.intel.com/#@Processors
import pandas as pd

cpuspec = pd.read_csv('intel-cpu-7+8.csv').set_index('Name')
print(cpuspec.info())
cpuspec.head()
```

From the following screen capture of the output, we see that we simply put the labels as the index and different properties in each column:

![](img/6044ea05-9d53-466c-8f5c-c29f742f8544.png)

Notice that there are 16 models that do not support boosting without the **Max Frequency** property value. It makes sense to consider the **Base Frequency** as the maximum for our purpose here. We will fill in the **NA** values with the `'Max Frequency'` by the corresponding `'Base Frequency'`:

```py
cpuspec['Max Frequency'] = cpuspec['Max Frequency'].fillna(cpuspec['Base Frequency'])
```

Now, let's draw the heatmap with the following code:

```py
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(13,13))
sns.heatmap(cpuspec.drop(['Gen'],axis=1),cmap='Blues')
plt.xticks(fontsize=16)
plt.show()
```

Simple, isn't it? Only one line of code actually draws the heatmap. This is also an example of how we can use basic Matplotlib code to adjust other fine details of the plot, such as figure dimensions and the `xticks` font size in this case. Let's look at the result:

![](img/ae986f5c-53a7-408d-b421-66246feee4d1.png)

From the figure, even if we have absolutely no idea about these CPU models, we can easily infer something from the darker colors at the top among the i7 models. They are designed for higher performance with more core and cache space.

# Showing hierarchy in multivariate data with clustermap

Sometimes, a heatmap illustration can be hard to interpret when there are too many alternating color bands. This is because our data may not be ordered in terms of similarity. In this case, we need to group more similar data together in order to see the structure.

For this purpose, Seaborn offers the `clustermap` API, which is a combination of heatmap and dendrogram. A dendrogram is a tree-shaped graph that clusters more similar variables under the same branches/leaves. Drawing a dendrogram involves generally unsupervised hierarchical clustering, which is run in the background by default when we call the `clustermap()` function.

Besides unsupervised clustering, if we have a priori knowledge of certain labels, we can also show it in colors with the `row_colors` keyword argument.

Here, we extend from the preceding heatmap example of CPU models, draw a clustered heatmap, and label the generation as row colors. Let's look at the code:

```py
import seaborn as sns

row_colors = cpuspec['Gen'].map({7:'#a2ecec',8:'#ecaabb'}) # map color values to generation
sns.clustermap(cpuspec.drop(['Gen'],axis=1),standard_scale=True,cmap='Blues',row_colors=row_colors);
```

Again, calling the API is just as simple as the earlier heatmap, and we have generated the following figure:

![](img/321092df-964b-47e8-8b69-3d92fb9116c2.png)

Other than being helpful in showing multiple properties of a larger number of samples, with some tweaking, clustermap can also be used in pairwise clustering to show the similarity among samples with all the available properties considered together.

To draw a pairwise clustering heatmap, we have to first calculate the correlation between samples from the various property values, convert the correlation matrix into a distance matrix, and then perform hierarchical clustering to generate linkage values for dendrogram plotting. We use the `scipy` package for this purpose. To understand more about linkage calculation methods, please refer to the SciPy documentation.

We will provide the user-defined function here:

```py
from scipy.cluster import hierarchy
from scipy.spatial import distance
import seaborn as sns

def pairwise_clustermap(df,method='average',metric='cityblock',figsize=(13,13),cmap='viridis',**kwargs):
    correlations_array = np.asarray(df.corr())

    row_linkage = hierarchy.linkage(
    distance.pdist(correlations_array), method=method)

    col_linkage = hierarchy.linkage(
    distance.pdist(correlations_array.T), method=method)

    g = sns.clustermap(correlations, row_linkage=row_linkage, col_linkage=col_linkage, \
    method=method, metric=metric, figsize=figsize, cmap=cmap,**kwargs)
    return g
```

Here is the result of the pairwise clustering plot:

![](img/94272b55-7117-4e23-95cc-353f6b98d980.png)

From both heatmaps, we can observe that, based on these four properties, the CPUs seem to be better clustered by the product line suffix such as **U**, **K**, and **Y** than by brand modifiers such as **i5** and **i7**. When we approach data, this is among the analytical skills where observation of the similarity within a large group is required.

# Image plotting

In analyzing images, the first step is to convert colors into numerical values. Matplotlib provides APIs to read and show an image matrix of RGB values. 

The following is a quick code example of reading an image into a NumPy array with `plt.imread('image_path')`, and we show it with `plt.imshow(image_ndarray)`. Make sure that the Pillow package is installed so that more image types other than PNG can be handled:

```py
import matplotlib.pyplot as plt
# Source image downloaded under CC0 license: Free for personal and commercial use. No attribution required.
# Source image address: https://pixabay.com/en/rose-pink-blossom-bloom-flowers-693155/
img = plt.imread('ch04.img/mpldev_ch04_rose.jpg')
plt.imshow(img)
```

Here is the original image displayed with the preceding code:

![](img/87fc37f7-8b79-4806-9aae-2b4c81089c0c.jpg)

After showing the original image, we will try to work with transforming the image by changing the color values in the image matrix. We will create a high-contrast image by setting the RGB values to either `0` or `255` (max) at the threshold of `160`. Here is how to do so:

```py
# create a copy because the image object from `plt.imread()` is read-only
imgcopy = img.copy() 
imgcopy[img<160] = 0
imgcopy[img>=160] = 255
plt.imshow(imgcopy)
plt.show()
```

This is the result of the transformed image. By artificially increasing the contrast, we have created a pop art image!

![](img/75fb4efd-ebc6-4134-b4db-8daf801ff55d.png)

To demonstrate a more practical use for the image processing feature of Matplotlib, we will demonstrate MNIST. MNIST is a famous dataset of handwritten digits. It is often used in tutorials of machine learning algorithms. Here, we will not go into details of machine learning but rather will try to recreate the scenario where we visually inspect the dataset during the exploratory data analysis stage. 

We can download the entire MNIST dataset from the official site at [http://yann.lecun.com/exdb/mnist/](http://yann.lecun.com/exdb/mnist/). To ease our discussion and introduce useful package for Python machine learning, we load the data from Keras, which is a high-level API that facilitates neural network implementation. The MNIST dataset from the Keras package contains 70,000 images, arranged in tuples of coordinates and corresponding labels to facilitate model training and testing when building neural networks.

Let's first import the package:

```py
from keras.datasets import mnist
```

The data is loaded only when `load_data()` is called. Because Keras is intended for training, the data is returned in tuples of training and testing datasets, each containing the actual image color values and labels, named `X` and `y` by convention here:

```py
(X_train,y_train),(X_test,y_test) = mnist.load_data()
```

When initially called, `load_data()` may take some time to download the MNIST dataset from the online database.

We can inspect the dimensions of the data as follows:

```py
for d in X_train, y_train, X_test, y_test:
    print(d.shape)
```

Here is the output:

```py
(60000, 28, 28)
(60000,)
(10000, 28, 28)
(10000,)
```

Finally let's take one of the images in the `X_train` set and plot it in black and white with `plt.imshow()`:

```py
plt.imshow(X_train[123], cmap='gray_r')
```

From the following figure, we can easily read seven with our bare eyes. In the case of solving real image recognition problems, we may sample some mis-called images and consider strategies to optimize our training algorithms:

![](img/a24cfbfa-a8f0-43db-bd90-800672d82eca.png)

# Financial plotting

There are situations where more raw values per time point are needed to understand a trend for prediction. The candlestick plot is a commonly used visualization in technical analysis in finance to show a price trend, most often seen in the stock market.  To draw a candlestick plot, we can use the `candlestick_ohlc` API in the `mpl_finance` package.

`mpl_finance` can be downloaded from GitHub. After cloning the repository in the Python site-packages directory, call `python3 setup.py install` in the terminal to install it.

`candlestick_ohlc()` takes the input of a Pandas DataFrame with five columns: `date` in floating-point numbers, `open`, `high`, `low`, and `close`.

In our tutorial, we use the cryptocurrency  market values as an example. Let's again look at the data table we obtained:

```py
import pandas as pd
# downloaded from kaggle "Cryptocurrency Market Data" dataset curated by user jvent
# Source URL: https://www.kaggle.com/jessevent/all-crypto-currencies
crypt = pd.read_csv('crypto-markets.csv')
print(crypt.shape)
crypt.head()
```

Here is the how the table looks:

![](img/be107386-df27-4788-a10a-145b23e535c1.png)

Let's select the first cryptocurrency, Bitcoin, as our example. The following code selects the OHLC values in the month of December 2017 and sets the index as `date` in the datetime format:

```py
from matplotlib.dates import date2num
btc = crypt[crypt['symbol']=='BTC'][['date','open','high','low','close']].set_index('date',drop=False)
btc['date'] = pd.to_datetime(btc['date'], format='%Y-%m-%d').apply(date2num)
btc.index = pd.to_datetime(btc.index, format='%Y-%m-%d')
btc = btc['2017-12-01':'2017-12-31']
btc = btc[['date','open','high','low','close']]
```

Next, we will draw the candlestick plot. Recall the techniques to set axis ticks to fine-tune time markers:

```py
import matplotlib.pyplot as plt
from matplotlib.dates import WeekdayLocator, DayLocator, DateFormatter, MONDAY
from mpl_finance import candlestick_ohlc
# from matplotlib.finance import candlestick_ohlc deprecated in 2.0 and removed in 2.2
fig, ax = plt.subplots()

candlestick_ohlc(ax,btc.values,width=0.8)
ax.xaxis_date() # treat the x data as dates
ax.xaxis.set_major_locator(WeekdayLocator(MONDAY)) # major ticks on the Mondays
ax.xaxis.set_minor_locator(DayLocator()) # minor ticks on the days
ax.xaxis.set_major_formatter(DateFormatter('%Y-%m-%d'))

# Align the xtick labels
plt.setp(ax.get_xticklabels(), horizontalalignment='right')

# Set x-axis label
ax.set_xlabel('Date',fontsize=16) 

# Set y-axis label
ax.set_ylabel('Price (US $)',fontsize=16) 
plt.show()
```

`mpl_finance` can be installed by running the following command:

```py
pip3 install --user https://github.com/matplotlib/mpl_finance/archive/master.zip
```

We can observe that the rapid rise of Bitcoin values in early December turns its direction in mid-December 2017:

![](img/61517768-e7bb-4fd9-8122-23f2947ec253.png)

# 3D plots with Axes3D

We have so far discussed plotting in two dimensions. In fact, there are numerous occasions where we may need 3D data visualizations. Examples include illustrating more complex mathematical functions, terrain features, fluid dynamics in physics, as well as just showing one more facet of our data.

In Matplotlib, it can done by `Axes3D` in the `mplot3d` library within `mpl_toolkits`.

We just need to specify `projection='3d'` when defining an axes object after importing the library. Next, we just have to define the axes with `x`, `y`, and `z` coordinates. Supported plot types include scatter plot, line plot, bar plot, contour plots, wireframe plots, and surface plots with or without triangulation.

The following is an example of drawing a 3D surface plot:

```py
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

x = np.linspace(-2, 2, 60)
y = np.linspace(-2, 2, 60)
x, y = np.meshgrid(x, y)
r = np.sqrt(x**2 + y**2)
z = np.cos(r)
surf = ax.plot_surface(x, y, z, rstride=2, cstride=2, cmap='viridis', linewidth=0)
```

Matplotlib Axes3D is useful for plotting simple 3D plots with the usual Matplotlib syntax and appearance. For advanced scientific 3D plotting with Python demanding high rendering, it is recommended to use the Mayavi package. Here is the official website of the project for more details: [http://code.enthought.com/pages/mayavi-project.html](http://code.enthought.com/pages/mayavi-project.html).

From the following screenshot, we see that the color gradient also helps in portraying the shape of the 3D plot:

![](img/31b62490-cdcf-4714-8a62-ca2c101ae8a7.png)

# Geographical plotting

To demonstrate the power of Matplotlib with third-party packages, we will illustrate its usage in spatial analysis. Since the invention of satellites, a myriad of useful **Geographics Information System** (**GIS**) data has been generated to facilitate various analyses, from natural phenomena to human activities.

To utilize this data, there are common Python packages integrated with Matplotlib to show spatial data on a map, such as Basemap, GeoPandas, Cartopy, and Descartes. In this final section of the chapter, we will briefly introduce the usage of the first two packages.

# Basemap

Basemap is among the most popular Matplotlib-based plotting toolkits to plot over world maps. It is a handy way to show any geographical location.

To install Basemap, do this:

1.  Unpack in `$Python3_dir/site-packages/mpl_toolkits`
2.  Enter the Basemap installation directory: `cd $basemap_dir`
3.  Enter the `geos` directory in the `Basemap` directory: `cd $basemap/geos-3.3.3`
4.  Install the GEOS library via `./configure`, `make`, and `make install`
5.  Install PyProj (refer to the following tip)
6.  Return to the Basemap installation directory and run `python3 setup.py install`
7.  Set the environment variable `` `PROJ_DIR=$pyproj_dir/lib/pyproj/data` ``

Basemap requires PyProj as a dependency, where there are recurrent reports of installation failures. We recommend installing  from GitHub with prior installation of the Cython dependency.

1.  Clone the PyProj GitHub repository from [https://github.com/jswhit/pyproj](https://github.com/jswhit/pyproj) into the Python site packages directory
2.  Install the Cython dependency with `pip install --user cython`
3.  Enter the PyProj directory and install using `python3 setup.py install` 

For Windows users, it could be easier to install via Anaconda with the command prompt `conda install -c conda-forge geopandas`.

As a brief introduction, we will show how to draw our beautiful Earth with shaded terrain with the following code snippet:

```py
from mpl_toolkits.basemap import Basemap
import matplotlib.pyplot as plt

# Initialize a Basemap object
# Use orthogonal spherical projection
# Adjust the focus by setting the latitude and longitude
map = Basemap(projection='ortho', lat_0=20, lon_0=80)

# To shade terrain by relief. This step may take some time.
map.shadedrelief()

# Draw the country boundaries in white
map.drawcountries(color='white')
plt.show()
```

Here is how the plot looks:

![](img/f15542ca-4594-4baf-b966-f2ac7abbf9b3.png)

Besides showing the earth as a sphere with orthogonal projection as shown in the preceding figure, we can also set `projection='cyl'` to use the Miller Cylindrical projection for a flat rectangular illustration.

Basemap provides plenty map drawing functions, such as drawing coastlines and plotting data over maps with hexbin or streamplot. Their details can be found in the official tutorials at [http://basemaptutorial.readthedocs.io](http://basemaptutorial.readthedocs.io). As in-depth geographical analysis is beyond the scope of this book, we will leave a more specific exploration of its usage as an exercise for interested readers.

# GeoPandas

GeoPandas is a geographical plotting package integrated with Matplotlib. It has comprehensive functionalities to read common GIS file formats. 

To use GeoPandas, we will import the library as follows:

```py
import geopandas as gpd
import matplotlib.pyplot as plt
```

In the following example, we will explore the climate change data prepared by the World Bank Group.

We have selected the projection of precipitation in 2080-2099 based on scenario B1: a convergent world with global population peaking in mid-century and then declining. The storyline describes economies becoming more service- and information-oriented, with the introduction of clean and resource-efficient technologies but without additional climate initiatives. 

As input, we have downloaded the shapefile (`.shp`), which is one of the standard formats used in geographical data analysis:

```py
# Downloaded from the Climate Change Knowledge portal by the World Bank Group
# Source URL: http://climate4development.worldbank.org/open/#precipitation
world_pr = gpd.read_file('futureB.ppt.totals.median.shp')
world_pr.head()
```

We can look at the first few rows of the GeoPandas DataFrame. Note that the shape data is stored in the `geometry` column:

![](img/8d7306f6-ad9c-45e7-bdcd-4da14d11b7c2.png)

Next, we will add borders to the world map to better identify the locations:

```py
# Downloaded from thematicmapping.org
# Source URL http://thematicmapping.org/downloads/world_borders.php
world_borders = gpd.read_file('TM_WORLD_BORDERS_SIMPL-0.3.shp')
world_borders.head()
```

Here we inspect the GeoPandas DataFrame. The shape information is also stored in the `geometry` as expected:

![](img/6f45a74a-9b3a-4cb9-bc6e-e2166a2438b8.png)

The geometry data will be plotted as filled polygons. To draw the edges only, we will generate the geometry of borders by `GeoSeries.boundary`:

```py
# Initialize an figure and an axes as the canvas
fig,ax = plt.subplots()

# Plot the annual precipitation data in ax
world_pr.plot(ax=ax,column='ANNUAL')

# Draw the simple worldmap borders
world_borders.boundary.plot(ax=ax,color='#cccccc',linewidth=0.6)

plt.show()
```

Now, we have obtained this result:

![](img/3e6456a8-90b6-4080-8c12-d5816fdc3e37.png)

The website also provides data for another scenario, A2, which describes a very heterogeneous world, with local identities preserved. How will the picture look? Will it look similar or strikingly different? Let's download the file to find out!

Again, GeoPandas provides many APIs for more advanced usage. Readers can refer to `http://geopandas.org/` for the full documentation or further details.

# Summary

Congratulations! We have come a long way in our advanced usage of Matplotlib. In this chapter, we learned how to draw and share axes between subplots, use a non-linear axis scale, adjust tick formatters and locators, plot images, create advanced plots with Seaborn, create a candlestick plot for financial data, draw simple 3D plots with Axes3D, and visualize geographic data with Basemap and GeoPandas.

You're all set to dig deeper into integrating these skills with your desired applications. In the next few chapters, we will be working with the different backends supported by Matplotlib. Stay tuned!