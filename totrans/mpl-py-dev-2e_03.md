
# Decorating Graphs with Plot Styles and Types

In the previous chapter, we learned some basic concepts to draw line and scatter plots with Matplotlib, and made adjustments to a few elements. Now that we are familiar with the Matplotlib syntax, we are ready to go further and explore the potential of Matplotlib.

In this chapter, we will discuss:

*   Color specification
*   Line style customization
*   Point style customization
*   More native plot types
*   Inserting text and other annotations
*   Considerations in plot styling

# Controlling the colors

Color is an essential element in any visual. It can have a huge impact on how graphics are perceived. For example, sharp color contrast can be used to highlight a focus; a combination of several distinct colors is useful in setting up a hierarchy.

In Matplotlib 2, colors have been set by default to better differentiate between categories; and to perceive continuous numerical values more intuitively, yet we often need better control over the colors to represent our data. In this section, we will introduce the common color options in Matplotlib.

# Default color cycle

A color cycle is a list of colors used to control the color of a series of elements automatically, such as each data series in multiline plots. In Matplotlib 2.0, the default color cycle has expanded from 7 to 10 colors using the *category10* palette in **Data-Driven Documents** (**D3**)[https://github.com/d3](https://github.com/d3) and Vega, a declarative language for visualization grammar. These colors are designed to show good contrast between distinct categories. Each color is named `'C0'` to `'C9'`, and can be called in manually by specifying a color in the preset color cycle. Here is a toy example of a multiline plot with each color in the default cycle:

```py
import matplotlib.pyplot as plt
for i in range(10):
    plt.plot([i]*5,c='C'+str(i),label='C'+str(i))
plt.xlim(0,5)
plt.legend()
plt.show()
```

The following is the figure output. The legend displays the name of each color in the default cycle:

![](img/5da8130d-045f-4230-95bc-c7000c1c643b.png)

To access the hexadecimal codes of the colors, you may use the following code:

```py
import matplotlib as mpl 
mpl.rcParams['axes.prop_cycle']
```

# Single-lettered abbreviations for basic colors

There are several common colors with built-in, single-lettered standard abbreviations for quick use. They are listed as follows:

*   `'b'`: Blue
*   `'g'`: Green
*   `'r'`: Red
*   `'c'`: Cyan
*   `'m'`: Magenta
*   `'y'`: Yellow
*   `'k'`: Black
*   `'w'`: White

# Standard HTML color names

When we want to quickly build a palette from a wider range of colors, names in plain English may be more intuitive to start with than numerical code. There are more than a hundred different color names that are supported by HTML on all modern browsers. They are well supported in Matplotlib, for example, salmon, orange, yellow-green, chocolate, and cornflower blue.

You can find the full list with matched colors and names at: [https://matplotlib.org/examples/color/named_colors.html](https://matplotlib.org/examples/color/named_colors.html). The corresponding hexadecimal code is available at: [https://www.w3schools.com/colors/colors_names.asp](https://www.w3schools.com/colors/colors_names.asp).

# RGB or RGBA color code

A color can also be specified as a tuple of three to four float numbers between zero and one, such as `(0.1,0.1,0.2)` or `(0.2,0.2,0.3,0.8)`. The first three numbers define how much red, green, and blue light should be mixed into the desired color output. The optional fourth number is the alpha value to control the transparency level.

# Hexadecimal color code

Similar to RGBA values, hexademical (hex) color codes control the amount of red, green, and blue light. They also control transparency with a two-digit hex number, each starting with a hash sign `'#'`, for instance, `'#81d8d0ec'`. Therefore, pure red, green, blue, black, and white's hex codes are `'#ff0000'`, `'#00ff00'`, `'#0000ff'`, `'#000000'`, and `'#ffffff'`, respectively.

# Depth of grayscale

You can specify any value within *0-1* in a string of a float number, such as `'0.5'`. A smaller number gives a darker shade of gray.

# Colormaps

Colormaps map numerical values to a range of colors.

Since Matplotlib 2.0, the default colormap has been changed from `'jet'`, which spans the visible light spectrum from red to blue, to `'viridis'`, which is a perceptually uniform continuum from yellow to blue. This makes it more intuitive to perceive continuous values:

```py
import numpy as np
import matplotlib.pyplot as plt

N = M = 200
X, Y = np.ogrid[0:20:N*1j, 0:20:M*10]
data = np.sin(np.pi * X*2 / 20) * np.cos(np.pi * Y*2 / 20)

fig, (ax2, ax1) = plt.subplots(1, 2, figsize=(7, 3)) # cmap=viridis by default
im = ax1.imshow(data, extent=[0, 200, 0, 200])
ax1.set_title("v2.0: 'viridis'")
fig.colorbar(im, ax=ax1, shrink=0.85)

im2 = ax2.imshow(data, extent=[0, 200, 0, 200], cmap='jet')
fig.colorbar(im2, ax=ax2, shrink=0.85)
ax2.set_title("classic: 'jet'")

fig.tight_layout()
plt.show()
```

Check out the following image generated with the preceding code to understand what perceptual color uniformity means:

![](img/f8c5798f-add3-4613-9c72-952b6697cf7b.png)

Matplotlib also provides a number of preset colormaps that are optimized for displaying diverging values or qualitative categories. Feel free to check them out at: [https://matplotlib.org/2.1.0/tutorials/colors/colormaps.html](https://matplotlib.org/2.1.0/tutorials/colors/colormaps.html).

# Creating custom colormaps

We can set up our own colormap. This is useful when customizing heatmaps and surface plots.
A simple way to create a custom linear colormap is to prepare a list of colors and allow Matplotlib to handle the transition. Let's look at the following example:

```py
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors

# Create a 30 random dots
np.random.seed(52)
x,y,c = zip(*np.random.rand(30,3))

# Create a custom linear colormap
cmap = matplotlib.colors.LinearSegmentedColormap.from_list("", ["red","yellow","green"])

plt.scatter(x,y,c=c, cmap=cmap)
plt.colorbar()
plt.show()
```

Here, we have a scatter plot with a colormap set up on our own, morphing from `red` through `yellow` to `green`:

![](img/d12da00a-66b6-4719-aac4-aac066efe7c8.png)

# Line and marker styles

We have demonstrated how to draw line plots and scatter plots in the previous chapter. We know that scatter plots are made up of dots denoting each data point, whereas line plots are generated by joining dots of data points. In Matplotlib, the marker to mark the location of data points can be customized to have different styles, including shape, size, color, and transparency. Similarly, the line segments joining the data points as well as different 2D lines that share the same class in the object-oriented Matplotlib structure can have their styles adjusted, as briefly demonstrated in the grid section of the previous chapter. Adjusting marker and line styles is useful in making the data series more distinguishable, and sometimes for aesthetic considerations. In this section, we will go through the details and implementation methods of marker and line styles in Matplotlib.

# Marker styles

For markers denoting data points, we can adjust their shapes, sizes, and colors. By default, Matplotlib draws markers as single round dots. Here, we introduce the methods of adjustment.

# Choosing the shape of markers

There are dozens of available markers to denote data points. They are grouped into unfilled `markers` and the bolder `filled_markers`. Here are a few examples:

*   `'o'`: Circle
*   `'x'`: Cross
*   `'+'`: Plus sign
*   `'P'`: Filled plus sign
*   `'D'`: Filled diamond
*   `'s'`: Square
*   `'^'`: Triangle

We can access the keys and names of all available marker shapes under `mpl.lines.Line2D.markers`. The following is a code snippet for an overview of all our marker shapes:

```py
import matplotlib.pyplot as plt
from matplotlib.lines import Line2D

for i,marker in enumerate(Line2D.markers):
    plt.scatter(i%10,i,marker=marker,s=100) # plot each of the markers in size of 100
plt.show()
```

Here is the graphical output of the markers:

![](img/8b3e8a2e-494a-4f38-bd0c-85f8812d7845.png)

# Using custom characters as markers

Matplotlib supports the use of custom characters as markers, which now include mathtext and emoji. To use characters as custom markers, we concatenate two dollar signs `'$'`, each in front of and behind the character, and we pass them as the `marker` parameter.
The notations starting with a backslash `'\'`, such as `'\clubsuit'`, are in mathtext, which will be introduced later in this chapter (in the text and annotations section).

Here is an example of a scatter plot of markers in mathematical symbols and an emoji:

```py
import matplotlib.pyplot as plt
from matplotlib.lines import Line2D

custom_markers = ['$'+x+'$' for x in ['\$','\%','\clubsuit','\sigma','']]
for i,marker in enumerate(custom_markers):
    plt.scatter(i%10,i,marker=marker,s=500) # plot each of the markers in size of 100
plt.show()
```

As seen from the following figure, we have successfully used symbols, a Greek alphabet, as well as an emoji as custom markers in a scatter plot:

![](img/73032a2c-8422-42cf-a6df-12d57a65c6ef.png)

# Adjusting marker sizes and colors

In a scatter plot, we can specify the marker size with the parameter `s` and the marker color with `c` in the `plt.scatter()` function.

To draw markers on line plots, we first specify the shape of the markers in the `plt.plot()` function, such as `marker='x'`. Marker colors follow the line color.

Please note that scatter plots accept list types as size and color values, convenient in visualizing clusters, while line plots only accept a single value per data series.

Let's look at the following example:

```py
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors

# Prepare a list of integers
n = list(range(5))

# Prepare a list of sizes that increases with values in n
s = [i**2*100+100 for i in n]

# Prepare a list of colors
c = ['red','orange','yellow','green','blue']

# Draw a scatter plot of n points with sizes in s and colors in c
plt.scatter(n,n,s=s,c=c)

# Draw a line plot with n points with black cross markers of size 12
plt.plot(n,marker='x',color='black',ms=12)

# Set axis limits to show the markers completely
plt.xlim(-0.5,4.5)
plt.ylim(-1,5)

plt.show()
```

This code generates a figure of a scatter plot with marker sizes increasing with the data values, and a line plot with cross-shaped markers of a fixed size:

![](img/b55785d3-30ba-40ab-9699-550e1a0ec628.png)

# Fine-tuning marker styles with keyword arguments

We can have further refined control over marker styles with some more keyword arguments. For example, for `plt.plot()`, we can change the `markeredgecolor`, `markeredgewidth,` and `markerfacecolor`.

Here is a code example:

```py
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors

# Prepare data points
x = list(range(5))
y = [1]*5

# Set the face color, edge color, and edge width of markers
plt.plot(x,y,marker='o',ms=36,markerfacecolor='floralwhite',markeredgecolor='slateblue',markeredgewidth=10)

plt.show()
```

This is the result of adding the extra keyword arguments:

![](img/8f87b433-889d-4045-8ab3-4f14751cd7e3.png)

# Line styles

Lines are among the most frequently occurring elements in Matplotlib visualization, from those representing data series to those marking axes, grids, and outlines of any shape. Hence, it is important to understand how we can adjust line style. Lines in Matplotlib are controlled by the `Line2D` class. The object-oriented structure of Matplotlib makes it easy to adjust line styles through keyword arguments with similar grammar in each API. Here, we shall introduce the several most commonly tuned aspects of Matplotlib lines.

# Color

Setting the color of the lines in a line plot is as simple as adding a `color` or its shorthand `c` parameter to the `plt.plot()` command. The color option is available in many other Matplotlib APIs as well.

# Line thickness

The thickness is set by the `linewidth` or `lw` parameter in most Matplotlib elements involving lines, including line plots.

# Dash patterns

Dash patterns of lines are designated by the `linestyle` or `ls` parameter. It can sometimes be used as a positional argument for convenience. For example, in line plots, we can specify the following:

*   `'solid'` or `'-'`: Solid line; default
*   `'dashed'` or `'--'`: Equally spaced dashes
*   `'dashdot'` or `'-.'`: Alternate dashes and dots
*   `'.'`: Loose dotted line
*   `':'`: Packed dotted line
*   `'None'`, `' '`, `''`: No visible line
*   `(offset, on-off-dash-seq)`: Customized dashes

The following is an example of lines in different dash patterns:

```py
import matplotlib.pyplot as plt

# Prepare 4 data series of sine curves
y = [np.sin(i) for i in np.arange(0.0, 10.0, 0.1)]

dash_capstyles = ['-','--','-.','.',':']

# Plot each data series in different cap dash styles
for i,x in enumerate(dash_capstyles):
    plt.plot([n*(i+1) for n in y],x,label=x)

plt.legend(fontsize=16,loc='lower left')
plt.show()
```

We can see the effect of each dash style in the following figure:

![](img/95bae8d9-3487-475f-979e-462082353f63.png)

# Designing a custom dash style

Matplotlib does not limit us to its preset line styles. In fact, we can design our own dash patterns by specifying the length and space for each repeating dash unit, such as `(0, (5,3,1,3,1,3))`.

# Cap styles

Another parameter for tuning is `dash_capstyle`. It controls the style of the dash ends:

*   `'butt'`: Blunt end
*   `'projecting'`: Extends in length
*   `'round'`: Rounded end

To demonstrate the different cap styles, we have a code snippet of a multiline plots with thick lines to enlarge the dashes:

```py
import matplotlib.pyplot as plt

# Prepare 4 data series of sine curves
y = list(range(10))

dash_capstyles = ['butt','projecting','round']

# Plot each data series in different cap dash styles
for i,x in enumerate(dash_capstyles):
 plt.plot([n*(i+1) for n in y],lw=10,ls='--',dash_capstyle=x,label=x)

plt.legend(fontsize=16)
plt.show()
```

From the following figure, we can clearly see how blunt and round dashes give different impressions of sharpness and subtlety:

![](img/9a85cd40-61a0-414f-9d5d-ae50a91223ef.png)

# Spines

Spines in Matplotlib refer to the lines surrounding the axes of the plotting area. We can set each spine to have different line styles or to be invisible.
To begin, we first access the axes with `plt.gca()`, where **gca** stands for **get current axes**, and store it in a variable, say `ax`.
We then adjust the properties of each spine in `ax.spines` with either `'top'`, `'right'`, `'bottom'`, or `'left'`. The common settings include line widths, color, and visibility, which is demonstrated in the following code snippet.
The following is an example to remove the top and right spines, often seen as a convention in certain scientific plots, and to enhance visual simplicity in general. It is also common to thicken the remaining spines. The change in color is shown as a demonstration. We can adjust it to suit our overall design where the plot is displayed:

```py

import matplotlib.pyplot as plt

y = list(range(4))
plt.plot(y)

# Store the current axes as ax
ax = plt.gca()

# Set the spine properties
ax.spines['left'].set_linewidth(3)
ax.spines['bottom'].set_linewidth(3)
ax.spines['left'].set_color('darkblue')
ax.spines['bottom'].set_color('darkblue')
ax.spines['right'].set_visible(False)
ax.spines['top'].set_visible(False)

plt.show()
```

This will create a graph of blue spines on the left and bottom:

![](img/3c9551bf-bc93-44e6-a6b5-febc805bfeb7.png)

# More native Matplotlib plot types

Besides the most basic scatter and line plots, Matplotlib provides a versatile collection of plot types to serve different data visualization purposes. In this section, we will introduce the rationale of plot type selection and the usage of each type.

# Choosing the right plot

A successful visualization must communicate the message well. In order to achieve this goal, we need to have a good understanding of the nature of our data as well as the advantages and limitations of each plot type in illustrating different relationships in data.

In choosing the right plot type to display, we have the following considerations:

*   Number of variables
*   Distribution of data
*   Relationships between data series

# Histogram

Histograms are useful in surveying the distribution of data. For example, we can plot data on a histogram when we want to see some age groups distributed in a population, light exposure in a photograph, or the amount of precipitation in each month in a city.

In Matplotlib, we call the `plt.hist()` function with a linear array. Matplotlib will automatically group the set of data points into `bins` and plot out the frequencies for each bin in bars. We can also specify the bin size by `plt.hist(array,bins=binsize)`.

Here is an example of plotting a randomly generated binomial distribution:

```py
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(8)
x = np.random.binomial(100, 0.5, size=10000)
plt.hist(x,bins=20) # or plt.hist(x,20)
plt.show()
```

The histogram produced is as follows:

![](img/8e23ba30-9cd2-4931-9193-a05b3a1cd290.png)

# Bar plot

Bar plots are useful for comparing absolute levels of discrete data series. They are created by the function `plt.bar(labels,heights)` in Matplotlib.

Let's look at the example of the market capitalization of today's much hyped cryptocurrencies. The five top cryptocurrencies in terms of market capitalization are shown here:

```py
import matplotlib.pyplot as plt

# Data retrieved from https://coinmarketcap.com on Jan 8, 2018
# Prepare the data series
cc = ['BTC','XRP','ETH','BCH','ADA']
cap = [282034,131378,107393,49999,26137]

# Plot the bar chart
plt.bar(cc,cap)
plt.title('Market capitalization of five top cryptocurrencies in Jan 2018')
plt.xlabel('Crytocurrency')
plt.ylabel('Market capitalization (million USD)')
plt.show()
```

We can see from the following figure that instead of following the input order, Matplotlib outputs a figure of bars with labels sorted alphabetically:

![](img/8c3f9fbb-4c29-4699-ac95-f89b9fbc97b4.png)

To create bar plots with bars in the designated order, we can make use of Pandas and its Matplotlib integration. The procedure is as follows:

1.  Create a Pandas DataFrame `df`
2.  Plot the bar chart with `df.plot(kind='bar')`
3.  Set the labels of `xticks`
4.  Adjust the other plot properties
5.  Show the plot with `plt.show()`

Please note that, by default, `df.plot()` includes a legend. We need to specify `legend=False` to turn it off.

Here is an example to reorder the bar plot in the previous output figure:

```py
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({'cc':cc,'cap':cap}, legend=False)

ax = df.plot(kind='bar')
ax.set_xticklabels(df['cc'])

plt.title('Market capitalization of five top cryptocurrencies in Jan 2018')
plt.xlabel('Crytocurrency')
plt.ylabel('Market capitalization (million USD)')
plt.show()
```

![](img/65f71bc5-cb9c-481a-af62-81f1df523cfb.png)

# Setting bar plot properties

We can set the `width`, `color`, and `bottom` coordinates of the bars as keyword arguments in `plt.bar()`.

The bar `width` is set in ratios, whereas the color is set as introduced in the earlier section of this chapter.
For data that may include experimental or measurement errors, we can input lists of `yerr` (and `xerr`) values to show the accuracy.

# Drawing bar plots with error bars using multivariate data

We can easily create bar plots with multiple data series with Pandas `df.plot()`. This API also allows us to easily add error bars by supplying the `xerr` and `yerr` parameters. Let's have a look at an example that demonstrates the usage of this function along with bar property adjustment. 
The following code snippet draws a multibar plot to show the performance of an imaginary drug to treat inflammation, by comparing the level of an inflammatory protein before and after treatment of a drug and placebo as control:

```py
import pandas as pd
import matplotlib.pyplot as plt

# Prepare the data series
labels_drug = ['Drug (Before)', 'Drug (After)']
labels_placebo = ['Placebo (Before)', 'Drug (After)']
drug = [2.88,1.42]
placebo = [2.72,2.68]
yerr_drug = [0.12,0.08]
yerr_placebo = [0.24,0.13]

df = pd.DataFrame([drug,placebo])
df.columns = ['Before', 'After']
df.index = ['Drug','Placebo']

# Plot the bar chart with error bars
df.plot(kind='bar',width=0.4,color=['midnightblue','cornflowerblue'],\
        yerr=[yerr_drug,yerr_placebo])

plt.title('Effect of Drug A Treatment')
plt.xlabel('Condition')
plt.ylabel('[hsCRP] (mg/L)')
plt.xticks(rotation=0) # to keep the xtick labels horizontal
plt.legend(loc=(0.4,0.8))

plt.show()
```

Here, you get a paired bar chart for two conditions. It seems the drug may have some effect compared to the placebo control. Can you think of more examples of data to draw multibar plots?

![](img/6ac134b1-c01c-4cbd-b369-f35052dc0bc5.png)

Besides using Pandas, we can also call multiple `plt.bar()` commands to draw multiple series of bar charts. Note that we will have to shift the coordinates so that the bars do not overlap each other.

# Mean-and-error plots

For experimental sciences, a data point is often averaged from several repeats of experiments, necessitating the need to show the error range to illustrate the precision level. In this case, mean-and-error plots may be more suitable than bar charts. In Matplotlib, mean-and-error plots are generated by the `plt.errorbar()`API.

When the positive errors and negative errors are the same, we can input 1D arrays to error values to draw symmetric error bars. Otherwise, we input 2D arrays of `[positive errors, negative errors]` for asymmetric error bars. While it is more common to have plots with `y` errors only, error values for both `x` and `y` axes are supported.

By default, Matplotlib draws a line linking each error bar, with format `fmt` set to `'.-'`. For discrete datasets, we can add the keyword argument `fmt='.'` to remove the line. Let's go through a simple example:

```py
import matplotlib.pyplot as plt
import numpy as np

# Prepare data for a sine curve
x = np.arange(0, 5, 0.3)
y = np.sin(-x)

# Prepare random error to plot the error bar
np.random.seed(100)
e1 = 0.1 * np.abs(np.random.randn(len(y)))

# Plotting the error bar
plt.errorbar(x, y, yerr=e1, fmt='.-')
plt.show()
```

We now get a sine curve with error bars, as follows. Try to substitute it with some real testing data you get:

![](img/13b784a0-c887-4fdc-94ed-ca1e6bd90e77.png)

# Pie chart

Pie chart is a circular representation of component ratios. The angle, and hence the arc length of each sector ratio (also called **wedges**), presents the proportion that each component accounts for, relative to the whole.

Matplotlib provides the `plt.pie()` function to draw pie charts. We can label each sector with `labels` as well as the percentage with `autopct` automatically. For different ways to customize the string format of the percentages, you may refer to: [https://pyformat.info/](https://pyformat.info/).
To maintain the circular shape of our pie chart, we specify the same width and length for a square figure with `plt.figure(figsize=(n,n))`.
Here, we have an example of web server usage in the first week of January 2017:

```py
# Data obtained from https://trends.builtwith.com/web-server on Jan 06, 2017
import matplotlib.pyplot as plt
plt.figure(figsize=(4,4))

x = [0.31,0.3,0.14,0.1,0.15]
labels = ['nginx','Apache','IIS','Varnish','Others']
plt.pie(x,labels=labels,autopct='%1.1f%%')
plt.title('Web Server Usage Statistics')
plt.show()
```

The resultant pie chart is as follows:

![](img/ba0baf01-10db-4e84-a653-a42c35c4838d.png)

We can also separate each sector by passing a list of ratios to the keyword argument `explode`. For example, adding the argument `explode=[0.1]*5` to the preceding `plt.pie()` plot will generate the following result:

![](img/dc2379b5-99ce-4b60-9db6-c2e9e3bd2bd8.png)

Please note that if the input array sums up to less than 1, the output pie chart will be incomplete, as shown in the following example:

```py
import matplotlib.pyplot as plt
plt.figure(figsize=(4,4))
x = [0.1,0.3]
plt.pie(x)
plt.show()
```

As seen here, instead of a full circle, we have an incomplete fan-shaped plot:

![](img/4d2bafb5-0f0c-4de0-a4a0-a2679a74be35.png)

In that case, we have to explicitly specify the ratio of each term. For instance, in the preceding example, change `x = [0.1,0.3]` to `x = [0.25,0.75]`.

# Polar chart

A polar chart is used to display multivariate data and is also known as a radar chart or a spider chart. It is often seen in illustrations of strength in different aspects of different objects for comparison, such as the evaluation of the price and various specifications of a piece of hardware, or the abilities of a game character.

Moreover, polar plots are also useful in drawing mathematical functions, which we are going to demonstrate here. In Matplotlib, we draw polar charts with the command `plt.polar()`. Apart from the x, y coordinate system we are familiar with, polar coordinates are used for polar charts, angles, and radii. The central point is called the **pole**. Note that Matplotlib takes a degree unit for the angle input.
 Here is the code to draw a polar rose:

```py
import numpy as np
import matplotlib.pyplot as plt

theta = np.arange(0., 2., 1./180.)*np.pi
plt.polar(3*theta, theta/6)
plt.polar(theta, np.cos(6*theta))
plt.polar(theta, [1.2]*len(theta))

plt.savefig('mpldev_03_polarrose.png')
plt.show()
```

This is the result. How many petals do you see in the rose?

![](img/a3853537-fad8-45a1-9699-ce95b0b8dcb4.png)

We can also make use of the polar coordinate system to create charts such as a heatmap of wind speed of the earth in geography, or the surface temperature of a round object for engineers. We will leave these advanced uses for you as an exercise when you have completed this book.

# Controlling radial and angular grids

There are two functions to control the radial and angular grids: `rgrid()` and `thetagrid()` respectively. We can pass the `radii`, `labels`, and `angle` arguments to the `rgrid()` function, and `angles`, `labels`, and `frac` to the `thetagrid()` function, respectively.

# Text and annotations

To enhance the understanding of plot details, we may sometimes add in text annotations for explanation. We will now introduce the methods of adding and adjusting text in Matplotlib plots.

# Adding text annotations

We can add text to our plot by calling `plt.text(x,y,text)`; we specify the `x` and `y` coordinates and the text string.
Here is a quick example:

```py
plt.text(0.25,0.5,'Hello World!',fontsize=30)
plt.show()
```

You can see in this figure the **Hello World!** message appearing in the center of the plot:

![](img/4b742924-31ab-44b1-a66d-50cebdb847f6.png)

# Font

Here are some of the common font properties adjustable in Matplotlib:

*   **Font size**: Float or relative size, for example, smaller and x-large
*   **Font weight**: For example, bold or semibold
*   **Font style**: For example, italic
*   **Font family**: For example, Arial
*   **Rotation**: Angle in degrees; it is vertical or horizontal

Matplotlib now supports unicode and emoji.

# Mathematical notations

As a plotting tool, mathematical notations are common. We can use the in-built mathtext or LaTeX to render mathematical symbols in Matplotlib.

# Mathtext

To create a mathtext notation, we precede a string with r, such as `r'$\alpha'`. The following is a short code for demo:

```py
plt.title(r'$\alpha > \beta$')
plt.show()
```

Alpha plus beta in the following plot are printed by MathTex:

![](img/f6b487c8-2294-478a-9341-d8eb29f3942c.png)

# LaTeX support

Matplotlib supports LaTeX, although it renders slower than mathtext; accordingly, it allows more flexible text rendering. Here are more details of the LaTeX usage: [https://matplotlib.org/users/usetex.html](https://matplotlib.org/users/usetex.html).

# External text renderer

If we have LaTeX installed, we can allow the external LaTeX engine to render the text elements by `matplotlib.rc('text', usetex='false')`.

# Arrows

To point out specific features in a plot, we can draw arrows with the function `plt.arrow()`. This code illustrates the different available styles of arrow annotations:

```py
import matplotlib.pyplot as plt

plt.axis([0, 9, 0, 18])
arrstyles = ['-', '->', '-[', '<-', '<->', 'fancy', 'simple', 'wedge']
for i, style in enumerate(arrstyles):
 plt.annotate(style, xytext=(1, 2+2*i), xy=(4, 1+2*i), \
 arrowprops=dict(arrowstyle=style))
connstyles=["arc", "arc,angleA=10,armA=30,rad=15", \
 "arc3,rad=.2", "arc3,rad=-.2", "angle", "angle3"]

for i, style in enumerate(connstyles):
 plt.annotate("", xytext=(6, 2+2*i), xy=(8, 1+2*i), \
 arrowprops=dict(arrowstyle='->', connectionstyle=style))

plt.show()

```

It generates the following figure to list the available arrow shapes for annotation:

![](img/4b2bc6c2-4308-466c-9b59-79a4a647ee02.png)

# Using style sheets

We have learned to style our plots step by step so far. For more persistent and portable settings, we can apply a predefined global style via the `matplotlib.style` module:

```py
## Available styles
 Matplotlib provides a number of pre-built style sheets. You can check them out by with `matplotlib.style.available`.

import matplotlib as mpl
mpl.style.available

Out[1]: ['seaborn-talk',
'seaborn-poster',
'_classic_test',
'seaborn-ticks',
'seaborn-paper',
'ggplot',
'seaborn',
'seaborn-dark',
'seaborn-bright',
'seaborn-pastel',
'fivethirtyeight',
'Solarize_Light2',
'classic',
'grayscale',
'bmh',
'seaborn-dark-palette',
'seaborn-whitegrid',
'seaborn-white',
'dark_background',
'seaborn-muted',
'fast',
'seaborn-notebook',
'seaborn-darkgrid',
'seaborn-colorblind',
'seaborn-deep']
```

# Applying a style sheet

We can call `plt.style.use(stylename)` to apply a style. This function takes in built-in style sheets, local paths, and URLs.

# Creating own style sheet

You can also create your own style sheet. For the specifications of a Matplotlib style sheet file, please refer to the documentation page at: [http://matplotlib.org/users/customizing.html](http://matplotlib.org/users/customizing.html).

# Resetting to default styles

The effects set by style sheets are sustained through new plots. To reset to the default parameters, call `plt.rcdefaults()`.

# Aesthetics and readability considerations in styling

As visualization is about delivering messages, the more we think from the reader's perspective, the more effective it will be. An attractive graphic catches more attention. The easier to read a plot is, the more likely are readers to understand the message. Here are several basic principles in designing data plots.

# Suitable font styles

The hierarchy can use no more than three levels of font family, weight, and sizes. Use less fancy font families, Sans Serif font if possible. Make sure the font size is large enough to be legible

**Serif versus Sans Serif**
Serif means decorative edges on alphabets. And sans means without in French. As the names imply, Sans Serif fonts are plainer and more simplistic than Serif fonts in general. Let's take the most popular examples of default fonts in Microsoft Office. Times New Roman used in Office 2007 and before is a Serif font, whereas the newer Calibri is a Sans Serif font.

# Effective use of colors

*   Use sharper color contrasts for emphasis and distinction
*   Use extra colors with discretion, for example, one color only for one data series
*   Be friendly for readers with a color weakness; for example, avoid red-green combinations

# Keeping it simple

<q>"Less is more. "</q> <q>                                                                              –  Andrea del Sarto (The Faultless Painter) by Robert Browning</q>

This quote spells out the basic principle of the preceding suggestions. The philosophy of minimalist design inspires much of the most brilliant work, from architecture to graphic design. While the use of different colors and styles creates distinction and hierarchy as well as adding attractiveness to our graphics, we must reduce the fanciness wherever possible. This helps our readers focus on the major message, and also helps keep a professional impression for our figures.

# Summary

Congratulations! You have now mastered the most commonly used plots and the basic methods to customize plots. We are now ready to move on to more advanced Matplotlib usage.

In the next chapter, we will cover more plot types with the help of third-party packages, methods to optimize displays for multiple plots and axes in certain scales, as well as showing pixels in images. Stay tuned!