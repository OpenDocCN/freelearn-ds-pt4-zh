
# Embedding Matplotlib in wxWidgets Using wxPython

This chapter will explain how we can use Matplotlib in the wxWidgets framework, particularly using wxPython bindings.

The contents of this chapter are as follows:

*   A brief introduction to wxWidgets and wxPython
*   A simple example of embedding Matplotlib in wxWidgets
*   Extending the previous example to include the Matplotlib navigation toolbar
*   How to update a Matplotlib plot in real time using the wxWidgets framework
*   How to design a GUI with wxGlade and embed a Matplotlib figure in it

Let's start with an overview of the features of wxWidgets and wxPython.

# A brief introduction to wxWidgets and wxPython

One of the most important features of wxWidgets is cross-platform portability; it currently supports Windows, macOS X, Linux (with the X11, Motif, and GTK+ libraries), OS/2, and several other operating systems and platforms (including an embedded version that is currently under development).

wxWidgets can best be described as a native mode toolkit because it provides a thin API abstraction layer across platforms and uses platform-native widgets under the hood, as opposed to emulating them. Using native controls gives wxWidgets applications a natural and familiar look and feel. On the other hand, introducing an additional layer can result in a slight performance penalty, although this is unlikely to be noticed in the kind of applications we will commonly develop.

wxWidgets is not restricted to GUI development. It's more than just a graphics toolkit, providing a whole set of additional facilities, such as database libraries, an inter-process communication layer, networking functionalities, and so on. Though it's written in C++, there are several bindings for many commonly used programming languages. Among them is a Python binding provided by wxPython.

wxPython (available at [http://www.wxpython.org/](http://www.wxpython.org/)) is a Python extension module that provides a set of bindings to the Python language from the wxWidgets library. This extension module allows Python programmers to create instances of wxWidgets classes and to invoke methods of those classes.

It is a great time to introduce wxPython because wxPython 4 was released a year ago. The latest version of wxPython is 4.0.1 to date (April 2018), and it is compatible with both Python 2 and Python 3.

Beginning in 2010, project Phoenix is an effort to clean up the wxPython implementation and to make it compatible with Python 3\. As one can imagine, wxPython was entirely rewritten with a focus on performance, maintainability, and extensibility.

Let us walk through the most basic example of using wxPython!

```py
#Here imports the wxPython library
import wx
#Every wxPython app is an instance of wx.App
app = wx.App(False)
#Here we create a wx.Frame() and specifying it as a top-level window
#by stating "None" as a parent object
frame = wx.Frame(None, wx.ID_ANY, "Hello World")
#Show the frame!
frame.Show(True)
#Start the applciation's MainLoop, and ready for events handling
app.MainLoop()
```

![](img/d4cc4e5e-3505-4253-8d28-4079cb9ae172.png)

Following on from the preceding example, there is one very important thing for beginners.

`wx.Frame` and `wx.Window()` are very different. `wx.Window` in wxWidgets is the base class from which all visual elements are derived, such as buttons and menus; we always refer to a program window as `wx.Frame` in wxWidgets.

The syntax for constructing a `wx.Frame` is `wx.Frame(Parent, ID, Title)`. When specifying `Parent` as `None`, as shown just now, we are essentially saying that the frame is a top-level `window`.

There is also an **ID system** in wxWidgets. Various controls and other parts of wxWidgets need an ID. Sometimes, the ID is provided by users; alternatively, it has a predefined value. However, the value of the ID is unimportant in most cases (such as the preceding example), and we can use `wx.ID_ANY` as the ID of an object, to tell wxWidgets to assign an ID automatically. Remember that all automatically assigned IDs are negative, and user-defined IDs should always be positive to avoid clashing with them.

Now, let us explore an example written in the OOP style that requires event handling—the `Hello world` button example:

```py
#Here imports the wxPython library
import wx

#Here is the class for the Frame inheriting from wx.Frame
class MyFrame(wx.Frame):

    #Instantiation based on the constructor defined below
    def __init__(self, parent):
        #creating the frame object and assigning it to self
        wx.Frame.__init__(self, parent, wx.ID_ANY)
        #Create panel
        self.panel = wx.Panel(self)
        #wx.BoxSizer is essentially the vertical box,
        #and we will add the buttons to the BoxSizer
        self.sizer = wx.BoxSizer(wx.VERTICAL)

        #Creating button 1 that will print Hello World once
        self.button1 = wx.Button(self.panel,label="Hello World!")
        #Create button 2 that will print Hello World twice
        self.button2 = wx.Button(self.panel,label="Hello World 5 times!")

        #There are two ways to bind the button with the event, here is method 1:
        self.button1.Bind(wx.EVT_BUTTON, self.OnButton1)
        self.button2.Bind(wx.EVT_BUTTON, self.OnButton2)

        #Here is method 2: 
        #self.Bind(wx.EVT_BUTTON, self.OnButton1, self.button1)
        #self.Bind(wx.EVT_BUTTON, self.OnButton2, self.button2)

        #Here we add the button to the BoxSizer
        self.sizer.Add(self.button1,0,0,0)
        self.sizer.Add(self.button2,0,0,0)

        #Put sizer into panel
        self.panel.SetSizer(self.sizer)

    #function that will be invoked upon pressing button 1
    def OnButton1(self,event):
        print("Hello world!")

    #function that will be invoked upon pressing button 2
    def OnButton2(self,event):
        for i in range(0,5):
            print("Hello world!")

#Every wxPython app is an instance of wx.App
app = wx.App()
#Here we create a wx.Frame() and specifying it as a top-level window
#by stating "None" as a parent object
frame = MyFrame(None)
#Show the frame!
frame.Show()
#Start the applciation's MainLoop, and ready for events handling
app.MainLoop()
```

The output looks like:

![](img/2b24a4cb-87f0-4f55-a26b-d7f99fea8619.png)

As readers may observe, all three GUI libraries that we have discussed here have a similar syntax. Therefore, getting familiar with just one of them lets you switch easily among them.

The layout managers for wxWidgets are `sizer` widgets: they are the containers for widgets (including other sizers) that will handle the visual arrangement of the widgets' dimensions according to our configuration. `BoxSizer` takes one parameter, its orientation. In this case, we pass the constant `wx.VERTICAL` to have widgets laid out in a column; however, there is also the constant `wx.HORIZONTAL` if we need a row of widgets:

```py
self.sizer.Add(self.button1, 1, wx.LEFT | wx.TOP | wx.EXPAND)
```

We are now able to add our `FigureCanvas` object to the `sizer`. The arguments of the `Add()` function are really important:

*   The first parameter is a reference to the object to be added.
*   Then, we have the second parameter proportion. This is used to express how much of the additional free space should be assigned to this widget. Often, the widgets on a GUI don't take up all the space, so there is some extra space available. This space is redistributed to all the widgets based on the proportion value of each widget with respect to all the widgets present in the GUI. Let's take an example: if we have three widgets respectively with the proportion set to `0`, `1`, and `2`, then the first (with the proportion set to `0`) will not change at all. The third (the proportion set to 2) will change twice more than the second (proportion `1`). In the book example, we set it to `1`, so we declare that the widget should take one slot of the free space available when resizing.
*   The third parameter is a combination of flags to further configure widget behavior in the sizer. It controls borders, alignment, separation between widgets, and expansions. Here we declare that the `FigureCanvas` should expand when the window is resized.

Let us try an example to embed a Matplotlib figure (polar plot) into the wxWidgets-powered GUI:

```py
#Specifying that we are using WXAgg in matplotlib
import matplotlib
matplotlib.use('WXAgg')
#Here imports the wxPython and other supporting libraries
import wx, sys, os, random, matplotlib, matplotlib.cm as cm, matplotlib.pyplot as plt
from numpy import arange, sin, pi, random, linspace
from matplotlib.backends.backend_wxagg import FigureCanvasWxAgg as FigureCanvas
from matplotlib.backends.backend_wx import NavigationToolbar2Wx
from matplotlib.figure import Figure

class MyFrame(wx.Frame):
    def __init__(self):
        # Initializing the Frame
        wx.Frame.__init__(self, None, -1, title="", size=(600,500))
        #Create panel
        panel = wx.Panel(self)

        #Here we prepare the figure, canvas and axes object for the graph
        self.fig = Figure(figsize=(6,4), dpi=100)
        self.canvas = FigureCanvas(self, -1, self.fig)
        self.ax = self.fig.add_subplot(111, projection='polar')

        #Here, we borrow one example shown in the matplotlib gtk3 cookbook
        #and show a beautiful bar plot on a circular coordinate system
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.plot_width = pi / 4 * random.rand(30)
        self.bars = self.ax.bar(self.theta, self.radii, width=self.plot_width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)
        #Here we generate the figure
        self.ax.plot()

        #Creating the vertical box of wxPython
        self.vbox = wx.BoxSizer(wx.VERTICAL)
        #Add canvas to the vertical box
        self.vbox.Add(self.canvas, wx.ALIGN_CENTER|wx.ALL, 1)
        #Add vertical box to the panel
        self.SetSizer(self.vbox)
        #Optimizing the size of the elements in vbox
        self.vbox.Fit(self)

#Every wxPython app is an instance of wx.App
app = wx.App()
#Here we create a wx.Frame() and specifying it as a top-level window
#by stating "None" as a parent object
frame = MyFrame()
#Show the frame!
frame.Show()
#Start the applciation's MainLoop, and ready for events handling
app.MainLoop()
```

Output:

![](img/7839a8db-0781-4cff-90dc-9a34b946bd2f.png)

We have shown how to embed a Matplotlib figure into the GUI; however, we have yet to demonstrate the interaction between Matplotlib and wxWidgets. It can be achieved easily by adding a button and a binding (`Bind`) a function to the button. This will update the figure each time the users click on it.

Let's walk through one important example showcasing how to update the plot by clicking a button! Although we are using the same figure, the underlying update methodology is different. Here we will update the figure through a click event instead of an auto-timer as shown in [Chapter 6](94590894-fac7-4931-a15e-c7ec328446aa.xhtml), *Embedding Matplotlib in Qt 5*:

```py
#Specifying that we are using WXAgg in matplotlib
import matplotlib
matplotlib.use('WXAgg')
#Here imports the wxPython and other supporting libraries
import wx, numpy, matplotlib.cm as cm, matplotlib.pyplot as plt
from numpy import arange, sin, pi, random, linspace
from matplotlib.backends.backend_wxagg import FigureCanvasWxAgg as FigureCanvas
from matplotlib.backends.backend_wx import NavigationToolbar2Wx
from matplotlib.figure import Figure

#This figure looks like it is from a radar, so we name the class radar
class Radar(wx.Frame):
    #Instantiation of Radar
    def __init__(self):
        # Initializing the Frame
        wx.Frame.__init__(self, None, -1, title="", size=(600,500))
        # Creating the panel
        panel = wx.Panel(self)
        #Setting up the figure, canvas and axes for drawing
        self.fig = Figure(figsize=(6,4), dpi=100)
        self.canvas = FigureCanvas(self, -1, self.fig)
        self.ax = self.fig.add_subplot(111, projection='polar')

        #Here comes the trick, create the button "Start Radar!"
        self.updateBtn = wx.Button(self, -1, "Start Radar!")
        #Bind the button with the clicking event, and invoke the update_fun function
        self.Bind(wx.EVT_BUTTON, self.update_fun, self.updateBtn)

        #Create the vertical box of Widgets
        self.vbox = wx.BoxSizer(wx.VERTICAL)
        #Add the canvas to the vertical box
        self.vbox.Add(self.canvas, wx.ALIGN_CENTER|wx.ALL, 1)
        #Add the button to the vertical box
        self.vbox.Add(self.updateBtn)
        #Add the vertical box to the Frame
        self.SetSizer(self.vbox)
        #Make sure the elements in the vertical box fits the figure size
        self.vbox.Fit(self)

    def update_fun(self,event):
        #Make sure we clear the figure each time before redrawing
        self.ax.cla()
        #updating the axes figure
        self.ax = self.fig.add_subplot(111, projection='polar')
        #Here, we borrow one example shown in the matplotlib gtk3 cookbook
        #and show a beautiful bar plot on a circular coordinate system
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.plot_width = pi / 4 * random.rand(30)
        self.bars = self.ax.bar(self.theta, self.radii, width=self.plot_width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)

        #Here we draw on the canvas!
        self.fig.canvas.draw()
        #And print on terminal to make sure the function was invoked upon trigger
        print('Updating figure!')

#Every wxPython app is an instance of wx.App
app = wx.App(False)
#Here we create a wx.Frame() and specifying it as a top-level window
#by stating "None" as a parent object
frame = Radar()
#Show the frame!
frame.Show()
#Start the applciation's MainLoop, and ready for events handling
app.MainLoop()
```

Output:

![](img/535ca0e7-3eba-4638-90f3-52be4b822c30.png) ![](img/551048e3-cd55-4f93-a2d9-708e260b336f.png)![](img/93b02621-4318-45ce-8c04-89ff832ec693.png) ![](img/478742a7-0ba1-4e9f-829d-3f2b74b63978.png)

By clicking on the Start Radar! button, we invoke the function `update_fun` and redraw a new graph every time.

# Embedding Matplotlib in a GUI from wxGlade

For very simple applications with limited GUIs, we can design the interface from inside the application source code. Once the GUI becomes more complex, this solution is not acceptable and we need a tool to support us in the GUI design. One of the most well-known tools for this activity in wxWidgets is wxGlade.

wxGlade is an interface design program written in Python using wxPython, and this allows it to run on all platforms where these two are available.

The philosophy is similar to Glade, the famous GTK+ GUI designer, and the look and feel are very similar as well. wxGlade is a program that helps us to create wxWidgets or wxPython user interfaces, but it is not a full-featured code editor; it's just a designer, and the code it generates does nothing more than display the created widgets.

Although project Phoenix and wxPython 4 are relatively new, they are both supported by wxGlade. wxGlade can be downloaded from sourceforge, and one can easily download the zipped file, unzip it, and run wxGlade with the **`python3` **command:

```py
python3 wxglade.py
```

And here comes the user interface!

![](img/6f2ae096-da93-4e00-b024-50e7dd2fecd9.png)

Here is a breakdown of the three main windows in *Figure 5*. The upper left window is the main Palette window. The first button of the first row (Windows) is the button to create a frame as a basis of everything. The lower left window is the Properties window, which lets us display and edit the properties of applications, windows, and controls. The window on the right is the Tree window. It enables us to visualize the structure. It allows editing the structure of the project, with its application, windows, sizers, and controls. By choosing an item in the Tree window, we can edit its corresponding properties in the Properties window.

Let us click on the button to Add a Frame. This will be followed by this small window:

![](img/dfade9ca-1daf-49ac-b009-7c0bf2dbdde8.png)

Select the base class as wxFrame; and here we will generate a Design window as shown in the following screenshot. From here, we can start to click and add buttons and features that we like:

![](img/3a3e8e5a-5c10-489f-b96f-b25aee9e662d.png)

Let us first create a container for the code we have shown previously. Before we click on any buttons, let us revisit the essential elements needed for the preceding GUI:

*   Frame
*   Vertical box (`wx.BoxSizer`)
*   Button

So it's very straightforward; let's click on the sizer button in the Palette window and then click on the Design window:

![](img/aad979d8-9169-4776-9441-b9066bc58781.png)

From the Tree window, we can see the structure of our GUI. We have a frame containing a sizer of two slots. However, we want a vertical box instead of a horizontal one; this can be modified in the Properties window when we click on sizer_2:

![](img/5c26ba1d-803e-481e-8181-4274fad5b641.png)

Now let's add a button to slot 2! This can be done by simply clicking on Button in the Palette window, then clicking on the slot in the lower part of the Design window. However, the button doesn't look very nice from there. It's on the left-hand side of the lower panel. This can be altered by selecting _button1 in the Tree window and modifying the alignment details in the Layout tab of the Properties window.

In here, we have selected wxEXPAND and wxALIGN_CENTER, which indicate that it has to expand and fill the width of the frame; this also ensures that it aligns at the center of the slot at all times:

![](img/177dc6d5-cf86-4c02-a609-12ae272d962b.png)

For now, the frame is all set. Let's export the code by choosing File and Generate Code:

![](img/2e52f081-98d1-4a91-a106-81e76ab0fe51.png)

Upon clicking on Generate Code, a file will be saved in the desired folder (where the user saved the `wxWidget` file) and here is a code snippet:

```py
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
#
# generated by wxGlade 0.8.0 on Sun Apr  8 20:35:42 2018
#

import wx

# begin wxGlade: dependencies
# end wxGlade

# begin wxGlade: extracode
# end wxGlade

class MyFrame(wx.Frame):
    def __init__(self, *args, **kwds):
        # begin wxGlade: MyFrame.__init__
        kwds["style"] = kwds.get("style", 0) | wx.DEFAULT_FRAME_STYLE
        wx.Frame.__init__(self, *args, **kwds)
        self.SetSize((500, 550))
        self.button_1 = wx.Button(self, wx.ID_ANY, "button_1")

        self.__set_properties()
        self.__do_layout()
        # end wxGlade

    def __set_properties(self):
        # begin wxGlade: MyFrame.__set_properties
        self.SetTitle("frame")
        # end wxGlade

    def __do_layout(self):
        # begin wxGlade: MyFrame.__do_layout
        sizer_2 = wx.BoxSizer(wx.VERTICAL)
        sizer_2.Add((0, 0), 0, 0, 0)
        sizer_2.Add(self.button_1, 0, wx.ALIGN_CENTER | wx.EXPAND, 0)
        self.SetSizer(sizer_2)
        self.Layout()
        # end wxGlade

# end of class MyFrame

class MyApp(wx.App):
    def OnInit(self):
        self.frame = MyFrame(None, wx.ID_ANY, "")
        self.SetTopWindow(self.frame)
        self.frame.Show()
        return True

# end of class MyApp

if __name__ == "__main__":
    app = MyApp(0)
    app.MainLoop()
```

The preceding code provides the GUI on its own. However, it lacks some key functions to make everything work. Let's quickly expand on that to see how it works:

```py
import matplotlib
matplotlib.use('WXAgg')

import wx, numpy, matplotlib.cm as cm, matplotlib.pyplot as plt
from numpy import arange, sin, pi, random, linspace
from matplotlib.backends.backend_wxagg import FigureCanvasWxAgg as FigureCanvas
from matplotlib.backends.backend_wx import NavigationToolbar2Wx
from matplotlib.figure import Figure

class MyFrame(wx.Frame):
    def __init__(self, *args, **kwds):
        # begin wxGlade: MyFrame.__init__
        kwds["style"] = kwds.get("style", 0) | wx.DEFAULT_FRAME_STYLE
        wx.Frame.__init__(self, *args, **kwds)
        self.SetSize((500, 550))
        self.button_1 = wx.Button(self, wx.ID_ANY, "button_1")
##Code being added***
        self.Bind(wx.EVT_BUTTON, self.__updat_fun, self.button_1)
        #Setting up the figure, canvas and axes
        self.fig = Figure(figsize=(5,5), dpi=100)
        self.canvas = FigureCanvas(self, -1, self.fig)
        self.ax = self.fig.add_subplot(111, projection='polar')
        ##End of Code being added***self.__set_properties()
        self.__do_layout()
        # end wxGlade

    def __set_properties(self):
        # begin wxGlade: MyFrame.__set_properties
        self.SetTitle("frame")
        # end wxGlade

    def __do_layout(self):
        # begin wxGlade: MyFrame.__do_layout
        sizer_2 = wx.BoxSizer(wx.VERTICAL)
        sizer_2.Add(self.canvas, 0, wx.ALIGN_CENTER|wx.ALL, 1)
        sizer_2.Add(self.button_1, 0, wx.ALIGN_CENTER | wx.EXPAND, 0)
        self.SetSizer(sizer_2)
        self.Layout()
        # end wxGlade
##The udpate_fun that allows the figure to be updated upon clicking
##The __ in front of the update_fun indicates that it is a private function in Python syntax
    def __updat_fun(self,event):
        self.ax.cla()
        self.ax = self.fig.add_subplot(111, projection='polar')
        #Here, we borrow one example shown in the matplotlib gtk3 cookbook
        #and show a beautiful bar plot on a circular coordinate system
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.plot_width = pi / 4 * random.rand(30)
        self.bars = self.ax.bar(self.theta, self.radii, width=self.plot_width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)

        self.fig.canvas.draw()
        print('Updating figure!')

# end of class MyFrame class MyApp(wx.App):
    def OnInit(self):
        self.frame = MyFrame(None, wx.ID_ANY, "")
        self.SetTopWindow(self.frame)
        self.frame.Show()
        return True

# end of class MyApp

if __name__ == "__main__":
    app = MyApp(0)
    app.MainLoop()
```

# Summary

We are now able to develop wxWidgets applications and then embed Matplotlib in them. To be specific, readers should be able to embed a Matplotlib figure in a wxFrame, use a sizer to embed both the figure and navigation toolbar in a wxFrame, update a plot through interaction, and use wxGlade to design a GUI for Matplotlib embedding. 

We are now ready to move further and see how to integrate Matplotlib into the web.
