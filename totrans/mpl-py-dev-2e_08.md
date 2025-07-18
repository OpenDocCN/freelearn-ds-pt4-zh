
# Integrating Matplotlib with Web Applications

Web-based applications (web apps) offer multi-level advantages. First, users can enjoy a unified experience across platforms. Second, since an installation process is not required, users can enjoy a simpler workflow. Lastly, from the perspective of developers, the development cycle can be simplified as less platform-specific code has to be maintained. Given these advantages, more and more applications are being developed online.

Owing to the popularity and flexibility of Python, it makes sense for web developers to use Python-based web frameworks such as Django and Flask to develop web applications. In fact, Django and Flask ranked 6th and 13th out of 175 respectively in terms of popularity, according to [http://hotframeworks.com/](http://hotframeworks.com/). These frameworks are *batteries included*. From user authentication, user administration, and content management to API design, these frameworks have got them all covered. The code base is closely reviewed by the open source community such that sites that were developed using these frameworks are protected against common attacks, such as SQL injection, cross-site request forgery, and cross-site scripting.

In this chapter, we are going to learn how to develop a simple site that displays the price of Bitcoin. Examples based on Django  will be covered. We will use Docker 18.03.0-ce, and Django 2.0.4 for demonstration. Let's begin by going through the steps of initializing a Docker-based development environment. 

# Installing Docker

Docker allows developers to run applications in self-contained and lightweight containers. Since its introduction in 2013, Docker has quickly gained popularity among developers. At the center of its technology, Docker uses the resource isolation methods of the Linux kernel instead of a full-blown virtualization hypervisor to run applications.

This enables easier development, packaging, deployment, and management of code. Therefore, all code development work in this chapter will be conducted using a Docker-based environment.

# Docker for Windows users

There are two ways to install Docker on Windows: the aptly named Docker for Windows package, and Docker Toolbox. I recommend stable versions of the Docker Toolbox because Docker for Windows requires Hyper-V support in 64-bit Windows 10 Pro. Meanwhile, Docker for Windows is not supported by older versions of Windows. Detailed installation instructions can be found at [https://docs.docker.com/toolbox/toolbox_install_windows/](https://docs.docker.com/toolbox/toolbox_install_windows/), but we will also cover the important steps here.

First, download Docker Toolbox from the following link: [https://github.com/docker/toolbox/releases](https://github.com/docker/toolbox/releases). Choose the file with the name `DockerToolbox-xx.xx.x-ce.exe`, where `x` refers to the latest version numbers:

![](img/5b2481e4-15e3-4773-8857-70a68bb5f567.png)

Next, run the downloaded installer. Follow the default instructions of each prompt to install:

![](img/68bda216-8509-443e-983d-34dcc30d1d3e.png)

Windows might ask you for permission to make certain changes. It is normal, and make sure you allow the changes to happen.

Finally, once the installation is complete, you should be able to locate Docker Quickstart Terminal in the Start menu:

![](img/22c6c3b3-51f5-4351-a2ac-b3ba3a4e1e83.png)

Click on the icon to launch the Docker Toolbox Terminal, which begins an initialization process. When the process is complete, the following terminal will be shown:

![](img/fb2879b8-18b6-4986-91dd-9a2a111858ac.png)

# Docker for Mac users

For Mac users, I recommend the Docker CE for Mac (stable) app, which is available at [https://store.docker.com/editions/community/docker-ce-desktop-mac](https://store.docker.com/editions/community/docker-ce-desktop-mac). In addition, a full installation guide can be found via the following link: [https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install/).

The installation process of Docker CE for Mac is arguably simpler than for its Windows counterpart. Here are the major steps:

1.  First, double-click on the downloaded `Docker.dmg` file to mount the image. When you see the following popup, drag and drop the Docker icon on the left to the Applications folder on the right:

![](img/cc00110a-9fb4-43a6-b6f2-aa17b92743d9.png)

2.  Next, in your Applications folder or Launchpad, locate and double-click on the Docker app. You should be able to see a whale icon in the top status bar if Docker was successfully launched:

![](img/2d5fb604-2f21-4bef-9a35-b9551dc3eef0.png)

3.  Finally, open the Terminal app in the Applications | Utilities folder. Type `docker info`, followed by *Enter* to check whether Docker is installed properly:

![](img/e255ba06-af04-4976-ba33-1f5c5d7e4a12.png)

# More about Django

Django is a popular web framework that is designed to simplify the development and deployment of web applications. It includes loads of boilerplate code for everyday tasks, such as database model management, frontend templating, session authentication, and security. Django was built around the **Model-Template-View** (**MTV**) design pattern.

The model is perhaps the most critical component of MTV. It refers to how you represent your data in terms of different tables and attributes. It also abstracts away the nitty-gritty details of different database engines such that the same model can be applied to SQLite, MySQL, and PostgreSQL. Meanwhile, the model layer of Django would expose engine-specific parameters, such as `ArrayField` and `JSONField` in PostgreSQL, for fine-tuning of your data representation.

The template is similar to the role of a view in the canonical MTV framework. It handles the presentation of data to the users. In other words, it doesn't handle the logic of how the data was generated.

The view in Django is responsible for handling a user's request, and the subsequent logic of returning a response. It sits between the model layer and the template layer. A view determines what kind of data should be fetched from the model and how to process the data for the template.

The key selling points of Django are as follows:

*   **Development speed**: Loads of key components are provided; this reduces the number of repetitive tasks in a development cycle. For instance, it takes mere minutes to build a simple blog using Django.
*   **Security**: Best practices of web security are included in Django. The risks of hacks such as SQL injection, cross-site scripting, cross-site request forgery, and clickjacking are greatly reduced. Its user authentication system uses the PBKDF2 algorithm with a salted SHA256 hash, which is recommended by NIST. Other state-of-the-art hash algorithms, such as Argon2, are also available.
*   **Scalability**: The MTV layers of Django use a shared-nothing architecture. If a certain layer is the bottleneck of the web application, just throw more hardware to it; Django will take advantage of the additional hardware for each of the layers.

# Django development in Docker containers

To keep things tidy, let's create an empty directory named `Django` to host all the files. Inside the `Django` directory, we need to define the content of a container by creating a `Dockerfile` using our favorite text editor. A `Dockerfile` defines the base image of a container as well as the commands that are necessary to compile an image.

For more information about Dockerfile, please visit [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/).

We will use Python 3.6.5 as our base image. Please copy the following code to your Dockerfile. A series of additional commands define the working directory and the initiation process:

```py
# The official Python 3.6.5 runtime is used as the base image
FROM python:3.6.5-slim
# Disable buffering of output streams
ENV PYTHONUNBUFFERED 1
# Create a working directory within the container
RUN mkdir /app
WORKDIR /app
# Copy files and directories in the current directory to the container
ADD . /app/
# Install Django and other dependencies
RUN pip install -r requirements.txt
```

As you may notice, we also need a text file, `requirements.txt`, to define any package dependencies in our project. Please add the following content to the `requirements.txt` file in the folder where the project is present:

```py
Django==2.0.4
Matplotlib==2.2.2
stockstats==0.2.0
seaborn==0.8.1
```

Now, we can run `docker build -t django` in the terminal to build the image. It takes up to several minutes before the process is complete:

Please make sure you are currently located in the same project folder before running the command.

![](img/99b24f91-28c0-4404-b0d9-e9ed1982a87e.png)

The following message will be shown if the building process is complete. The exact hash code at the end of the `Successfully built ...` message could be different:

```py
Successfully built 018e75992e59
Successfully tagged django:latest
```

# Starting a new Django site

We will now create a new Docker container using the `docker run` command. The `-v "$(pwd)":/app` parameter creates a bind-mount of the current directory to `/app` in the container. Files in the current directory are shared between the host and the guest systems.

The second untagged parameter `django` defines the image that we use for the creation of a container. The rest of the command string is as follows:

```py
django django-admin startproject --template=https://github.com/arocks/edge/archive/master.zip --extension=py,md,html,env crypto_stats
```

This is passed to the guest container for execution. It creates a new Django project named `crypto_stats` using the edge template by Arun Ravindran ([https://django-edge.readthedocs.io/en/latest/](https://django-edge.readthedocs.io/en/latest/)):

```py
docker run -v "$(pwd)":/app django django-admin startproject --template=https://github.com/arocks/edge/archive/master.zip --extension=py,md,html,env crypto_stats
```

Upon successful execution, you should be able to see the following files and directories if you go inside the newly created `crypto_stats` folder:

![](img/ce85ce17-fff8-4bf1-83d7-a7ad9f454c72.png)

# Installation of Django dependencies

The `requirements.txt` file in the `crypto_stats` folder defines the Python package dependencies of our Django project. To install these dependencies, please issue the following `docker run` command. 

The `-p 8000:8000` parameter exposes port `8000` from the guest to the host machine. The `-it` parameter creates a pseudo-terminal with `stdin` support to allow an interactive terminal session.

We are again using the `django` image, but this time we launch a Bash Terminal shell instead:

```py
docker run -v "$(pwd)":/app -p 8000:8000 -it django bash
cd crypto_stats
pip install -r requirements.txt
```

You should make sure that you are still in your root project folder (that is, `Django`) when you issue the command.

The chain of commands would produce the following results:

![](img/183d2d48-3040-439f-bd55-6459f01a66bc.png)

# Django environment setup

Sensitive environment variables, such as Django's `SECRET_KEY` ([https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-SECRET_KEY](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-SECRET_KEY)), should be kept in a private file that is excluded from version control software. For simplicity, we can just use the sample from the project template:

```py
cd src
cp crypto_stats/settings/local.sample.env crypto_stats/settings/local.env
```

Next, we can use `manage.py` to create a default SQLite database and the superuser:

```py
python manage.py migrate
python manage.py createsuperuser
```

The `migrate` command initializes the database models, including user authentication, admin, user profiles, user sessions, content types, and thumbnails.

The `createsuperuser` command will ask you a series of questions for the creation of a superuser:

![](img/533006a6-da8b-49b8-8c0d-d5bccd54502f.png)

# Running the development server

Launching the default development server is very simple; in fact, it takes only a single line of code:

```py
python manage.py runserver 0.0.0.0:8000
```

The `0.0.0.0:8000` parameter will tell Django to serve the website to all addresses at port `8000`.

In your host machine, you can now launch your favorite browser and go to `http://localhost:8000` to see your site:

![](img/c5a99622-ec6d-41b6-86b6-44d0c3d7ff24.png)

The look of the site is decent, isn't it? 

# Showing Bitcoin prices using Django and Matplotlib

We have now established a fully fledged website backbone using just a few commands. I hope you will appreciate the simplicity of using Django for web development. Now, I will demonstrate how we can integrate Matplotlib charts into a Django site, which is the key topic of this chapter.

# Creating a Django app

An app in the Django ecosystem refers to an application that handles a specific function within a site. For instance, our default project comes with the profile and account apps already. With the terminology clarified, we are set to build an app to display the latest price of bitcoin.

We should keep our development server running in the background. When the server detects any changes to our code base, it will reload automatically to reflect the changes. Therefore, right now, we need to launch a new Terminal and attach to the running server container:

```py
docker exec -it 377bfb2f3db4 bash
```

The strange-looking numbers before `bash` refer to the ID of the container. We can find the ID from the terminal that holds the running server:

![](img/e4bba061-76af-4b7f-8b59-e6b3833a7929.png)

Alternatively, we can get the IDs of all running containers by issuing the following command:

```py
docker ps -a
```

The `docker exec` command helps you go back to the same Bash environment as the development server. We can now start a new app:

```py
cd /app/crypto_stats/src
python manage.py startapp bitcoin
```

Inside the host computer's project directory, we should be able to see a new `bitcoin` folder inside `crypto_stats/src/`: 

![](img/b2ab9c7d-d86b-4ce8-af7c-3b26f15e6b70.png)

# Creating a simple Django view

I will demonstrate the workflow of creating a Django view via a simple line chart. 

Inside the newly created bitcoin app folder, you should be able to find `views.py`, which stores all the views within the app. Let's edit it and create a view that outputs a Matplotlib line chart:

```py
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
from io import BytesIO
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt

def test_view(request):
    # Create a new Matplotlib figure
    fig, ax = plt.subplots()

    # Prepare a simple line chart
    ax.plot([1, 2, 3, 4], [3, 6, 9, 12])

    ax.set_title('Matplotlib Chart in Django') 

    plt.tight_layout()

    # Create a bytes buffer for saving image
    fig_buffer = BytesIO()
    plt.savefig(fig_buffer, dpi=150)

    # Save the figure as a HttpResponse
    response = HttpResponse(content_type='image/png')
    response.write(fig_buffer.getvalue())
    fig_buffer.close()

    return response
```

Since Tkinter is not available inside our server container, we need to switch the Matplotlib graphical backend from the default TkAgg to Agg by calling `matplotlib.use('Agg')` first.

`matplotlib.use('Agg')` must be called right after the line `import matplotlib` and before any other function calls of Matplotlib.

The function `test_view` (request) expects a Django `HttpRequest` object ([https://docs.djangoproject.com/en/2.0/ref/request-response/#django.http.HttpRequest](https://docs.djangoproject.com/en/2.0/ref/request-response/#django.http.HttpRequest)) as input, and outputs a Django `HttpResponse` object ([https://docs.djangoproject.com/en/2.0/ref/request-response/#django.http.HttpResponse](https://docs.djangoproject.com/en/2.0/ref/request-response/#django.http.HttpResponse)).

To import a Matplotlib plot to the `HttpResponse` object, we need to save the plot to an intermediate `BytesIO` object first, which can be found in the `io` package ([https://docs.python.org/3/library/io.html#binary-i-o](https://docs.python.org/3/library/io.html#binary-i-o)). The `BytesIO` object acts as a buffer of the binary image file, such that `plt.savefig` can write a PNG file directly into it.

Next, we create a new `HttpResponse()` object with the `content_type` parameter set to `image/png`. The binary content inside the buffer is exported to the `HttpResponse()` object via `response.write(fig_buffer.getvalue())`. Finally, the buffer is closed to free up the temporary memory.

To direct users to this view, we need to create a new file called `urls.py` inside the `{Project_folder}/crypto_stats/src/bitcoin` folder. 

```py
from django.urls import path

from . import views

app_name = 'bitcoin'
urlpatterns = [
    path('test/', views.test_view),
]
```

The line `path('test/', views.test_view)` states that all URLs with suffix `test/`  will be directed to the `test_view`.

We need to add our app's `url` patterns to the global patterns as well. Let's edit `{Project_folder}/crypto_stats/src/crypto_stats/urls.py`, and add the two lines commented as follows:

```py
...
import profiles.urls
import accounts.urls
# Import your app's url patterns here
import bitcoin.urls
from . import views

...

urlpatterns = [
    path('', views.HomePage.as_view(), name='home'),
    path('about/', views.AboutPage.as_view(), name='about'),
    path('users/', include(profiles.urls)),
    path('admin/', admin.site.urls),
    # Add your app's url patterns here
    path('bitcoin/', include(bitcoin.urls)),
    path('', include(accounts.urls)),
]
...
```

The line `path('bitcoin/', include(bitcoin.urls)),` states that every URL that begins with [http://<your-domain>/bitcoin](http://%3Cyour-domain/bitcoin) would be directed to the bitcoin app.

Wait for a few seconds until the development server reloads. You can now head to [http://localhost:8000/bitcoin/test/](http://localhost:8000/bitcoin/test/) to see your plot.

![](img/99e179ea-dfce-41fe-9964-cb70c7ef4e74.png)

# Creating a Bitcoin candlestick view

In this section, we are going to fetch the historical prices of Bitcoin from the Quandl API. Please note that we do not guarantee the accuracy, completeness, or validity of the visualizations presented; nor are we responsible for any errors or omissions that may have occurred. The data, visualizations, and analyses are provided on an *as is* basis for educational purposes only, without any warranties of any kind. Readers are advised to conduct their own independent research into individual cryptocurrencies before making a investment decision.

If you are not familiar with Quandl, it is a financial and economic data warehouse that stores millions of datasets from hundreds of publishers. Before you can use the Quandl API, you would need to register an account on its website ([https://www.quandl.com](https://www.quandl.com)). A free API access key can be obtained by following the instructions in this link [https://docs.quandl.com/docs#section-authentication](https://docs.quandl.com/docs#section-authentication). I will cover more about Quandl and APIs in the next chapter.

Now, remove the existing `views.py` file from the `crypto_stats/src/bitcoin` folder. Copy `views1.py` from this chapter's code repository to `crypto_stats/src/bitcoin`, and rename it as `views.py`. I will explain each part of `views1.py` accordingly.

The historical bitcoin prices data in the Bitstamp exchange can be found here: [https://www.quandl.com/data/BCHARTS/BITSTAMPUSD-Bitcoin-Markets-bitstampUSD](https://www.quandl.com/data/BCHARTS/BITSTAMPUSD-Bitcoin-Markets-bitstampUSD). The corresponding unique identifier for our target dataset is `BCHARTS/BITSTAMPUSD`. Although an official Python client library is available from Quandl, we are not going to use that for the sake of demonstrating the general procedures of importing JSON data. The function `get_bitcoin_dataset` uses nothing more than `urllib.request.urlopen` and `json.loads` to fetch the JSON data from the API. Finally the data is processed into a pandas DataFrame for our further consumption.

```py
... A bunch of import statements

def get_bitcoin_dataset():
    """Obtain and parse a quandl bitcoin dataset in Pandas DataFrame     format
     Quandl returns dataset in JSON format, where data is stored as a 
     list of lists in response['dataset']['data'], and column headers
     stored in response['dataset']['column_names'].

     Returns:
     df: Pandas DataFrame of a Quandl dataset"""

    # Input your own API key here
    api_key = ""

    # Quandl code for Bitcoin historical price in BitStamp exchange
    code = "BCHARTS/BITSTAMPUSD"
    base_url = "https://www.quandl.com/api/v3/datasets/"
    url_suffix = ".json?api_key="

    # We want to get the data within a one-year window only
    time_now = datetime.datetime.now()
    one_year_ago = time_now.replace(year=time_now.year-1)
    start_date = one_year_ago.date().isoformat()
    end_date = time_now.date().isoformat()
    date = "&start_date={}&end_date={}".format(start_date, end_date)

    # Fetch the JSON response 
    u = urlopen(base_url + code + url_suffix + api_key + date)
    response = json.loads(u.read().decode('utf-8'))

    # Format the response as Pandas Dataframe
    df = pd.DataFrame(response['dataset']['data'], columns=response['dataset']['column_names'])

    # Convert Date column from string to Python datetime object,
    # then to float number that is supported by Matplotlib.
    df["Datetime"] = date2num(pd.to_datetime(df["Date"], format="%Y-%m-%d").tolist())

    return df
```

Remember to specify your own API key at this line:  `api_key = ""`.

The `Date` column in `df` is recorded as a series of Python strings. Although Seaborn can use string-formatted dates in some functions, Matplotlib cannot. To make the dates malleable to data processing and visualizations, we need to convert the values to float numbers that is supported by Matplotlib. Therefore, I have used `matplotlib.dates.date2num` to perform the conversion.

Our data frame contains the opening and closing price, as well as the highest and lowest price per trading day. None of the plots we described thus far are able to describe the trend of all these variables in a single plot.

In the financial world, candlestick plot is almost the default choice for describing price movements of stocks, currencies, and commodities over a time period. Each candlestick consists of the body that describes the opening and closing prices, and extended wicks that illustrate the highest and lowest prices, in one particular trading day. If the closing price is higher than the opening price, the candlestick is often colored black. Conversely, the candlestick would be colored red if the closing is lower. Traders can then infer the opening and closing prices based on the combination of color and the boundary of candlestick body.

In the following example, we are going to prepare a candlestick chart of bitcoin in the last 30 trading days of our data frame. The `candlestick_ohlc` function was adapted from the deprecated `matplotlib.finance` package. It plots the time, open, high, low, and close as a vertical line ranging from low to high. It further uses a series of colored rectangular bars to represent the open-close span. 

```py
def candlestick_ohlc(ax, quotes, width=0.2, colorup='k', colordown='r',
 alpha=1.0):
    """
    Parameters
    ----------
    ax : `Axes`
    an Axes instance to plot to
    quotes : sequence of (time, open, high, low, close, ...) sequences
    As long as the first 5 elements are these values,
    the record can be as long as you want (e.g., it may store volume).
    time must be in float days format - see date2num
    width : float
        fraction of a day for the rectangle width
    colorup : color
        the color of the rectangle where close >= open
    colordown : color
        the color of the rectangle where close < open
    alpha : float
        the rectangle alpha level
    Returns
    -------
    ret : tuple
        returns (lines, patches) where lines is a list of lines
        added and patches is a list of the rectangle patches added
    """
    OFFSET = width / 2.0
    lines = []
    patches = []
    for q in quotes:
        t, open, high, low, close = q[:5]
        if close >= open:
            color = colorup
            lower = open
            height = close - open
        else:
            color = colordown
            lower = close
            height = open - close

        vline = Line2D(
                   xdata=(t, t), ydata=(low, high),
                   color=color,
                   linewidth=0.5,
                   antialiased=True,
        )
        rect = Rectangle(
                     xy=(t - OFFSET, lower),
                     width=width,
                     height=height,
                     facecolor=color,
                     edgecolor=color,
        )
        rect.set_alpha(alpha)
        lines.append(vline)
        patches.append(rect)
        ax.add_line(vline)
        ax.add_patch(rect)
        ax.autoscale_view()

    return lines, patches
```

The `bitcoin_chart` function handles the actual processing of user requests and the output of `HttpResponse`. 

```py
def bitcoin_chart(request):
    # Get a dataframe of bitcoin prices
    bitcoin_df = get_bitcoin_dataset()

    # candlestick_ohlc expects Date (in floating point number), Open, High, Low, Close columns only
    # So we need to select the useful columns first using DataFrame.loc[]. Extra columns can exist, 
    # but they are ignored. Next we get the data for the last 30 trading only for simplicity of plots.
    candlestick_data = bitcoin_df.loc[:, ["Datetime",
                                          "Open",
                                          "High",
                                          "Low",
                                          "Close",
                                          "Volume (Currency)"]].iloc[:30]

    # Create a new Matplotlib figure
    fig, ax = plt.subplots()

    # Prepare a candlestick plot
    candlestick_ohlc(ax, candlestick_data.values, width=0.6)

    ax.xaxis.set_major_locator(WeekdayLocator(MONDAY)) # major ticks on the mondays
    ax.xaxis.set_minor_locator(DayLocator()) # minor ticks on the days
    ax.xaxis.set_major_formatter(DateFormatter('%Y-%m-%d'))
    ax.xaxis_date() # treat the x data as dates

    # rotate all ticks to vertical
    plt.setp(ax.get_xticklabels(), rotation=90, horizontalalignment='right') 

    ax.set_ylabel('Price (US $)') # Set y-axis label

    plt.tight_layout()

    # Create a bytes buffer for saving image
    fig_buffer = BytesIO()
    plt.savefig(fig_buffer, dpi=150)

    # Save the figure as a HttpResponse
    response = HttpResponse(content_type='image/png')
    response.write(fig_buffer.getvalue())
    fig_buffer.close()

    return response
```

`ax.xaxis.set_major_formatter(DateFormatter('%Y-%m-%d'))` is useful for the conversion of floating point numbers back to dates.

Like the first Django view example, we need to modify our urls.py to direct the URLs to our `bitcoin_chart` view.

```py
from django.urls import path

from . import views

app_name = 'bitcoin'
urlpatterns = [
    path('30/', views.bitcoin_chart),
]
```

Voila! You can look at the bitcoin candlestick plot by going to `http://localhost:8000/bitcoin/30/`.

![](img/d2616a8d-0e10-4c28-a726-986aa922e1be.png)

# Integrating more pricing indicators

The candlestick plot in the current form is a bit bland. Traders would usually overlay stock indicators such as **average true range** (**ATR**), Bollinger band, **commodity channel index** (**CCI**), **exponential moving average** (**EMA**), **moving average convergence divergence** (**MACD**), **relative strength index** (**RSI**), and other various stats for technical analysis.

Stockstats ([https://github.com/jealous/stockstats](https://github.com/jealous/stockstats)) is a great package for calculating the preceding indicators/stats and much more. It wraps around pandas DataFrames and generate that stats on the fly when they are accessed.

In this section, we can convert a pandas DataFrame to a stockstats DataFrame via `stockstats.StockDataFrame.retype()`. A plethora of stock indicators can then be accessed by following the pattern `StockDataFrame["variable_timeWindow_indicator"]`. For example, `StockDataFrame['open_2_sma']` would give us 2-day simple moving average on opening price. Shortcuts may be available for some indicators, so please consult the official documentation for more information.

The file `views2.py` in our code repository contains the code to create an extended Bitcoin pricing view. You can copy `views2.py` from this chapter's code repository to `crypto_stats/src/bitcoin`, and rename it as `views.py`.

Here are the important changes to our previous code:

```py
# FuncFormatter to convert tick values to Millions
def millions(x, pos):
    return '%dM' % (x/1e6)

def bitcoin_chart(request):
    # Get a dataframe of bitcoin prices
    bitcoin_df = get_bitcoin_dataset()

    # candlestick_ohlc expects Date (in floating point number), Open, High, Low, Close columns only
    # So we need to select the useful columns first using DataFrame.loc[]. Extra columns can exist, 
    # but they are ignored. Next we get the data for the last 30 trading only for simplicity of plots.
    candlestick_data = bitcoin_df.loc[:, ["Datetime",
                                          "Open",
                                          "High",
                                          "Low",
                                          "Close",
                                          "Volume (Currency)"]].iloc[:30]

    # Convert to StockDataFrame
    # Need to pass a copy of candlestick_data to StockDataFrame.retype
    # Otherwise the original candlestick_data will be modified
    stockstats = StockDataFrame.retype(candlestick_data.copy())

    # 5-day exponential moving average on closing price
    ema_5 = stockstats["close_5_ema"]
    # 10-day exponential moving average on closing price
    ema_10 = stockstats["close_10_ema"]
    # 30-day exponential moving average on closing price
    ema_30 = stockstats["close_30_ema"]
    # Upper Bollinger band
    boll_ub = stockstats["boll_ub"]
    # Lower Bollinger band
    boll_lb = stockstats["boll_lb"]
    # 7-day Relative Strength Index
    rsi_7 = stockstats['rsi_7']
    # 14-day Relative Strength Index
    rsi_14 = stockstats['rsi_14']

    # Create 3 subplots spread across three rows, with shared x-axis. 
    # The height ratio is specified via gridspec_kw
    fig, axarr = plt.subplots(nrows=3, ncols=1, sharex=True, figsize=(8,8),
                             gridspec_kw={'height_ratios':[3,1,1]})

    # Prepare a candlestick plot in the first axes
    candlestick_ohlc(axarr[0], candlestick_data.values, width=0.6)

    # Overlay stock indicators in the first axes
    axarr[0].plot(candlestick_data["Datetime"], ema_5, lw=1, label='EMA (5)')
    axarr[0].plot(candlestick_data["Datetime"], ema_10, lw=1, label='EMA (10)')
    axarr[0].plot(candlestick_data["Datetime"], ema_30, lw=1, label='EMA (30)')
    axarr[0].plot(candlestick_data["Datetime"], boll_ub, lw=2, linestyle="--", label='Bollinger upper')
    axarr[0].plot(candlestick_data["Datetime"], boll_lb, lw=2, linestyle="--", label='Bollinger lower')

    # Display RSI in the second axes
    axarr[1].axhline(y=30, lw=2, color = '0.7') # Line for oversold threshold
    axarr[1].axhline(y=50, lw=2, linestyle="--", color = '0.8') # Neutral RSI
    axarr[1].axhline(y=70, lw=2, color = '0.7') # Line for overbought threshold
    axarr[1].plot(candlestick_data["Datetime"], rsi_7, lw=2, label='RSI (7)')
    axarr[1].plot(candlestick_data["Datetime"], rsi_14, lw=2, label='RSI (14)')

    # Display trade volume in the third axes
    axarr[2].bar(candlestick_data["Datetime"], candlestick_data['Volume (Currency)'])

    # Label the axes
    axarr[0].set_ylabel('Price (US $)')
    axarr[1].set_ylabel('RSI')
    axarr[2].set_ylabel('Volume (US $)')

    axarr[2].xaxis.set_major_locator(WeekdayLocator(MONDAY)) # major ticks on the mondays
    axarr[2].xaxis.set_minor_locator(DayLocator()) # minor ticks on the days
    axarr[2].xaxis.set_major_formatter(DateFormatter('%Y-%m-%d'))
    axarr[2].xaxis_date() # treat the x data as dates
    axarr[2].yaxis.set_major_formatter(FuncFormatter(millions)) # Change the y-axis ticks to millions
    plt.setp(axarr[2].get_xticklabels(), rotation=90, horizontalalignment='right') # Rotate x-tick labels by 90 degree

    # Limit the x-axis range to the last 30 days
    time_now = datetime.datetime.now()
    datemin = time_now-datetime.timedelta(days=30)
    datemax = time_now
    axarr[2].set_xlim(datemin, datemax)

    # Show figure legend
    axarr[0].legend()
    axarr[1].legend()

    # Show figure title
    axarr[0].set_title("Bitcoin 30-day price trend", loc='left')

    plt.tight_layout()

    # Create a bytes buffer for saving image
    fig_buffer = BytesIO()
    plt.savefig(fig_buffer, dpi=150)

    # Save the figure as a HttpResponse
    response = HttpResponse(content_type='image/png')
    response.write(fig_buffer.getvalue())
    fig_buffer.close()

    return response
```

Again, remember to specify your own API key at the line inside `get_bitcoin_dataset():  api_key = ""`.

The modified `bitcoin_chart` view would create three subplots that are spread across three rows, with a shared *x* axis. The height ratio between the subplots is specified via `gridspec_kw`.

The first subplot would display a candlestick chart as well as various stock indicators from the `stockstats` package.

The second subplot displays the **relative strength index** (**RSI**) of bitcoin across the 30-day window. 

Finally, the third subplot displays the volume (USD) of Bitcoin. A custom `FuncFormatter millions` is used to convert the *y* axis values to millions.

You can now go to the same link at [http://localhost:8000/bitcoin/30/](http://localhost:8000/bitcoin/30/) to view the complete chart.

![](img/2bb2dd45-be87-4286-ab8e-08e8b26093d9.png)

# Integrating the image into a Django template

To display the chart in our home page, we can modify the home page template at `{Project_folder}/crypto_stats/src/templates/home.html`.

We would need to modify the lines after the comment sentence `<!-- Benefits of the Django application -->` to the following:

```py
{% block container %}
<!-- Benefits of the Django application -->
<a name="about"></a>

<div class="container">
  <div class="row">
    <div class="col-lg-8">
      <h2>Bitcoin pricing trend</h2>
      <img src="img/" alt="Bitcoin prices" style="width:100%">
      <p><a class="btn btn-primary" href="#" role="button">View details &raquo;</a></p>
    </div>
    <div class="col-lg-4">
      <h2>Heading</h2>
      <p>Donec sed odio dui. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Vestibulum id ligula porta felis euismod semper. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa.</p>
      <p><a class="btn btn-primary" href="#" role="button">View details &raquo;</a></p>
    </div>
  </div>
</div>

{% endblock container %}
```

Basically, our `bitcoin_chart` view is loaded as an image through the line `<img src="img/" alt="Bitcoin prices" style="width:100%">`. I have also reduced the number of columns in the container section from 3 to 2, and adjusted the size of the first column by setting the class to  `col-lg-8` instead.

If you go to the home page (that is, `http://localhost:8000`), you will see the following screen when you scroll to the bottom:

![](img/d802a17d-36f4-431d-8928-437e70197d26.png)

There are a few caveats of this implementation. First, each page visit would incur an API call to Quandl, so your free API quota will be consumed quickly. A better way would be to fetch the prices daily and record the data in a proper database model. 

Second, the image output in its current form is not integrated into the app-specific template. This is out of the scope of this Matplotlib-focused book. However, interested readers can refer to the instructions in the online documentation ([https://docs.djangoproject.com/en/2.0/topics/templates/](https://docs.djangoproject.com/en/2.0/topics/templates/)).

Lastly, the images are static. There are third-party packages such as `mpld3` and Plotly that can turn a Matplotlib chart into an interactive Javascript-based chart. The use of these packages may further enhance the user experience.

# Summary

In this chapter, you learned about a popular framework made to simplify the development and deployment of web applications, called Django. You further learned about integrating Matplotlib charts into a Django site.

In the next chapter, we will cover some useful techniques to customize figure aesthetics for effective storytelling.
