
# Matplotlib in the Real World

At this point, we hope you are equipped with the techniques of creating and customizing plots using Matplotlib. Let's build on top of the things we have learned so far, and begin our journey of understanding more advanced Matplotlib usage through real-world examples.

First, we will cover how to fetch online data, which is commonly obtained through an **application programming interface** (**API**) or plain old web scraping techniques. Next, we will explore how to integrate Matplotlib 2.x with other scientific computing packages in Python for visualizations of different data types.

# Typical API data formats

Many websites distribute data via their API, which bridges applications via standardized architecture. While we are not going to cover the details of using APIs here, we will cover the most common API data exchange formats, namely CSV and JSON.

Interested readers can visit site-specific documentations for the use of APIs.

We have briefly covered parsing of CSV files in [Chapter 4](456b0dc2-84d5-40f9-bf63-1ab4635cbac8.xhtml), *Advanced Matplotlib*. To aid your understanding, we are going to represent the same data using both CSV and JSON.

# CSV

**Comma-separated values** (**CSV**) is one of the oldest file formats, introduced long before the World Wide Web even existed. However, it is now becoming deprecated as other advanced formats such as JSON and XML are gaining popularity. As the name suggests, data values are separated by commas. The preinstalled `csv` package and the `pandas` package contain classes to read and write data in CSV format. The following CSV example defines a population table with two countries:

```py
Country,Time,Sex,Age,Value
United Kingdom,1950,Male,0-4,2238.735
United States of America,1950,Male,0-4,8812.309
```

# JSON

**JavaScript Object Notation** (**JSON**) is gaining popularity these days due to its efficiency and simplicity. JSON allows the specification of number, string, Boolean, array, and object. Python provides the default `json` package for parsing JSON. Alternatively, the `pandas.read_json` class can be used to import JSON as a pandas DataFrame. The preceding population table can be represented as JSON as follows:

```py
{
 "population": [
 {
 "Country": "United Kingdom",
 "Time": 1950,
 "Sex", "Male",
 "Age", "0-4",
 "Value",2238.735
 },{
 "Country": "United States of America",
 "Time": 1950,
 "Sex", "Male",
 "Age", "0-4",
 "Value",8812.309
 },
 ]
}
```

# Importing and visualizing data from a JSON API

Now, let's learn how to parse financial data from Quandl's API to create insightful visualizations. Quandl is a financial and economic data warehouse, storing millions of datasets from hundreds of publishers. The best thing about Quandl is that these datasets are delivered via the unified API, without worrying about the procedures to parse the data correctly. Anonymous users can get up to 50 API calls per day, or up to 500 free API calls if registered. Readers can sign up for a free API key at [https://www.quandl.com/?modal=register](https://www.quandl.com/?modal=register.).

At Quandl, every dataset is identified by a unique ID, as defined by the Quandl code on each search result web page. For example, the Quandl code `GOOG/NASDAQ_SWTX` defines the historical NASDAQ index data published by Google Finance. Every dataset is available in three different formats—CSV, JSON, and XML.

Although an official Python client library is available from Quandl, we are not going to use that, for the sake of demonstrating the general procedures of importing JSON data from an API. According to Quandl's documentation, we can fetch JSON-formatted data tables through the following API call:

```py
GET https://www.quandl.com/api/v3/datasets/{Quandl code}/data.json
```

To begin with, let's try to get the Big Mac index data from Quandl. The Big Mac index was invented by *The Economist* in 1986 as a lighthearted guide to whether currencies are at their **correct** level. It is based on the theory of **purchasing power parity** (**PPP**), and is considered an informal measure of currency exchange rates at PPP. It measures their value against a similar basket of goods and services, in this case a Big Mac. Differing prices at market exchange rates would imply that one currency is undervalued or overvalued:

```py
from urllib.request import urlopen
import json
import time
import pandas as pd

def get_bigmac_codes():
    """Get a pandas DataFrame of all codes in the Big Mac index dataset

    The first column contains the code, while the second header
    contains the description of the code.

    E.g. 
    ECONOMIST/BIGMAC_ARG,Big Mac Index - Argentina
    ECONOMIST/BIGMAC_AUS,Big Mac Index - Australia
    ECONOMIST/BIGMAC_BRA,Big Mac Index - Brazil

    Returns:
        codes: pandas DataFrame of Quandl dataset codes"""

    codes_url = "https://www.quandl.com/api/v3/databases/ECONOMIST/codes"
    codes = pd.read_csv(codes_url, header=None, names=['Code', 'Description'], 
                        compression='zip', encoding='latin_1')

    return codes

def get_quandl_dataset(api_key, code):
    """Obtain and parse a quandl dataset in pandas DataFrame format

    Quandl returns dataset in JSON format, where data is stored as a 
    list of lists in response['dataset']['data'], and column headers
    stored in response['dataset']['column_names'].

    E.g. {'dataset': {...,
             'column_names': ['Date',
                              'local_price',
                              'dollar_ex',
                              'dollar_price',
                              'dollar_ppp',
                              'dollar_valuation',
                              'dollar_adj_valuation',
                              'euro_adj_valuation',
                              'sterling_adj_valuation',
                              'yen_adj_valuation',
                              'yuan_adj_valuation'],
             'data': [['2017-01-31',
                       55.0,
                       15.8575,
                       3.4683903515687,
                       10.869565217391,
                       -31.454736135007,
                       6.2671477203176,
                       8.2697553162259,
                       29.626894343348,
                       32.714616745128,
                       13.625825886047],
                      ['2016-07-31',
                       50.0,
                       14.935,
                       3.3478406427854,
                       9.9206349206349,
                       -33.574590420925,
                       2.0726096168216,
                       0.40224795003514,
                       17.56448458418,
                       19.76377270142,
                       11.643103380531]
                      ],
             'database_code': 'ECONOMIST',
             'dataset_code': 'BIGMAC_ARG',
             ... }}

    A custom column--country is added to denote the 3-letter country code.

    Args:
        api_key: Quandl API key
        code: Quandl dataset code

    Returns:
        df: pandas DataFrame of a Quandl dataset

    """
    base_url = "https://www.quandl.com/api/v3/datasets/"
    url_suffix = ".json?api_key="

    # Fetch the JSON response 
    u = urlopen(base_url + code + url_suffix + api_key)
    response = json.loads(u.read().decode('utf-8'))

    # Format the response as pandas Dataframe
    df = pd.DataFrame(response['dataset']['data'], columns=response['dataset']['column_names'])

    # Label the country code
    df['country'] = code[-3:]

    return df

quandl_dfs = []
codes = get_bigmac_codes()

# Replace this with your own API key
api_key = "INSERT-YOUR-KEY-HERE" 

for code in codes.Code:
    # Get the DataFrame of a Quandl dataset
    df = get_quandl_dataset(api_key, code)

    # Store in a list
    quandl_dfs.append(df)

    # Prevents exceeding the API speed limit
    time.sleep(2)

# Concatenate the list of data frames into a single one 
bigmac_df = pd.concat(quandl_dfs)
bigmac_df.head()
```

Here comes the expected result, which shows the first five rows of the data frame:

|   | **0** | **1** | **2** | **3** | **4** |
| **Date** | 31-07-17 | 31-01-17 | 31-07-16 | 31-01-16 | 31-07-15 |
| **local_price** | 5.9 | 5.8 | 5.75 | 5.3 | 5.3 |
| **dollar_ex** | 1.303016 | 1.356668 | 1.335738 | 1.415729 | 1.35126 |
| **dollar_price** | 4.527955 | 4.27518 | 4.304737 | 3.743655 | 3.922265 |
| **dollar_ppp** | 1.113208 | 1.146245 | 1.140873 | 1.075051 | 1.106472 |
| **dollar_valuation** | -14.56689 | -15.510277 | -14.588542 | -24.06379 | -18.115553 |
| **dollar_adj_valuation** | -11.7012 | -11.9234 | -11.0236 | -28.1641 | -22.1691 |
| **euro_adj_valuation** | -13.0262 | -10.2636 | -12.4796 | -22.2864 | -18.573 |
| **sterling_adj_valuation** | 2.58422 | 7.43771 | 2.48065 | -22.293 | -23.1926 |
| **yen_adj_valuation** | 19.9417 | 9.99688 | 4.39776 | -4.0042 | 6.93893 |
| **yuan_adj_valuation** | -2.35772 | -5.82434 | -2.681 | -20.6755 | -14.1711 |
| **country** | AUS | AUS | AUS | AUS | AUS |

The code for parsing JSON from Quandl API is a bit complicated, and thus requires extra explanation. The first function, `get_bigmac_codes()`, parses the list of all available dataset codes in the Quandl Economist database as a pandas DataFrame. Meanwhile, the second function, `get_quandl_dataset(api_key, code)`, converts the JSON response of a Quandl dataset API query to a pandas DataFrame. All datasets obtained are concatenated into a single data frame using `pandas.concat()`.

We should bear in mind that the Big Mac index is not directly comparable between countries. Normally, we would expect commodities in poor countries to be cheaper than those in rich ones. To represent a fairer picture of the index, it would be better to show the relationship between Big Mac pricing and **gross domestic product** (**GDP**) per capita.

To that end, we are going to acquire the GDP dataset from Quandl's **World Bank World Development Indicators** (**WWDI**) database. Based on the previous code example of acquiring JSON data from Quandl, can you try to adapt it to download the GDP per capita dataset?

For those who are impatient, here is the full code:

```py
import urllib
import json
import pandas as pd
import time
from urllib.request import urlopen

def get_gdp_dataset(api_key, country_code):
    """Obtain and parse a quandl GDP dataset in pandas DataFrame format
    Quandl returns dataset in JSON format, where data is stored as a 
    list of lists in response['dataset']['data'], and column headers
    stored in response['dataset']['column_names'].

    Args:
        api_key: Quandl API key
        country_code: Three letter code to represent country

    Returns:
        df: pandas DataFrame of a Quandl dataset
    """
    base_url = "https://www.quandl.com/api/v3/datasets/"
    url_suffix = ".json?api_key="

    # Compose the Quandl API dataset code to get GDP per capita (constant 2000 US$) dataset
    gdp_code = "WWDI/" + country_code + "_NY_GDP_PCAP_KD"

   # Parse the JSON response from Quandl API
   # Some countries might be missing, so we need error handling code
   try:
       u = urlopen(base_url + gdp_code + url_suffix + api_key)
   except urllib.error.URLError as e:
       print(gdp_code,e)
       return None

   response = json.loads(u.read().decode('utf-8'))

   # Format the response as pandas Dataframe
   df = pd.DataFrame(response['dataset']['data'], columns=response['dataset']['column_names'])

   # Add a new country code column
   df['country'] = country_code

   return df

api_key = "INSERT-YOUR-KEY-HERE" #Change this to your own API key

quandl_dfs = []

# Loop through all unique country code values in the BigMac index DataFrame
for country_code in bigmac_df.country.unique():
    # Fetch the GDP dataset for the corresponding country 
    df = get_gdp_dataset(api_key, country_code)

    # Skip if the response is empty
    if df is None:
        continue

    # Store in a list DataFrames
    quandl_dfs.append(df)

    # Prevents exceeding the API speed limit
    time.sleep(2)

# Concatenate the list of DataFrames into a single one 
gdp_df = pd.concat(quandl_dfs)
gdp_df.head()
```

The GDP data of several geographical regions is missing, but this should be handled gracefully by the `try...except` code block in the `get_gdp_dataset` function. This is what you are expecting to see after running the preceding code:

```py
WWDI/EUR_NY_GDP_PCAP_KD HTTP Error 404: Not Found 
WWDI/ROC_NY_GDP_PCAP_KD HTTP Error 404: Not Found 
WWDI/SIN_NY_GDP_PCAP_KD HTTP Error 404: Not Found 
WWDI/UAE_NY_GDP_PCAP_KD HTTP Error 404: Not Found
```

|  | **Date** | **Value** | **country** |
| **0** | 2016-12-31 | 55478.577294 | AUS |
| **1** | 2015-12-31 | 54800.366396 | AUS |
| **2** | 2014-12-31 | 54293.794205 | AUS |
| **3** | 2013-12-31 | 53732.003969 | AUS |
| **4** | 2012-12-31 | 53315.029915 | AUS |

Next, we will merge the two pandas DataFrames that contain Big Mac Index or GDP per capita using `pandas.merge()`. The most recent record in WWDI's GDP per capita dataset was collected at the end of 2016, so let's pair that up with the closest Big Mac index dataset in January 2017.

For those who are familiar with the SQL language, `pandas.merge()` supports four modes, namely left, right, inner, and outer joins. Since we are interested in rows that have matching countries in both pandas DataFrames only, we are going to choose inner join:

```py
merged_df = pd.merge(bigmac_df[(bigmac_df.Date == "2017-01-31")], gdp_df[(gdp_df.Date == "2016-12-31")], how='inner', on='country')
merged_df.head()
```

Here is the merged data frame:

|   | **0** | **1** | **2** | **3** | **4** |
| **Date_x** | 31-01-17 | 31-01-17 | 31-01-17 | 31-01-17 | 31-01-17 |
| **local_price** | 5.8 | 16.5 | 3.09 | 2450 | 55 |
| **dollar_ex** | 1.356668 | 3.22395 | 0.828775 | 672.805 | 15.8575 |
| **dollar_price** | 4.27518 | 5.117945 | 3.728394 | 3.641471 | 3.46839 |
| **dollar_ppp** | 1.146245 | 3.26087 | 0.610672 | 484.189723 | 10.869565 |
| **dollar_valuation** | -15.510277 | 1.145166 | -26.316324 | -28.034167 | -31.454736 |
| **dollar_adj_valuation** | -11.9234 | 67.5509 | -18.0208 | 11.9319 | 6.26715 |
| **euro_adj_valuation** | -10.2636 | 70.7084 | -16.4759 | 14.0413 | 8.26976 |
| **sterling_adj_valuation** | 7.43771 | 104.382 | 0 | 36.5369 | 29.6269 |
| **yen_adj_valuation** | 9.99688 | 109.251 | 2.38201 | 39.7892 | 32.7146 |
| **yuan_adj_valuation** | -5.82434 | 79.1533 | -12.3439 | 19.6828 | 13.6258 |
| **Country** | AUS | BRA | GBR | CHL | ARG |
| **Date_y** | 31-12-16 | 31-12-16 | 31-12-16 | 31-12-16 | 31-12-16 |
| **Value** | 55478.5773 | 10826.2714 | 41981.3921 | 15019.633 | 10153.99791 |

# Using Seaborn to simplify visualization tasks

The scatter plot is one of the most common plots in the scientific and business worlds. It is particularly useful for displaying the relationship between two variables. While we can simply use `matplotlib.pyplot.scatter` to draw a scatterplot (see [Chapter 2](e1962b92-9e72-4c5a-bdd8-11b7ce29411b.xhtml), *Getting Started with Matplotlib*, and [Chapter 4](456b0dc2-84d5-40f9-bf63-1ab4635cbac8.xhtml), *Advanced Matplotlib*, for more details), we can also use Seaborn to build similar plots with more advanced features.

The two functions, `seaborn.regplot()` and `seaborn.lmplot()`, display a linear relationship in the form of a scatter plot, a regression line, and the 95% confidence interval around the regression line. The main difference between the two functions is that `lmplot()` combines `regplot()` with `FacetGrid`, such that we can create color-coded or faceted scatter plots to show the interaction between three or more pairs of variables. 

The simplest form of `seaborn.regplot()` supports NumPy arrays, pandas Series, or pandas DataFrames as input. The regression line and the confidence interval can be removed by specifying `fit_reg=False`.

We are going to investigate the hypothesis that Big Macs are cheaper in countries with lower GDP, and vice versa. To that end, we will try to find out whether there is any correlation between the Big Mac index and GDP per capita:

```py
import seaborn as sns
import matplotlib.pyplot as plt

# seaborn.regplot() returns a matplotlib.Axes object
ax = sns.regplot(x="Value", y="dollar_price", data=merged_df, fit_reg=False)
# We can modify the axes labels just like other ordinary
# Matplotlib objects
ax.set_xlabel("GDP per capita (constant 2000 US$)")
ax.set_ylabel("BigMac index (US$)")
plt.show()
```

The code will greet you with a good old scatter plot:

![](img/91b63fe9-f7d5-4405-a7f0-c8aaa756ba70.png)

So far so good! It looks like the Big Mac index is positively correlated with GDP per capita. Let's turn the regression line back on and label a few countries that show extreme Big Mac index values (that is, ≥ 5 or ≤ 2). Meanwhile, the default plotting style is a bit plain; we can spice up the graph by running `sns.set(style="whitegrid")`. There are four other styles to choose from, namely `darkgrid`, `dark`, `white`, and `ticks`:

```py
sns.set(style="whitegrid")
ax = sns.regplot(x="Value", y="dollar_price", data=merged_df)
ax.set_xlabel("GDP per capita (constant 2000 US$)")
ax.set_ylabel("BigMac index (US$)")
# Label the country codes which demonstrate extreme BigMac index
for row in merged_df.itertuples():
    if row.dollar_price >= 5 or row.dollar_price <= 2:
     ax.text(row.Value,row.dollar_price+0.1,row.country)
plt.show()
```

Here is the labeled plot:

![](img/99ebb967-73f2-42b8-8dde-6be3c98d6679.png)

We can see that many countries fall within the confidence interval of the regression line. Given the GDP per capita level for each country, the linear regression model predicts the corresponding Big Mac index. The currency value shows signs of undervaluation or overvaluation if the actual index deviates from the regression model.

By labeling the countries that show extremely high or low values, we can clearly see that the average price of a Big Mac in Brazil and Switzerland were overvalued, while it is undervalued in South Africa, Malaysia, Ukraine, and Egypt.

Since Seaborn is not a package for statistical analysis, we need to use other packages, such as `scipy.stats` or `statsmodels`, to obtain the parameters of a regression model. In the next example, we are going to get the slope and intercept parameters from the regression model, and apply different colors for points that are above or below the regression line:

```py
from scipy.stats import linregress

ax = sns.regplot(x="Value", y="dollar_price", data=merged_df)
ax.set_xlabel("GDP per capita (constant 2000 US$)")
ax.set_ylabel("BigMac index (US$)")

# Calculate linear regression parameters
slope, intercept, r_value, p_value, std_err = linregress(merged_df.Value, merged_df.dollar_price)

colors = []
for row in merged_df.itertuples():
    if row.dollar_price > row.Value * slope + intercept:
        # Color markers as darkred if they are above the regression line
        color = "darkred"
    else:
        # Color markers as darkblue if they are below the regression line
        color = "darkblue"

    # Label the country code for those who demonstrate extreme BigMac index
    if row.dollar_price >= 5 or row.dollar_price <= 2:
        ax.text(row.Value,row.dollar_price+0.1,row.country)

    # Highlight the marker that corresponds to China
    if row.country == "CHN":
        t = ax.text(row.Value,row.dollar_price+0.1,row.country)
        color = "yellow"

    colors.append(color)

# Overlay another scatter plot on top with marker-specific color
ax.scatter(merged_df.Value, merged_df.dollar_price, c=colors)

# Label the r squared value and p value of the linear regression model.
# transform=ax.transAxes indicates that the coordinates are given relative to the axes bounding box, 
# with 0,0 being the lower left of the axes and 1,1 the upper right.
ax.text(0.1, 0.9, "$r^2={0:.3f}, p={1:.3e}$".format(r_value ** 2, p_value), transform=ax.transAxes)

plt.show()
```

This screenshot shows the color-labeled plot:

![](img/d0f89ab5-681b-48b2-b00d-45410b8b3c33.png)

Contrary to popular belief, it looks as if China's currency was not significantly undervalued in 2016, as the value lies within the 95% confidence interval of the regression line.

We can also combine histograms of *x* and *y* values with a scatter plot using `seaborn.jointplot`:

By additionally specifying the `kind` parameter in `jointplot` to any one of `reg`, `resid`, `hex`, or `kde`, we can quickly change the plot type to regression, residual, hex bin, or KDE contour plot, respectively.

```py
# seaborn.jointplot() returns a seaborn.JointGrid object
g = sns.jointplot(x="Value", y="dollar_price", data=merged_df)

# Provide custom axes labels through accessing the underlying axes object
# We can get matplotlib.axes.Axes of the scatter plot by calling g.ax_joint
g.ax_joint.set_xlabel("GDP per capita (constant 2000 US$)")
g.ax_joint.set_ylabel("BigMac index (US$)")

# Set the title and adjust the margin
g.fig.suptitle("Relationship between GDP per capita and BigMac Index")
g.fig.subplots_adjust(top=0.9)
plt.show()
```

The `jointplot` is as follows:

![](img/898e91c6-42bc-4ded-b51c-df3a52622124.png)

Here is the big disclaimer. With all the data in our hands, it is still too early to make any conclusion about the valuation of currencies! Different business factors such as labor cost, rent, raw material costs, and taxation can all contribute to the pricing model of Big Mac, but this is beyond the scope of this book.

# Scraping information from websites

Governments or jurisdictions around the world are increasingly embracing the importance of open data, which aims to increase citizen involvement and informed decision-making, and also aims to make policies more open to public scrutiny. Some examples of open data initiatives around the world include [https://www.data.gov/](https://www.data.gov/) (United States of America), [https://data.gov.uk/](https://data.gov.uk/) (United Kingdom), and [https://data.gov.hk/en/](https://data.gov.hk/en/) (Hong Kong).

These data portals often provide an API for programmatic access of data. However, an API is not available for some datasets, hence we need to rely on good old web scraping techniques to extract information from websites.

Beautiful Soup ([https://www.crummy.com/software/BeautifulSoup/](https://www.crummy.com/software/BeautifulSoup/)) is an incredibly useful package for scraping information from websites. Basically, everything marked with an HTML tag can be scraped with this wonderful package. Scrapy is also a good package for web scraping, but it is more like a framework for writing powerful web crawlers. So if you just need to fetch a table from the page, Beautiful Soup offers simpler procedures.

We are going to use Beautiful Soup version 4.6 throughout this chapter. To install Beautiful Soup 4, we can once again rely on PyPI:

```py
pip install beautifulsoup4
```

The US unemployment rates and earnings by educational attainment data (2017) is available from the following website: [https://www.bls.gov/emp/ep_table_001.htm](https://www.bls.gov/emp/ep_table_001.htm). Currently, Beautiful Soup does not handle HTML requests. So we need to use the `urllib.request` or `requests` package to fetch a web page for us. Among the two options, the `requests` package is arguably easier to use due to its higher-level HTTP client interface. If `requests` is not available on your system, we can install that through PyPI:

```py
pip install requests
```

Let's take a look at the web page before we write the web scraping code. If we use Google Chrome to visit the Bureau of Labor Statistics website, we can inspect the HTML code corresponding to the table we need:

![](img/c8742c4c-8452-4001-919c-d192cb90aa89.png)

Next, expand `<div id="bodytext" class="verdana md">` until you can see `<table class="regular" cellspacing="0" cellpadding="0" xborder="1">...</table>`. When you put your mouse over the HTML code, the corresponding section on the page will be highlighted:

![](img/0b9378c6-61e9-4ff1-ba1f-41ec509516b0.png)

After further expanding the HTML code of the `<table>`, we can see that the column names are defined in the `<thead>...</thead>` section, while the table content is defined in the `<tbody>...</tbody>` section.

In order to instruct Beautiful Soup to scrape the information we need, we need to give clear directions to it. We can right-click on the relevant section in the code inspection window and copy the unique identifier in the format of a CSS selector:

![](img/6979cfd1-bddb-49c7-a9f3-945f6f9d5264.png)

Let's try to get the CSS selectors for `thead` and `tbody`, and use the `BeautifulSoup.select()` method to scrape the respective HTML code:

```py
import requests
from bs4 import BeautifulSoup

# Specify the url
url = "https://www.bls.gov/emp/ep_table_001.htm"

# Query the website and get the html response
response = requests.get(url)

# Parse the returned html using BeautifulSoup
bs = BeautifulSoup(response.text)

# Select the table header by CSS selector
thead = bs.select("#bodytext > table > thead")[0]

# Select the table body by CSS selector
tbody = bs.select("#bodytext > table > tbody")[0]

# Make sure the code works
print(thead)
```

You will be greeted by the HTML code of the table headers:

```py
<thead> 
<tr> 
<th scope="col"><p align="center" valign="top"><strong>Educational attainment</strong></p></th> 
<th scope="col"><p align="center" valign="top">Unemployment rate (%)</p></th> 
<th scope="col"><p align="center" valign="top">Median usual weekly earnings ($)</p></th> 
</tr> 
</thead>
```

Next, we are going to find all instances of `<th></th>`, which contains the name of each column. We will build a dictionary of lists with headers as keys to hold the data:

```py
# Get the column names
headers = []

# Find all header columns in <thead> as specified by <th> html tags
for col in thead.find_all('th'):
   headers.append(col.text.strip())

# Dictionary of lists for storing parsed data
data = {header:[] for header in headers}
```

Finally, we parse the remaining rows of the table and convert the data to a pandas DataFrame:

```py
import pandas as pd

# Parse the rows in table body
for row in tbody.find_all('tr'):
    # Find all columns in a row as specified by <th> or <td> html tags
    cols = row.find_all(['th','td'])

    # enumerate() allows us to loop over an iterable, 
    # and return each item preceded by a counter
    for i, col in enumerate(cols):
        # Strip white space around the text
        value = col.text.strip()

        # Try to convert the columns to float, except the first column
        if i > 0:
            value = float(value.replace(',','')) # Remove all commas in string

        # Append the float number to the dict of lists
        data[headers[i]].append(value)

# Create a data frame from the parsed dictionary
df = pd.DataFrame(data)

# Show an excerpt of parsed data
df.head()
```

We should now be able to reproduce the first few rows of the main table:

|  | **Educational attainment** | **Median usual weekly earnings ($)** | **Unemployment rate (%)** |
| **0** | Doctoral degree | 1743.0 | 1.5 |
| **1** | Professional degree | 1836.0 | 1.5 |
| **2** | Master's degree | 1401.0 | 2.2 |
| **3** | Bachelor's degree | 1173.0 | 2.5 |
| **4** | Associate's degree | 836.0 | 3.4 |

The main HTML table has been formatted as a structured pandas DataFrame. We can now proceed to visualize the data.

# Matplotlib graphical backends

The code for plotting graphs is considered the frontend in Matplotlib's terminology. We first mentioned backends in [Chapter 1](840a0403-49de-4053-adf9-bf389ed96cf5.xhtml), *Introduction to Matplotlib*, when we were talking about output formats. In reality, Matplotlib backends have much more differences than just support for graphical formats. Backends handle so many things behind the scenes! And that would determine the support of plotting capabilities. For example, the LaTeX text layout is supported only by Agg, PDF, PGF, and PS backends.

# Non-interactive backends

We have been using several non-interactive backends so far, which include Agg, Cairo, GDK, PDF, PGF, PS, and SVG. Most of these backends work without extra dependencies, but Cairo and GDK require the Cairo graphics library or GIMP Drawing Kit, respectively, to work.

Non-interactive backends can be further classified into two groups—vector or raster. Vector graphics describe images in terms of points, paths, and shapes that are calculated using mathematical formulas. A vector graphic will always appear smooth irrespective of the scale, and its size is usually much smaller than the raster counterpart. PDF, PGF, PS, and SVG backends belong to the vector group.

Raster graphics describe images in terms of a finite number of tiny color blocks (pixels). So if we zoom in enough, we start to see an *unsmooth* representation of the image, in other words, pixelation. By increasing the resolution or **dots per inch** (**DPI**) of the image, we are less likely to observe pixelation. Agg, Cairo, and GDK belong to this group of backends. The following table summarizes the key functionalities and differences among the non-interactive backends:

| Backend | Vector or raster? | Output formats |
| Agg | Raster | `.png` |
| Cairo | Vector/Raster | `.pdf`, `.png`, `.ps`, `.svg` |
| PDF | Vector | `.pdf` |
| PGF | Vector | `.pdf`, `.pgf` |
| PS | Vector | `.ps` |
| SVG | Vector | `.svg` |
| GDK* | Raster | `.png`, `.jpg`, `.tiff` |

*Deprecated in Matplotlib 2.0.

Normally, we don't need to manually select a backend, as the default choice would work great for most tasks. On the other hand, we can specify a backend through the `matplotlib.use()` method before importing `matplotlib.pyplot` for the **first** time:

```py
import matplotlib
matplotlib.use('SVG') # Change to SVG backend
import matplotlib.pyplot as plt
import textwrap # Standard library for text wrapping

# Create a figure
fig, ax = plt.subplots(figsize=(6,7))

# Create a list of x ticks positions
ind = range(df.shape[0])

# Plot a bar chart of median usual weekly earnings by educational attainments
rects = ax.barh(ind, df["Median usual weekly earnings ($)"], height=0.5)

# Set the x-axis label
ax.set_xlabel('Median weekly earnings (USD)')

# Label the x ticks
# The tick labels are a bit too long, let's wrap them in 15-char lines
ylabels=[textwrap.fill(label,15) for label in df["Educational attainment"]]
ax.set_yticks(ind)
ax.set_yticklabels(ylabels)

# Give extra margin at the bottom to display the tick labels
fig.subplots_adjust(left=0.3)

# Save the figure in SVG format
plt.savefig("test.svg")
```

![](img/09f26012-7935-4384-adc2-bcaf167d749f.png)

# Interactive backends

Matplotlib can build interactive figures that are far more engaging for readers. Sometimes, a plot might be overwhelmed with graphical elements, making it hard to discern individual data points. On other occasions, some data points may appear so similar, in which it could be hard to spot the differences with our naked eyes. An interactive plot can address these two scenarios by allowing us to zoom in, zoom out, pan, and explore the plot in the way we want.

Through the use of interactive backends, plots in Matplotlib can be embedded in **graphical user interfaces** (**GUI**) applications. By default, Matplotlib supports the pairing of the Agg raster graphics renderer with a wide variety of GUI toolkits, including wxWidgets (Wx), GIMP Toolkit (GTK+), Qt and TkInter (Tk). As Tkinter is the de facto standard GUI for Python, which is built on top of Tcl/Tk, we can create an interactive plot with nothing more than calling `plt.show()` in a standalone Python script. Let's try to copy the following code to a separate text file and name it `interactive.py`. After that, type `python interactive.py` in your Terminal (Mac/Linux) or Command Prompt (Windows). If you are unsure about how to open a Terminal or Command Prompt, please refer to [Chapter 1](840a0403-49de-4053-adf9-bf389ed96cf5.xhtml), *Introduction to Matplotlib*, for more details:

```py
import matplotlib
import matplotlib.pyplot as plt
import textwrap
import requests
import pandas as pd
from bs4 import BeautifulSoup
# Import Matplotlib radio button widget
from matplotlib.widgets import RadioButtons

url = "https://www.bls.gov/emp/ep_table_001.htm"
response = requests.get(url)
bs = BeautifulSoup(response.text)
thead = bs.select("#bodytext > table > thead")[0]
tbody = bs.select("#bodytext > table > tbody")[0]

headers = []
for col in thead.find_all('th'):
    headers.append(col.text.strip())

data = {header:[] for header in headers}
for row in tbody.find_all('tr'):
    cols = row.find_all(['th','td'])

    for i, col in enumerate(cols):
        value = col.text.strip()
        if i > 0:
            value = float(value.replace(',','')) 
        data[headers[i]].append(value)

df = pd.DataFrame(data)

fig, ax = plt.subplots(figsize=(6,7))
ind = range(df.shape[0])
rects = ax.barh(ind, df["Median usual weekly earnings ($)"], height=0.5)
ax.set_xlabel('Median weekly earnings (USD)')
ylabels=[textwrap.fill(label,15) for label in df["Educational attainment"]]
ax.set_yticks(ind)
ax.set_yticklabels(ylabels)
fig.subplots_adjust(left=0.3)

# Create axes for holding the radio selectors.
# supply [left, bottom, width, height] in normalized (0, 1) units
bax = plt.axes([0.3, 0.9, 0.4, 0.1])
radio = RadioButtons(bax, ('Weekly earnings', 'Unemployment rate'))

# Define the function for updating the displayed values
# when the radio button is clicked
def radiofunc(label):
  # Select columns from dataframe depending on label
  if label == 'Weekly earnings':
    data = df["Median usual weekly earnings ($)"]
    ax.set_xlabel('Median weekly earnings (USD)')
  elif label == 'Unemployment rate':
    data = df["Unemployment rate (%)"]
    ax.set_xlabel('Unemployment rate (%)')

  # Update the bar heights
  for i, rect in enumerate(rects):
    rect.set_width(data[i])

  # Rescale the x-axis range
  ax.set_xlim(xmin=0, xmax=data.max()*1.1)

  # Redraw the figure
  plt.draw()
radio.on_clicked(radiofunc)

plt.show()
```

We shall see a pop-up window similar to the following one. We can pan, zoom to selection, configure subplot margins, save, and go back and forth between different views by clicking on the buttons on the bottom toolbar. If we put our mouse over the plot, we can also observe the exact coordinates in the lower-right corner. This feature is extremely useful for dissecting data points that are close to each other:

![](img/6e180f63-ddca-4a58-ada0-43ed0d124acc.png)

Next, we are going to extend the application by adding a radio button widget on top of the figure, such that we can switch between the display of weekly earnings or unemployment rates. The radio button can be found in `matplotlib.widgets`, and we are going to attach a data updating function to the `.on_clicked()` event of the button. You can paste the following code right before the `plt.show()` line to the previous code example (`interactive.py`). Let's see how it works:

```py
# Import Matplotlib radio button widget
from matplotlib.widgets import RadioButtons

# Create axes for holding the radio selectors.
# supply [left, bottom, width, height] in normalized (0, 1) units
bax = plt.axes([0.3, 0.9, 0.4, 0.1])
radio = RadioButtons(bax, ('Weekly earnings', 'Unemployment rate'))

# Define the function for updating the displayed values
# when the radio button is clicked
def radiofunc(label):
    # Select columns from dataframe, and change axis label depending on selection
    if label == 'Weekly earnings':
        data = df["Median usual weekly earnings ($)"]
        ax.set_xlabel('Median weekly earnings (USD)')
    elif label == 'Unemployment rate':
        data = df["Unemployment rate (%)"]
        ax.set_xlabel('Unemployment rate (%)')

    # Update the bar heights
    for i, rect in enumerate(rects):
        rect.set_width(data[i])

    # Rescale the x-axis range
    ax.set_xlim(xmin=0, xmax=data.max()*1.1)

    # Redraw the figure
    plt.draw()

# Attach radiofunc to the on_clicked event of the radio button
radio.on_clicked(radiofunc)
```

You will be welcomed by a new radio selector box on top of the plot. Try switching between the two states and see if the figure would be updated accordingly. The complete code is also available in the code bundle:

![](img/57ac5081-9361-4cc7-aa1b-8cea80ae2003.png)

Before we conclude this section, we are going to introduce one more interactive backend that is rarely covered by books. Starting with Matplotlib 1.4, there is an interactive backend specifically designed for Jupyter Notebook. To invoke that, we simply need to paste `%matplotlib notebook` at the start of your notebook. We are going to adapt one of the earlier examples in this chapter to use this backend:

```py
# Import the interactive backend for Jupyter Notebook
%matplotlib notebook
import matplotlib
import matplotlib.pyplot as plt
import textwrap

fig, ax = plt.subplots(figsize=(6,7))
ind = range(df.shape[0])
rects = ax.barh(ind, df["Median usual weekly earnings ($)"], height=0.5)
ax.set_xlabel('Median weekly earnings (USD)')
ylabels=[textwrap.fill(label,15) for label in df["Educational attainment"]]
ax.set_yticks(ind)
ax.set_yticklabels(ylabels)
fig.subplots_adjust(left=0.3)

# Show the figure using interactive notebook backend
plt.show()
```

The following interactive plot will be embedded right into your Jupyter Notebook:

![](img/e06418fb-48b0-4652-943c-7e2b81d29a75.png)

# Creating animated plot

Matplotlib was not designed as an animation package from the get-go, and thus it would appear sluggish in some advanced usages. For animation-centric applications, PyGame is a very good alternative ([https://www.pygame.org](https://www.pygame.org)) which supports OpenGL- and Direct3D-accelerated graphics for the ultimate speed in animating objects. Nevertheless, Matplotlib has acceptable performance most of the time, and we will guide you through the steps to create animations that are more engaging than static plots.

Before we start making animations, we need to install either FFmpeg, avconv, mencoder, or ImageMagick on our system. These additional dependencies are not bundled with Matplotlib, and thus we need to install them separately. We are going to walk you through the steps of installing FFmpeg.

For Debian-based Linux users, FFmpeg can be installed by simply issuing the following command in Terminal.

```py
sudo apt-get install ffmpeg
```

For Mac users, Homebrew ([https://brew.sh/](https://brew.sh/)) is the simplest way to search and install `ffmpeg` package. If you don't have Homebrew, you can paste the following code into your Terminal to install it.

```py
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

After that, we can install FFmpeg by issuing the following command in the Terminal:

```py
brew install ffmpeg
```

Alternatively, you may install FFmpeg by copying the binaries ([https://evermeet.cx/ffmpeg/](https://evermeet.cx/ffmpeg/)) to the system path (for example, `/usr/local/bin`).

The installation for Windows users is quite a bit more involved, but luckily there is a detailed guide on wikiHow ([https://www.wikihow.com/Install-FFmpeg-on-Windows](https://www.wikihow.com/Install-FFmpeg-on-Windows)).

Matplotlib provides two main interfaces for creating animations: `TimedAnimation` and `FuncAnimation`. `TimedAnimation` is useful for creating time-based animation, while `FuncAnimation` can be used to create animation according to a custom-defined function. Given by the much higher level of flexibility offered by `FuncAnimation`, we will only explore the use of `FuncAnimation` in this section. Interested readers can refer to the official documentation ([https://matplotlib.org/api/animation_api.html](https://matplotlib.org/api/animation_api.html)) for more information about `TimedAnimation`.

In the following example, we simulated the change in median weekly earnings by assuming a 5% annual increase. We are going to create a custom function—animate, which returns Matplotlib `Artist` objects that are changed in each frame. This function will be supplied to `animation.FuncAnimation()` together with a few more extra parameters:

```py
import textwrap 
import matplotlib.pyplot as plt
import random
# Matplotlib animation module
from matplotlib import animation
# Used for generating HTML video embed code
from IPython.display import HTML

# Adapted from previous example, codes that are modified are commented
fig, ax = plt.subplots(figsize=(6,7))
ind = range(df.shape[0])
rects = ax.barh(ind, df["Median usual weekly earnings ($)"], height=0.5)
ax.set_xlabel('Median weekly earnings (USD)')
ylabels=[textwrap.fill(label,15) for label in df["Educational attainment"]]
ax.set_yticks(ind)
ax.set_yticklabels(ylabels)
fig.subplots_adjust(left=0.3)

# Change the x-axis range
ax.set_xlim(0,7600)

# Add a text annotation to show the current year
title = ax.text(0.5,1.05, "Median weekly earnings (USD) in 2017", 
 bbox={'facecolor':'w', 'alpha':0.5, 'pad':5},
 transform=ax.transAxes, ha="center")

# Animation related stuff
n=30 #Number of frames

def animate(frame):
    # Simulate 5% annual pay rise 
    data = df["Median usual weekly earnings ($)"] * (1.05 ** frame)

    # Update the bar heights
    for i, rect in enumerate(rects):
        rect.set_width(data[i])

    # Update the title
    title.set_text("Median weekly earnings (USD) in {}".format(2016+frame))

    return rects, title

# Call the animator. Re-draw only the changed parts when blit=True. 
# Redraw all elements when blit=False
anim=animation.FuncAnimation(fig, animate, blit=False, frames=n)

# Save the animation in MPEG-4 format
anim.save('test.mp4')

# OR--Embed the video in Jupyter Notebook
HTML(anim.to_html5_video())
```

Here is the resultant video:

[https://github.com/PacktPublishing/Matplotlib-for-Python-Developers-Second-Edition/blob/master/extra_ch9/ch09_animation.mp4](https://github.com/PacktPublishing/Matplotlib-for-Python-Developers-Second-Edition/blob/master/extra_ch9/ch09_animation.mp4)

In the preceding example, we output animation in the form of MPEG-4 encoded videos. The video can also be embedded in Jupyter Notebook in the form of H.264 encoded video. All you need to do is to call the `Animation.to_html5_video()` method, and supply the returned object to `IPython.display.HTML`. Video encoding and HTML5 code generation will happen automagically behind the scenes.

Starting from version 2.2.0, Matplotlib supports the creation of animated GIF writing via the Pillow imaging library and ImageMagick. As the WWW is never tired of GIFs, let's learn how to create one!

Before we are able to create animated GIFs, we need to install ImageMagick first. The download links and the installation instructions for all major platforms can be found here: [https://www.imagemagick.org/script/download.php](https://www.imagemagick.org/script/download.php).

Once the package is installed, we can generate animated GIFs by changing the line `anim.save('test.mp4')` to `anim.save('test.gif', writer='imagemagick', fps=10)`. The `fps` parameter denotes the frame rate of the animation.

Here is the resultant animated GIF:

[https://github.com/PacktPublishing/Matplotlib-for-Python-Developers-Second-Edition/blob/master/extra_ch9/ch%2009_GIF.gif](https://github.com/PacktPublishing/Matplotlib-for-Python-Developers-Second-Edition/blob/master/extra_ch9/ch%2009_GIF.gif)

# Summary

In this chapter, you learned how to parse online data in CSV or JSON formats using the versatile pandas package. You further learned how to filter, subset, merge, and process data into insights. Finally, you learned how to scrape information directly from websites. You have now equipped yourself with the knowledge to visualize time series, univariate, and bivariate data. The chapter concluded with a number of useful techniques to customize figure aesthetics for effective storytelling.

Phew! We have just completed a long chapter, so go grab a burger, have a break, and relax.
