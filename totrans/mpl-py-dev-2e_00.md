# Preface

Python is a general-purpose programming language that's increasingly being used for data analysis and visualization. Matplotlib is a popular data visualization package in Python that's used to design effective plots and graphs. This book is a practical, hands-on resource to help you visualize data with Python using the Matplotlib library.

This book shows you how to create attractive graphs, charts, and plots using Matplotlib. You will also get a quick introduction to the third-party packages Seaborn, pandas, Basemap, and Geopandas, and learn how to use them with Matplotlib. After that, you'll embed and customize your plots in third-party tools such as GTK+, Qt 5, and WXWIDGETS.

You'll also be able to tweak the look and feel of your visualization with the help of the practical examples provided in this book. Further on, you'll explore Matplotlib 2.1.x on the web from a cloud-based platform using third-party packages such as Flask and Django. Finally, you will integrate interactive, real-time visualization techniques into your current workflow with the help of practical real-world examples.

By the end of this book, you'll be thoroughly comfortable with using the popular Python data visualization library Matplotlib 2.1.x, and leverage its power to build attractive, insightful, and powerful visualizations.

# Who this book is for

This book is essentially for anyone who wants to create intuitive data visualizations using the Matplotlib library. If you're a data scientist or analyst and wish to create attractive visualizations using Python, you'll find this book useful. Some knowledge of Python programming is all you need to get started.

# What this book covers

[Chapter 1](840a0403-49de-4053-adf9-bf389ed96cf5.xhtml), *Introduction to Matplotlib*, gets you familiar with the capabilities and functionalities of Matplotlib.

[Chapter 2](e1962b92-9e72-4c5a-bdd8-11b7ce29411b.xhtml), *Getting Started with Matplotlib*, gets you started with basic plotting techniques using Matplotlib syntax.

[Chapter 3](be9d4d89-e9b1-4296-804a-f1c80d567a31.xhtml), *Decorating Graphs with Plot Styles and Types*, shows how to beautify your plots and select the right kind of plot that communicates your data effectively.

[Chapter 4](456b0dc2-84d5-40f9-bf63-1ab4635cbac8.xhtml), *Advanced Matplotlib*, teaches you how to group multiple relevant plots into subplots in one figure using nonlinear scales, axis scales, plotting images, and advanced plots with the help of some popular third-party packages.

[Chapter 5](b069fa19-b6c3-4558-acf4-fcc22b4786c1.xhtml), *Embedding Matplotlib in GTK+3*, shows examples of embeding Matplotlib in applications using GTK+3.

[Chapter 6](94590894-fac7-4931-a15e-c7ec328446aa.xhtml), *Embedding Matplotlib in Qt 5*, explains how to embed a figure in a QWidget, use layout manager to pack a figure in a QWidget, create a timer, react to events, and update a Matplotlib graph accordingly. We use QT Designer to draw a simple GUI for Matplotlib embedding.

[Chapter 7](04b1a880-a2e6-43b0-a699-4dad85443a79.xhtml), *Embedding Matplotlib in wxWidgets Using wxPython*, shows how you can use Matplotlib in the wxWidgets framework, particularly using wxPython bindings.

[Chapter 8](fa218074-c1d0-4cfc-9f8a-a9b42bdb2e3b.xhtml), *Integrating Matplotlib with Web Applications*, teaches you how to develop a simple site that displays the price of Bitcoin. 

[Chapter 9](97241b85-a708-44e3-9b6f-163fcd54372f.xhtml), *Matplotlib in the Real World*, begins our journey of understanding more advanced Matplotlib usage through real-world examples.

[Chapter 10](666b0202-682e-450a-af1a-8536428c0df0.xhtml), *Integrating Data Visualization into the Workflow*, covers a mini-project combining the skills of data analytics with the visualization techniques you have learned.

# To get the most out of this book

A working installation of Python 3.4 or later is required. The default Python distribution can be obtained from [https://www.python.org/download/](https://www.python.org/download/). The installation of packages is covered in the chapters, but you can refer to the official documentation pages for more details. A Windows 7+, macOS 10.10+, or Linux-based computer with 4 GB RAM or above is recommended.

# Download the example code files

You can download the example code files for this book from your account at [www.packtpub.com](http://www.packtpub.com). If you purchased this book elsewhere, you can visit [www.packtpub.com/support](http://www.packtpub.com/support) and register to have the files emailed directly to you.

You can download the code files by following these steps:

1.  Log in or register at [www.packtpub.com](http://www.packtpub.com/support).
2.  Select the SUPPORT tab.
3.  Click on Code Downloads & Errata.
4.  Enter the name of the book in the Search box and follow the onscreen instructions.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

*   WinRAR/7-Zip for Windows
*   Zipeg/iZip/UnRarX for Mac
*   7-Zip/PeaZip for Linux

The code bundle for the book is also hosted on GitHub at [https://github.com/PacktPublishing/Matplotlib-for-Python-Developers-Second-Edition/](https://github.com/PacktPublishing/Matplotlib-for-Python-Developers-Second-Edition/). In case there's an update to the code, it will be updated on the existing GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at **[https://github.com/PacktPublishing/](https://github.com/PacktPublishing/)**. Check them out!

# Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: [http://www.packtpub.com/sites/default/files/downloads/MatplotlibforPythonDevelopersSecondEdition_ColorImages.pdf](http://www.packtpub.com/sites/default/files/downloads/MatplotlibforPythonDevelopersSecondEdition_ColorImages.pdf).

# Conventions used

There are a number of text conventions used throughout this book.

`CodeInText`: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "Another parameter for tuning is `dash_capstyle`."

A block of code is set as follows:

```py
import matplotlib.pyplot as plt
plt.figure(figsize=(4,4))
x = [0.1,0.3]
plt.pie(x)
plt.show()
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```py
        self.SetSize((500, 550))
        self.button_1 = wx.Button(self, wx.ID_ANY, "button_1")
##Code being added***
        self.Bind(wx.EVT_BUTTON, self.__updat_fun, self.button_1)
        #Setting up the figure, canvas and axes
```

Any command-line input or output is written as follows:

```py
python3 first_gtk_example.py
```

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For example, words in menus or dialog boxes appear in the text like this. Here is an example: "Select Qt in Files and Classes and Qt Designer Form in the middle panel."

Warnings or important notes appear like this.

Tips and tricks appear like this.

# Get in touch

Feedback from our readers is always welcome.

**General feedback**: Email `feedback@packtpub.com` and mention the book title in the subject of your message. If you have questions about any aspect of this book, please email us at `questions@packtpub.com`.

**Errata**: Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit [www.packtpub.com/submit-errata](http://www.packtpub.com/submit-errata), selecting your book, clicking on the Errata Submission Form link, and entering the details.

**Piracy**: If you come across any illegal copies of our works in any form on the Internet, we would be grateful if you would provide us with the location address or website name. Please contact us at `copyright@packtpub.com` with a link to the material.

**If you are interested in becoming an author**: If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit [authors.packtpub.com](http://authors.packtpub.com/).

# Reviews

Please leave a review. Once you have read and used this book, why not leave a review on the site that you purchased it from? Potential readers can then see and use your unbiased opinion to make purchase decisions, we at Packt can understand what you think about our products, and our authors can see your feedback on their book. Thank you!

For more information about Packt, please visit [packtpub.com](https://www.packtpub.com/).

# Introduction to Matplotlib

*"A picture is worth a thousand words." - Fred R Barnard*

Welcome aboard to the journey of creating good data visuals. In this era of exploding big data, we are probably well aware of the importance of data analytics. Developers are keen to join the data mining game, and build tools to collect and model all kinds of data. Even for non-data analysts, information such as performance test results and user feedback is often paramount in improving the software being developed. While strong statistical skills surely set the foundation of successful software development and data analysis, good storytelling is crucial even for the best data crunching results. The quality of graphical data representation often determines whether or not you can extract useful information during exploratory data analysis and get the message across during the presentation.

Matplotlib is a versatile and robust Python plotting package; it provides clean and easy ways to produce various quality data graphics and offers huge flexibility for customization.

In this chapter, we will introduce Matplotlib as follows: what it does, why you would want to use it, and how to get started. Here are the topics we will cover:

*   What is Matplotlib?
*   Merits of Matplotlib
*   What's new in Matplotlib?
*   Matplotlib websites and online documentation.
*   Output formats and backends.
*   Setting up Matplotlib.