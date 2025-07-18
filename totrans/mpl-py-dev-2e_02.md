

# Getting Started with Matplotlib

Now that we are familiar with the capabilities and functionalities of Matplotlib and all geared up with the Python environment, let's go straight ahead and create our first plots.

In this chapter, we will learn how to:

*   Draw basic line and scatter plots
*   Overlay multiple data series on the same plots
*   Adjust grids, axes, and labels
*   Add a title and legend
*   Save created plots as separate files
*   Configure Matplotlib global settings

# Loading data

Before we start plotting, we need to import the data we intend to plot and get familiar with basic plotting commands in Matplotlib. Let's start going through these basic commands!

While working on data visualization projects, we need to ensure that we have basic familiarity and understanding of the tools used for data processing. Before we begin, let's briefly revise the most common data structures you will encounter when handling data with Python.

# List

This is the most basic Python data structure; it stores a collection of values. While you can store any data type as an element in a Python list, for our purpose of data visualization, we mostly handle lists of numerical values as data input, or at, most, lists with elements of the same data type, such as strings to store text labels.

A list is specified by square brackets, `[]`. To initiate an empty list, assign `[]` to a variable by `l = []`. To create a list, we can write the following:

```py
fibonacci = [1,1,2,3,5,8,13]
```

Sometimes, we may want to get a list of arithmetic sequences. We may do so by using `list(range(start,stop,step))`.

See the following example:

```py
In [1]: fifths = list(range(10,30,5))
        fifths
Out[1]: [10, 15, 20, 25]

In [2]: list(range(10,30,5))==[10, 15, 20, 25]
Out[2]: True
```

Unlike Python 2.7, in Python 3.x, you cannot use the `range()` object interchangeably with a list.

# NumPy array

NumPy allows the creation of n-dimensional arrays, which is where the name of the data type, `numpy.ndarray`, comes from. It handles many sophisticated scientific and matrix operations. It provides many linear algebra and random number functionalities.

NumPy lies at the core of many calculations that computationally enable Matplotlib and many other Python packages. It is therefore a dependency for many common packages and often comes along with Python distributions. For instance, it provides the fundamental data structure for SciPy, a package that handles statistical calculations useful in science and many other areas.

To import NumPy, input this:

```py
import numpy as np
```

To create a NumPy array from lists, use the following:

```py
x = np.array([2,3,1,0])
```

You can also create non-integral arithmetic series with NumPy by using `np.linspace(start,stop,number)`. 

See the following example:

```py
In [1]: np.linspace(3,5,20)
Out[1]: array([ 3\.        ,  3.10526316,  3.21052632,  3.31578947,  3.42105263,
        3.52631579,  3.63157895,  3.73684211,  3.84210526,  3.94736842,
        4.05263158,  4.15789474,  4.26315789,  4.36842105,  4.47368421,
        4.57894737,  4.68421053,  4.78947368,  4.89473684,  5\.        ])
```

Matrix operations can be applied across NumPy arrays. Here is an example of multiplying two arrays:

```py
In [2]: a = np.array([1, 2, 1])
In [3]: b = np.array([2, 3, 8])
In [4]: a*b
Out[4]: array([2, 6, 8])
```

# pandas DataFrame

You may often see `df` appearing on Python-based data science resources and literature. It is a conventional way to denote the pandas DataFrame structure. pandas lets us perform the otherwise tedious operations on tables (data frames) with simple commands, such as `dropna()`, `merge()`, `pivot()`, and `set_index()`.

pandas is designed to streamline handling processes of common data types, such as time series. While NumPy is more specialized in mathematical calculations, pandas has built-in string manipulation functions and also allows custom functions to be applied to each cell via `apply()`.

Before use, we import the module with the conventional shorthand as:

```py
pd.DataFrame(my_list_or_array)
```

To read data from existing files, just use the following:

```py
pd.read_csv()
```

For tab-delimited files, just add `'\t'` as the separator: 

```py
pd.read_csv(sep='\t')
```

pandas supports data import from a wide range of common file structures for data handling and processing, from `pd.read_xlsx()` for Excel and `pd.read_sql_query()` for SQL databases to the more recently popular JSON, HDF5, and Google BigQuery.

pandas provides a collection of handy operations for data manipulation and is considered a must-have in a Python data scientist's or developer's toolbox.

We encourage our readers to seek resources and books on our Mapt platform to get a better and intimate understanding of the pandas library usage. 

To fully understand and utilize the functionalities, you may want to read more from the official documentation: 

[http://pandas.pydata.org/pandas-docs/stable/ ](http://pandas.pydata.org/pandas-docs/stable/)

# Our first plots with Matplotlib

We have just revised the basic ways of data handling with Python. Without further ado, let's create our first "Hello World!" plot example.

# Importing the pyplot

To create a pandas DataFrame from objects such as lists and ndarrays, you may call:

```py
import pandas as pd

```

To start creating Matplotlib plots, we first import the plotting API `pyplot` by entering this command:

```py
import matplotlib.pyplot as plt
```

This will start your plotting routine.

In Jupyter Notebook, you need to import modules once you begin a notebook session after starting the kernel.

# Line plot

After importing `matplotlib.pyplot` as `plt`, we draw line plots with the `plt.plot()` command.

Here is a code snippet for a simple example of plotting the temperature of the week:

```py
# Import the Matplotlib module
import matplotlib.pyplot as plt

# Use a list to store the daily temperature
t = [22.2,22.3,22.5,21.8,22.5,23.4,22.8]

# Plot the daily temperature t as a line plot
plt.plot(t)

# Show the plot
plt.show()
```

After you run the code, the following plot will be displayed as the output in the notebook cell:

![](img/6190a621-9bf5-4dc1-b51a-574f059e7759.png)

When a single parameter is parsed, the data values are assumed to be on the *y* axis, with the indices on the *x* axis.

Remember to conclude each plot with `plt.show()`. If you forget this, a plot object will be shown as the output instead of the plot. If you do not overwrite the plot with other plotting commands, you can call `plt.show()` in the next running cell to display the plot. The following is a screenshot made to illustrate the case:

![](img/4cfb8cab-1cc6-4afd-bf17-ee5e2c56e4d9.png)

Also, if you run the plotting commands multiple times before calling `plt.show()`, multiple plots or plots with unexpected elements (for example, changed color) will appear in the output area the next time you add the line back and run. We can demonstrate this by duplicating the same plotting commands in two cells running consecutively before showing the plot. In the following screenshot, you will see a change in color from the default blue as previously, to brown. This is due to the blue, line plotted with the first command being covered with a second brown line from the second command:

![](img/db779b58-3596-46c0-946a-9db1a63320c8.png)

Do not be alarmed if this happens. You can rerun the cell and achieve the desired result. 

**Suppressing function output**: Sometimes the plot may show up without calling `plt.show()`, but the line of the `matplotlib` object also shows up without giving much useful information. We can put a semicolon (`;`) at the end of a code line to suppress its input. For instance, in the following quick example, we will not see the Matplotlib object  `[<matplotlib.lines.Line2D at 0x7f6dc6afe2e8>]` appear in the output when we put `;` after the plotting command:

![](img/cc40004f-ea25-44f7-a52a-ddb64eb492b9.png)

To specify a customized *x-*axis, simply supply it as the first argument to `plt.plot()`. Let's say we plot the temperatures from the date 11^(th). We can plot temperatures `t` against a list of dates `d` by calling `plt.plot(d,t)`. Here is the result, where you can observe the specified dates on the *x* axis:

![](img/518c1bd4-b587-467d-a887-ce0f31c68355.png)

# Scatter plot

Another basic plot type is scatter plot, a plot of dots. You can draw it by calling `plt.scatter(x,y)`. The following example shows a scatter plot of random dots:

```py
import numpy as np
import matplotlib.pyplot as plt

# Set the random seed for NumPy function to keep the results reproducible
np.random.seed(42)

# Generate a 2 by 100 NumPy Array of random decimals between 0 and 1
r = np.random.rand(2,100)

# Plot the x and y coordinates of the random dots on a scatter plot
plt.scatter(r[0],r[1])

# Show the plot
plt.show()
```

The following plot is the result of the preceding code:

![](img/d0de8183-eaeb-4c66-9518-62ad59f46117.png)

# Overlaying multiple data series in a plot

We can stack several plotting commands before concluding a plot with `plt.show()` to create a plot with multiple data series. Each data series can be plotted with the same or different plot types. The following are examples of line plots and scatter plots with multiple data series, as well as a combination of both plot types show trends.

# Multiline plots

For example, to create a multiline plot, we can draw a line plot for each data series before concluding the figure. Let's try plotting the temperatures of three different cities with the following code:

```py
import matplotlib.pyplot as plt

# Prepare the data series
d = [11,12,13,14,15,16,17]
t0 = [15.3,15.4,12.6,12.7,13.2,12.3,11.4]
t1 = [26.1,26.2,24.3,25.1,26.7,27.8,26.9]
t2 = [22.3,20.6,19.8,21.6,21.3,19.4,21.4]

# Plot the lines for each data series
plt.plot(d,t0)
plt.plot(d,t1)
plt.plot(d,t2)

plt.show()
```

Here is the plot generated by the preceding code:

![](img/47436e31-9f6c-457a-b559-266c4b352dc5.png)

This example is adapted from the maximum temperatures of three cities in a week in December 2017\. From the graph, can you recognize which two lines are more likely to be representative of cities from the same continent?

# Scatter plot to show clusters

While we have seen a scatter plot of random dots before, scatter plots are most useful with representing discrete data points that show a trend or clusters. Each data series will be plotted in different color per plotting command by default, which helps us distinguish the distinct dots from each series. To demonstrate the idea, we will generate two artificial clusters of data points using a simple random number generator function in NumPy, shown as follows:

```py
import matplotlib.pyplot as plt

# seed the random number generator to keep results reproducible
np.random.seed(123) 

# Generate 10 random numbers around 2 as x-coordinates of the first data series
x0 = np.random.rand(10)+1.5

# Generate the y-coordinates another data series similarly
np.random.seed(321) 
y0 = np.random.rand(10)+2
np.random.seed(456)
x1 = np.random.rand(10)+2.5
np.random.seed(789)
y1 = np.random.rand(10)+2
plt.scatter(x0,y0)
plt.scatter(x1,y1)

plt.show()
```

We can see from the following plot that there are two artificially created clusters of dots colored blue (approximately in the left half) and orange (approximately in the right half) respectively:

![](img/02c33f72-c65c-4581-9983-abdcda054fc3.png)

There is another way to generate clusters and plot them on scatter plots. We can generate clusters of data points for testing and demonstration more directly using the `make_blobs()` function in a package called `sklearn`, which is developed for more advanced data analysis and data mining, as shown in the following snippet. We can specify the colors according to the assigned feature (cluster identity):

```py
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs

# make blobs with 3 centers with random seed of 23
blob_coords,features = make_blobs(centers=3, random_state=23)

# plot the blobs, with c value set to map colors to features
plt.scatter(blob_coords[:, 0], blob_coords[:, 1], marker='x', c=features) 
plt.show()
```

As the `make_blob` function generates dots based on isotropic Gaussian distribution, we can see from the resultant plot that they are better clustered as three distinct blobs of dots centralized at three points:

![](img/3130bd82-e187-45e2-9206-ce9f4801210f.png)

scikit-learn is a powerful Python package that provides many different simple functions for data mining and data analysis. It has a versatile suite of algorithms for classification, regression, clustering, dimension reduction, and modeling. It also allows data preprocessing and pipelining multiple processing stages.

For getting familiar with the scikit-learn library, we can use the datasets preloaded in package, such as the famous iris petals, or generate datasets according to the specified distribution as shown before. Here we'll only use the data to illustrate a simple visualization using scatter plot, and we won't go into the details just yet. More examples are available and can be found by clicking on this link:

[http://scikit-learn.org/stable/auto_examples/datasets/plot_random_dataset.html ](http://scikit-learn.org/stable/auto_examples/datasets/plot_random_dataset.html)

The previous example is a demonstration of an easier way to map dot color with labeled features, if any. The details of `make_blobs()` and other scikit-learn functions are out of our scope of introducing basic plots in this chapter.

We encourage our readers to seek resources and books on our Mapt platform to get a better understanding of the scikit-learn library usage. 

 Alternatively, readers can also read the scikit-learn documentation here:[ http://scikit-learn.org](https://scikit-learn.org).

# Adding a trendline over a scatter plot

Multiple plot types can be overlaid on top of each other. For example, we can add a trendline over a scatter plot. The following is an example of adding a trendline to 10 *y* coordinates with slight deviations from a linear relationship with the *x* coordinates:

```py
import numpy as np
import matplotlib.pyplot as plt

# Generate th
np.random.seed(100)
x = list(range(10))
y = x+np.random.rand(10)-0.5

# Calculate the slope and y-intercept of the trendline
fit = np.polyfit(x,y,1)

# Add the trendline
yfit = [n*fit[0] for n in x]+fit[1]
plt.scatter(x,y)
plt.plot(yfit,'black')

plt.show()
```

We can observe from the following plot that the trendline overlays the upward sloping dots:

![](img/b7fb0e78-0d20-42d3-9de9-923a9089efe7.png)

# Adjusting axes, grids, labels, titles, and legends

We have just learned how to turn numerical values into dots and lines with Matplotlib. By default, Matplotlib optimizes the display by calculating various values in the background, such as the reasonable axis range and font sizes. However, good visualization often requires more design input to suit our custom data visualization needs and purpose. Moreover, text labels are needed to make figures informative in many cases. In the following sections, we will demonstrate the methods to adjust these elements.

# Adjusting axis limits

While Matplotlib automatically chooses the range of *x* and *y* axis limits to spread data onto the whole plotting area, sometimes we want some adjustment, such as to show 100% as maximum instead of somewhere lower. To set the limits of *x* and *y* axes, we use the commands `plt.xlim()` and `plt.ylim()`. In our daily temperature example, the auto-scaling makes the temperature changes of less than 2 degrees Celsius seem very dramatic. Here is how we can adjust it, say, to show 0 degrees as the lower limit on the *y* axis for temperatures of the first 5 days only:

```py
import matplotlib.pyplot as plt

d = [11,12,13,14,15,16,17]
t0 = [15.3,12.6,12.7,13.2,12.3,11.4,12.8]
t1 = [26.1,26.2,24.3,25.1,26.7,27.8,26.9]
t2 = [22.3,20.6,19.8,21.6,21.3,19.4,21.4]

plt.plot(d,t0)
plt.plot(d,t1)
plt.plot(d,t2)

# Set the limit for each axis
plt.xlim(11,15)
plt.ylim(0,30)

plt.show()
```

The preceding code produces a plot with the *y* axis ranging from 0 to 30, as follows:

![](img/6eea3efd-23ac-4cbc-8430-f991b2c9875e.png)

# Adding axis labels

To give meaning to the values on the *x* and *y* axes, we need information about the nature and type of data, and its corresponding units. We can provide this piece of information by placing axis labels with `plt.xlabel()` or `plt.ylabel()`.

Let us continue with our example plot of multi-city temperatures. We shall add `plt.xlabel('Temperature (°C)')` and `plt.ylabel('Date')` to label the axes to produce the following plot:

![](img/33673c03-4e2c-4770-8631-ad1f9b75379a.png)

Similar to many other Matplotlib functions involving text, it is possible to set the text properties, such as font size and color within the `plt.xlabel()` and `plt.ylabel()` functions, by passing the properties as parameters. Here, we specified a bolder font weight for the labels for some hierarchy:

```py
plt.xlabel('Date',size=12,fontweight='semibold')
plt.ylabel('Temperature (°C)',size=12,fontweight='semibold')
```

As you can see, Matplotlib supports inline adjustment of fonts for many text elements. Here, we have specified a bolder font weight for the labels for some hierarchy:

![](img/22f5ddcb-256b-4bd7-bcef-5788f2d4b6af.png)

# Adding a grid

While a blank plot background is clean, sometimes we may like to get some reference gridlines for better reference, such as in the multiline case.

We can turn on the background gridlines by calling `plt.grid(True)` before `plt.show()`. For example, we can add this command to the preceding multi-city temperature plot to obtain the following plot:

![](img/68f37f3d-4252-4d87-98af-1850e442cb65.png)

Similarly, when we do not want the grid any longer, such as when using styles with gridlines as the default, we can use  `plt.grid(False)` to remove the grid.

Detailed styling options will be discussed in the next chapter. The grid in the preceding example distinctly stands out too much and interferes with the interpretation of the line plots. Grid line properties, such as line width, color, and dash patterns, are adjustable within the `plt.grid()` command; here is a brief example of making the grid more subtle:

```py
plt.grid(True,linewidth=0.5,color='#aaaaaa',linestyle='-')
```

As seen in the following plot, the grid lines become lighter and less interfering with the data lines in comparison to the default grid color in the plot in the last example:

![](img/02063f29-18b4-4af7-b37f-418a58b01947.png)

# Titles and legends

Depending on where and how our plots will be presented, they may or may not be accompanied by figure caption that describes the background and results that the data plot is illustrating. We may need to add a title to succinctly summarize and communicate the result.

Meanwhile, although axis labels suffice to identify data series for some figure types, such as bar plots and box plots, there may be cases where we need an extra legend key for this purpose. The following are ways to add and adjust these text elements to make our plot more informative.

# Adding a title

To describe the information of the plotted data, we can give a title to our figure. This can be done simply with the command `plt.title(yourtitle)`:

```py
plt.title("Daily temperature of 3 cities in the second week of December")
```

Again, we can specify text style properties. Here we set the title font to be larger than other labels:

```py
plt.title("Daily temperature of 3 cities in the second week of December", size=14, fontweight='bold')
```

The following is the plot with title added:

![](img/63ca8322-17d0-402f-832a-a75f56d063c7.png)

# Adding a legend

To match data series on plots with their labels, such as by their line styles and marker styles, we add the following:

```py
plt.legend()
```

The label of each data series can be specified within each `plt.plot()` command with the `label` parameter.

By default, Matplotlib chooses the best location to minimize overlap with data points and add transparency to the legend face color in case of overlapping. However, this does not always guarantee the location to be ideal in each case. To adjust the location, we can't pass `loc` settings , such as by using `plt.legend(loc='upper left')`.

Possible `loc` settings are as follows:

*   `'best'`: 0 (only implemented for axes' legends)
*   `'upper right'`: 1
*   `'upper left'`: 2
*   `'lower left'`: 3
*   `'lower right'`: 4
*   `'right'`: 5 (the same as 'center right'; for back-compatibility)
*   `'center left'`: 6
*   `'center right'`: 7
*   `'lower center'`: 8
*   `'upper center'`: 9
*   `'center'`: 10

You can also set `loc` as normalized coordinates with respect to the parent, which is usually the axes' area; that is, edges of axes are at 0 and 1\. For instance, `plt.legend(loc=(0.5,0.5))` sets the legend right in the middle.

Let's try to set the legend to our multiline plot in the lower-right corner with absolute coordinates, `plt.legend(loc=(0.64,0.1))`, as shown in the following created plot:

![](img/f73e8cf9-5d38-4903-8faf-82278b29ece1.png)

# A complete example

To get further acquainted with Matplotlib functions, let us plot a multiline plot with axes, labels, title, and legend configured in one single snippet.

In this example, we take real-world data from the World Bank on agriculture. As the world population continues to grow, food security continues to be an important global issue. Let us have a look at the production data of a few major crops in the recent decade by plotting a multiline plot with the following code:

```py
Data source: https://data.oecd.org/agroutput/crop-production.htm
OECD (2017), Crop production (indicator). doi: 10.1787/49a4e677-en (Accessed on 25 December 2017)
# Import relevant modules
import pandas as pd
import matplotlib.pyplot as plt

# Import dataset
crop_prod = pd.read_csv('OECD-THND_TONNES.txt',delimiter='\t')
years = crop_prod[crop_prod['Crop']=='SOYBEAN']['Year']
rice = crop_prod[crop_prod['Crop']=='RICE']['Value']
wheat = crop_prod[crop_prod['Crop']=='WHEAT']['Value']
maize = crop_prod[crop_prod['Crop']=='MAIZE']['Value']
soybean = crop_prod[crop_prod['Crop']=='SOYBEAN']['Value']

# Plot the data series
plt.plot(years, rice, label='Rice')
plt.plot(years, wheat, label='Wheat')
plt.plot(years, maize, label='Maize')
plt.plot(years, soybean, label='Soybean')

# Label the x- and y-axes
plt.xlabel('Year',size=12,fontweight='semibold')
plt.ylabel('Thousand tonnes',size=12,fontweight='semibold')

# Add the title and legend
plt.title('Total OECD crop production in 1995-2016', size=14, fontweight='semibold')
plt.legend()

# Show the figure
plt.show()​
```

From the resultant plot, we can observe the production of maize > wheat > soybean > rice, a generally growing trend of crop production and a steady growth of soybean production:

![](img/d71fbb5e-457e-4d05-8225-1e65ba9796e2.png)

# Saving plots to a file

To save a figure, we put `plt.savefig(outputpath)` at the end of plotting commands. It can be used in place of `plt.show()`, to directly save the figure without displaying it.

If you want to save the figure as a file as well as display it on the notebook output, you can call both `plt.savefig()` and `plt.show()`.

Reversing the order can result in the plot elements being cleaned up, leaving a blank canvas for the saved figure file.

# Setting the output format

`plt.savefig()` automatically detects the file extension of the specified output path, and generates the corresponding file format if it is supported. If no file extension is specified in the input, a PNG format file would be obtained as output with the default backend. This supports a number of image formats, including PNG, JPG, PDF, and PostScript:

```py
import numpy as np
import matplotlib.pyplot as plt
y = np.linspace(1,2000)
x = 1.0/np.sin(y)
plt.plot(x,y,'green')
plt.xlim(-20,20)
plt.ylim(1000,2400)
plt.show()
plt.savefig('123')
```

# Setting the figure resolution

Depending on the format, location, and purpose of display, each figure may require a different scale of resolution. Generally, large printed materials, such as posters, would require higher resolution. We can set the resolution by specifying the **dot per inch** (**DPI**) value, like this for example:

```py
plt.savefig(dpi=300)
```

For a *8x12* squared inches figure and output with 300 DPI, there will be *(8x300)x(12x300) = 2400x3600* pixels in the image.

# Jupyter support

Matplotlib is well integrated into Jupyter Notebook natively; such integration allows the plots to be displayed directly as static images as the output of each notebook cell. At times, we might be interested in the interactive GUI of Matplotlib, such as for zooming or panning a plot to view from different angles. We can continue working in the Jupyter Notebook with some simple steps.

# Interactive navigation toolbar

To access the interactive navigation toolbar in a Jupyter Notebook, first call the Jupyter cell magic:

```py
%matplotlib notebook
```

We will demonstrate with a plot with a more dynamic shape:

```py
import numpy as np
import matplotlib.pyplot as plt

y = np.linspace(1,2000)
x = 1.0/np.sin(y)

plt.plot(x,y,'green')
plt.xlim(-20,20)
plt.ylim(1000,2400)

plt.show()
```

As shown in the illustration, here we have a Christmas-tree-shaped plot embedded within a GUI box:

![](img/a361c7dc-9769-430a-856b-2d963e4e3bb4.png)

You can find a tool bar in the bottom-left corner. The button functions from left to right are as follows:

*   **Home logo**: Reset original view
*   **Left arrow**: Back to previous view
*   **Right arrow**: Forward to next view
*   **Four-direction arrow**: Pan by holding down the left mouse key; zoom with the right arrow key on the screen
*   **Rectangle**: Zoom by dragging rectangle on the plot
*   **Floppy disk icon**: Download the plot

Here is an example of panning by dragging on the plot:

![](img/2df9200c-449b-4835-9583-e0a162aae41d.png)

The following illustration shows the result of zooming by dragging a rectangle over the plot:

![](img/94e1f024-a31b-4226-a235-df76030cd382.png)

To revert to inline output mode, use the cell magic `%matplotlib inline`, or click on the power button in the top-right corner.

# Configuring Matplotlib

We have learned to tweak a few major elements in a Matplotlib plot. When we recurrently generate figures of similar style, it would be nice to have a way to store and apply the persistent global settings. Matplotlib offers a few options for configuration.

# Configuring within Python code

To keep settings throughout the current session, we can execute `matplotlib.rcParams` to override configuration file settings.

For instance, we can set the font size of all text in plots to 18 with the following:

```py
matplotlib.rcParams['font.size'] = 18
```

Alternatively we can call the `matplotlib.rc()` function. As `matplotlib.rc()` just changes one property, to change multiple settings, we can use the function `matplotlib.rcParams.update()`, and pass parameters in the form of a dictionary of key-value pairs:

```py
matplotlib.rcParams.update({'font.size': 18, 'font.family': 'serif'})
```

# Reverting to default settings

To revert to default settings, you can call `matplotlib.rcdefaults()` or `matplotlib.style.use('default')`.

# Global setting via configuration rc file

If you have a set of configurations that you want to apply globally without setting every time, you can set the `matplotlibrc` default values. To make persistent and selective changes on a certain set of parameters, we store the options in an `rc` configuration file. 

# Finding the rc configuration file

On Linux/Unix systems, you can set a global configuration for all users on the machine by editing `/etc/matplotlibrc` for `$HOME/.matplotlib/matplotlib/rc` or `~/.config/matplotlib/matplotlibrc`.

On Windows, the default `matplotlibrc` file may be placed in `C:\Python35\Lib\site-packages`. To find the path of the currently active `matplotlibrc` file, we can use the `matplotlib_fname()` function of Matplotlib in Python shell as follows:

```py
In [1]: import matplotlib as mpl
        mpl.matplotlib_fname()
Out[1]: '/home/mary/.local/lib/python3.6/site-packages/matplotlib/mpl-data/matplotlibrc'

```

The `rc` configuration file is found under `$INSTALL_DIR/matplotlib/mpl-data/matplotlibrc`, where `$INSTALL_DIR` is where you installed Matplotlib, which usually looks like `python3.6/site-packages/`. The `rc` file in the installation directory will be overwritten at each update. To keep the changes persistent across version updates, please keep them in the local configuration directory, such as `'/home/mary/.config/matplotlib/matplotlibrc'`. 

# Editing the rc configuration file

The basic format of the file is in the form of `option: value`. For example, to keep the legend always on the right, we put:

```py
legend.loc: right
```

Matplotlib provides massive configurability of plots, and there are several places to control the customization:

*   **Global machine configuration file**: Matplotlib is configured for each user using the global machine configuration file
*   **User configuration file**: A unique file for each user, where they can overwrite the global configuration file choosing their own settings (note that the user can execute a Matplotlib-related code anytime)
*   **Configuration file in the current directory**: The current script or program can be customized specifically by using this directory

This is particularly useful in situations when different programs have different needs, and using an external configuration file is better than hardcoding those settings in the code.

# Summary

Congratulations! We are now familiar with the basic plotting techniques using Matplotlib syntax! Remember, the success of a data visualization project relies heavily upon making appealing visuals.

In the next chapters, we will learn how to beautify our plots and select the right kind of plot that communicates our data effectively!
