# What is Matplotlib?

Matplotlib is a Python package for data visualization. It allows easy creation of various plots, including line, scattered, bar, box, and radial plots, with high flexibility for refined styling and customized annotation. The versatile `artist` module allows developers to define basically any kind of visualization. For regular usage, Matplotlib offers a simplistic object-oriented interface, the `pyplot` module, for easy plotting.

Besides generating static graphics, Matplotlib also supports an interactive interface which not only aids in creating a wide variety of plots but is also very useful in creating web-based applications.

Matplotlib is readily integrated into popular development environments, such as Jupyter Notebook, and it supports many more advanced data visualization packages.

# Merits of Matplotlib

There are many advantages in creating data visualization with code so that the visualization streamlines into part of the result generation pipeline. Let's have a look at some of the key advantages of the Matplotlib library. 

# Easy to use

The Matplotlib plotting library is easy to use in several ways:

*   Firstly, the object-oriented module structures simplify the plotting process. More often than not, we're only required to call `import maplotlib.pyplot as plt` to import the plotting API to create and customize many basic plots.
*   Matplotlib is highly integrated with two common data analytics packages, pandas and NumPy. For example, we can simply append `.plot()` to a pandas DataFrame such as by `df.plot()` to create a simple plot, and customize its styling with Matplotlib syntax.
*   For styling, Matplotlib offers functions to alter the appearance of each feature, and ready-made default style sheets are also available to avoid these extra steps when refined aesthetics is not required.

# Diverse plot types

Often in data analytics, we need sophisticated plots to express our data. Matplotlib offers numerous plotting APIs natively, and is also the basis for a collection of third-party packages for additional functionalities, including:

*   **Seaborn**: Provides simple plotting APIs, including some advanced plot types, with aesthetically appealing default styling
*   **HoloViews**: Creates interactive plots with metadata annotation from bundled data 
*   **Basemap/GeoPandas/Canopy**: Maps data values to colors on geographical maps

We would learn some of the applications of these third-party packages in later chapters on advanced plotting.

# Hackable to the core (only when you want)

When we want to go beyond the default settings to ensure that the resultant figure meets our specific purpose, we can customize the appearance and behaviors of each plot feature:

*   Per-element styling is possible
*   The ability to plot data values as colors and draw any shape of patches allows the creation of almost any kind of visualization
*   Useful in customizing plots created by extensions such as Seaborn

# Open source and community support

As Matplotlib is open source, it enables developers and data analysts to use it for free. The users also have the freedom to improve and contribute to the Matplotlib library. As part of the open source experience, the users get prompt online support from the members of the global community on various platforms and forums.

# What's new in Matplotlib 2.x?

Matplotlib supports Python 3 since version 1.2, released in 2013\. The Matplotlib 2.0 release introduced a number of changes and upgrades to improve data visualization projects. Let us look at some of the key improvements and upgrades. 

# Improved functionality and performance

Matplotlib 2.0 presents new features that improve user experience, including speed, and output quality, as well as resource usage.

# Improved color conversion API and RGBA support

The alpha channel that specifies the transparency level is fully supported in Matplotlib 2.0.

# Improved image support

Matplotlib 2.0 now resamples images with less memory and less data type conversion.

# Faster text rendering

Community developers claim that the speed of text rendering by the `Agg` backend has improved by 20%.

# Change in the default animation codec

A very efficient codec, H.264, is now used as the default, which replaces MPEG-4, to generate video output for animated plots. With H.264, we can now have longer video record time and lesser data traffic and loading time thanks to the higher compression rate and smaller output file size. It is also noted that real-time playback of H.264 videos is better than those encoded in MPEG-4.

# Changes in default styles

There are a number of style changes for improved visualization, such as more intuitive colors by default. We will discuss more in the chapter on figure aesthetics.

For details on all Matplotlib updates, you may visit [http://matplotlib.org/devdocs/users/whats_new.html](http://matplotlib.org/devdocs/users/whats_new.html).

# Matplotlib website and online documentation

As developers, you probably recognize the importance of reading documentation and manuals to get acquainted with syntax and functionality. We would like to reiterate the importance of reading the library documentation and encourage you to do the same. You can find the documentation here: [https://matplotlib.org](https://matplotlib.org/). On the official Matplotlib website, you would find the documentation for each function, news of latest releases and ongoing development, and a list of third-party packages, as well as tutorials and galleries of example plots.

However, building advanced and sophisticated plots by reading through documentation from scratch means a much steeper learning curve and a lot more time spent, especially when the documentation is regularly updated for better comprehension. This book aims to provide the reader with a guided road-map to accelerate the learning process, save time and effort, and put theory into practice. The online manuals can serve as the atlases you can turn to whenever you want to explore further.

The Matplotlib source code is available on GitHub at [https://github.com/matplotlib/matplotlib](https://github.com/matplotlib/matplotlib). We encourage our readers to fork it and add their ideas!

# Output formats and backends 

Matplotlib enables users to obtain output plots as static figures. The plots can also be piped and made responsive through interactive backends. 

# Static output formats

Static images are the most commonly used output format for reporting and presentation purposes, and for our own quick inspection of data. Static images can be classified into two distinct categories.

# Raster images

Raster is the classic image format that provides support to a wide variety of image files, including PNG, JPG and BMP. Each raster image can be seen as a dense array of color values. For raster images, resolution matters.

The amount of image details kept is measured in **dots per inch** (**DPI**). The higher the DPI value (that is, the more pixel dots kept in it), the clearer the resultant image would be, even when stretched to a larger size. Of course, the file size and computational resources needed for the rendering would increase accordingly.

# Vector images

For vector images, instead of a matrix of discrete color dots, information is saved as paths, which are lines joining dots. They scale without losing any details:

*   SVG
*   PDF
*   PS

# Setting up Matplotlib

Now that we have a comprehensive overview of the capabilities and functionalities of Matplotlib, we are ready to get our hands dirty and work through some examples. We will begin after ensuring that we have set up the Matplotlib environment. Follow along the steps discussed to set up the environment. 

# Installing Python

Since version 2.0, Matplotlib supports both Python 2.7 and 3.4+. We are using Python 3 in this book, which is the latest stable Python version. You can download Python from [http://www.python.org/download/](http://www.python.org/download/).

# Python installation for Windows 

Python comes as an installer or zipped source code for Windows. We recommend the executable installer. Choose the right computer architecture for the best performance. You can call Python in the Command Prompt by pressing the Windows + *R* keys and typing `cmd.exe`, as shown in the following screenshot:

![](img/78c14f01-eb5f-49d4-a56c-aee840e7d040.png)

# Python installation for macOS

macOS natively comes with Python 2.7\. To install Python 3.4+, download the installation wizard and follow the instructions. Following is a screenshot of the wizard at the first step:

![](img/6fa2f292-a7ed-4c0a-9020-3c1dd47a047c.png)

Some Python packages require Xcode command-line tools to compile properly. Xcode can be obtained from the Mac App Store. To install the command-line tools, enter the following command in the Terminal: `xcode-select --install`. Then follow the installation prompts.

# Python installation for Linux

Most Linux distributions have Python 3.4 preinstalled. You may confirm this by typing `python3` in the Terminal. If you see the following, it means Python 3.4 is present:

```py
Python 3.6.3 (default, Oct 6 2017, 08:44:35) [GCC 5.4.0 20160609] on linux Type "help", "copyright", "credits" or "license" for more information. >>>​
```

If the Python shell does not appear at the command, you can install Python 3 with `apt`, the Linux software management tool:

```py
sudo apt update
sudo apt install Python3 build-essential
```

The `build-essential` package contains compilers that are useful for building non-pure Python packages. Meanwhile, you may need to substitute `apt` with `apt-get` if you have Ubuntu 14.04 or older.

# Installing Matplotlib

Matplotlib requires a large number of dependencies. We recommend installing Matplotlib by a Python package manager, which will help you to automatically resolve and install dependencies upon each installation or upgrade of a package. We will demonstrate how to install Matplotlib with `pip`.

# About the dependencies

Matplotlib depends on a host of Python packages for background calculation, graphic rendering, interaction, and more. They are NumPy, libpng, and FreeType, and so on. Depending on the usage, users can install additional backend packages, such as PyQt5, for a better user interface.

# Installing the pip Python package manager

We recommend installing Matplotlib using the Python package manager `pip`; it resolves basic dependencies automatically. `pip` is installed with `Python 2 >= 2.7.9` or `Python 3 >= 3.4` binaries.

If `pip` is not installed, you may do so by downloading `get-pip.py` from [http://bootstrap.pypa.io/get-pip.py](http://bootstrap.pypa.io/get-pip.py), and running it in the console:

```py
python3 get-pip.py
```

To upgrade `pip` to the latest version, do this: 

```py
pip3 install --upgrade pip
```

The documentation for `pip` can be found at [http://pip.pypa.io](http://pip.pypa.io/).

# Installing Matplotlib with pip

Enter `python3 -m pip install matplotlib` on the Terminal/Command Prompt to install. Add a `--user` option for `pip install` for users without root/admin rights where necessary.

# Setting up Jupyter Notebook

To create our plots, we need a user-friendly development environment.

Jupyter Notebook provides an interactive coding ground to edit and run your code, display the results, and document them neatly. Data and methods can be loaded to the memory for reuse within a session. As each notebook is hosted as a web server, you can connect to notebook instances running at a remote server on a browser.

If you are excited to try it out before installing, you may go to [https://try.jupyter.org](https://try.jupyter.org/) and open a Python 3 notebook.

To install Jupyter, type this in your console: 

```py
python3 -m pip install jupyter
```

# Starting a Jupyter Notebook session

Just type `jupyter notebook` in the console. This will start a Jupyter Notebook session as a web server.

By default, a notebook session should pop up on your default browser. To manually open the page, type `localhost:8888` as the URL. Then you will enter the following home page of the Jupyter Notebook:

![](img/c65bbed6-9858-43ca-a60e-5d40fa199339.png)

You can choose to host the notebook on different ports, for instance, when you are running multiple notebooks. You can specify the port to use with the `--port=<customportnum>` option.

Since the release of 4.3, token authentication has been added to Jupyter, so you may be asked for a token password before entering the notebook home page, as shown in the following screenshot:

![](img/91507fbc-f312-4612-9724-ea789ab67dd0.png)

To retrieve the token, such as when visiting the running notebook from a different browser or machine, you may call `jupyter notebook list` from the console:

![](img/8f7d0c80-5102-4976-a573-e013dabcee4b.png)

# Running Jupyter Notebook on a remote server

To open a notebook running on a remote server, you may set up port forwarding during SSH, as follows:

```py
ssh –L 8888:localhost:8888 mary@remoteserver
```

Then you may open the notebook again with `localhost:8888` as the URL.

When multiple users are running Jupyter Notebooks on the same server on the same port (say the default, `8888`) and each uses the same port forwarding, there is a possibility that your notebook content will be forwarded to another user who cannot read his/her own content without changing the port. While this might be fixed with later releases, it is recommended to change the port from the default. 

To upgrade from a previous version, run the following command:

```py
pip3 install --upgrade matplotlib
```

`pip` will automatically collect and install the Matplotlib dependencies for you.

# Editing and running code

A Jupyter Notebook has boxes called **cells**. It begins with the text input area for code editing, known as the gray box cell, by default. To insert and edit the code do the following:

1.  Click inside the gray box.
2.  Type your Python code inside it.
3.  Click on the play button or press *Shift* + *Enter* to run the current cell and move the cursor to the next cell:

![](img/a514e680-8386-41d2-a451-5515181c9852.png)

Once a cell is run, the relevant data and methods are loaded to the memory and can be used across cells in the same notebook kernel. No reloading is needed unless for an intended change. This saves effort and time in debugging and reloading large datasets.

# Manipulating notebook kernel and cells

You can use the toolbar at the top to manipulate cells and the kernel. The available functions are annotated as follows:

![](img/1ea382b7-c667-4225-8375-f3893d1e2d16.png)

Verify your output amount before running a cell! A huge output stream doesn't usually kill your console, but it can easily crash your browser and notebook in a matter of seconds. This issue has been addressed since Jupyter 4.2 by stopping large output. However, it does not guarantee to capture all non-stop output. Therefore, readers are advised to exercise caution and avoid attempts to obtain large output results in a notebook cell. Consider slicing it for a glimpse or obtaining output in another file:

![](img/d00f4180-4bf4-45b5-a23b-78ebc0519892.png)

# Embed your Matplotlib plots

Matplotlib is highly integrated into Jupyter Notebook. Use the Jupyter built-in *magic* command `%matplotlib inline` (set as default in the current release) to display resultant plots as static image output at each cell:

![](img/10ec035e-61bf-422b-aba3-827dd767f8d5.png)

Alternatively, you can run a magic cell command—`%matplotlib notebook` to use the interactive Matplotlib GUI for zooming or rotating in the same output area:

![](img/1fc043f2-5808-469e-aead-fe36030907a8.png)

# Documenting in Markdown

Jupyter Notebook supports Markdown syntax for organized documentation:

1.  Select Markdown from the drop-down list in the toolbar.
2.  Write your notes in the gray input box.
3.  Click on Run or *Shift* + *Enter*:

![](img/e8c394d9-ac9e-4435-9715-769a9fe04236.png)

After running the cell, the text will be styled in the display:

![](img/e4dd6677-6d1d-45a3-b239-471a091ee090.png)

You can find a detailed Markdown cheat sheet by Adam Pritchard at [https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

# Save your hard work!

Jupyter Notebook auto-saves itself every 2 minutes. As good practice, you should save it yourself more often by clicking on the floppy icon on the toolbar, or more conveniently by *Ctrl* + *S*.

Each project you open on Jupyter is saved as the JSON-based `.ipynb` notebook format:

![](img/4c0f2039-c640-4bfc-8fc2-2800defbe691.png)

The `.ipynb` notebook is portable across different Jupyter servers. Notebooks can be exported as basic runnable Python script `.py`, Markdown `.md` for documentation, and web page format `.html` for instant display of the flow of your project without having the readers install Jupyter Notebook in advance. It also supports LaTex format and PDF conversion via LaTex upon installation of the dependency Pandoc. If interested, you may check out the installation instructions at [http://pandoc.org/installing.html](http://pandoc.org/installing.html).

# Summary

Woo-hoo! We've taken our first steps in our Matplotlib journey together. You can rest assured you have a comprehensive understanding of Matplotlib's capabilities and also have the requisite environment set up. Now that we have successfully ventured into data visualization and gotten our feet wet, let's go ahead and build our first plots!