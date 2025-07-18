

# Embedding Matplotlib in GTK+3

We have worked on quite a few examples so far, and now have a good foundation from which to use Matplotlib to generate data plots and figures. While using Matplotlib alone is very handy in generating interactive figures, experimenting with datasets, and understanding substructures of data, there may be instances where we want an application to acquire, parse, and display our data.

In this chapter, we will study examples on how to embed Matplotlib in applications through GTK+3. 

# Installing and setting up GTK+3

Setting up GTK+3 is fairly simple and straightforward. There are quite a few ways to install GTK+3 depending on your OS version and environment.

We encourage our readers refer to the link: [https://python-gtk-3-tutorial.readthedocs.io/en/latest/install.html](https://python-gtk-3-tutorial.readthedocs.io/en/latest/install.html) for the latest updates and information on installation.

At the time of writing this book, the official website advises users to install GTK+3 through JHBuild. However, users have experienced a compatibility issue with JHBuild with the macOS El Capitan. 

We recommend macOS users to use the package manager `brew` to install GTK+3.

GTK+3 can be installed simply if you have `brew` installed in your macOS:

```py
#Installing the gtk3 package
brew install gtk3
#Installing PyGObject
brew install pygobject3
```

For Linux systems such as Ubuntu, GTK+3 is installed by default. For sophisticated and advanced users who prefer a more customized approach to installing GTK+3, we encourage referring their website to obtain the most recently updated information.

We have observed that GTK+3 visualization is not as compatible with IPython Notebook. We encourage you to run the code on the Terminal for best results.

# A brief introduction to GTK+3

Before exploring the various examples and applications, let's first acquire a brief, high-level understanding of the GTK+3.

GTK+3 contains a set of graphical control elements (widgets) and is a highly usable, feature-rich toolkit used to develop graphical user interfaces. It has cross-platform compatibility and is relatively easy to use. GTK+3 is an object-oriented widget toolkit written in the C programming language. Therefore, when running GTK+3 in Python, we need a wrapper to call the functions in the GTK+3 library. In this case, PyGObject is a Python module that serves as the wrapper and saves us time by not having to learn two languages to plot our figures. PyGObject exclusively supports GTK+3 or later versions. If you prefer to use GTK+2 in your application, we recommend using PyGTK instead.

Together with the Glade GUI builder, they provide a very powerful application development environment.

# Introduction to the GTK+3 signal system

GTK+3 is an event-driven toolkit, which means it is always dormant in a loop function and waiting (*listening*) for events to occur; then it passes control to the appropriate function. Examples of events are a click on a button, menu item activation, ticking a checkbox, and so forth. When widgets receive an event, they frequently emit one or more signals. That signal will then evoke functions that you have connected to, in this case known as **callbacks**. This passing of control is done using the concept of signals.

Although the terminology is almost identical, GTK+3 signals are not the same as Unix system signals and are not implemented using them.

When an *event* such as the press of a mouse button occurs, the appropriate signal is emitted by the click that received the widget. This is one of the most important parts of how GTK+3 works. There are signals that all widgets inherit, such as *destroy* and *delete-event*, and there are signals that are widget-specific, such as toggling on a toggle button. To make the signal framework functional, we need to set up a signal handler to catch those signals and call the appropriate function.

From a more abstract point of view, a generic example is as follows:

```py
handler_id = widget.connect("Event", callback, data )
```

`widget`, shown in this generic example, is an instance of a widget we created earlier. It can display widgets, buttons, toggles, or text data entry. Each widget has its own particular *event* that must occur for it to respond. If the widget is a button, and when there is an action such as a click, a signal is issued. The `callback` argument is the name of the callback function. The callback function will be executed when the *event* has occurred. Finally, the `data` argument includes any data that should be passed when the signal is generated; this is optional and can be left out if the *callback* function requires no argument.

Here is our first sample of GTK+3:

```py
#In here, we import the GTK module in order to access GTK+3's classes and functions
#We want to make sure we are importing GTK+3 and not any other version of the library
#Therefore we require_version('Gtk','3.0')
import gi
gi.require_version('Gtk', '3.0')
from gi.repository import Gtk

#This line uses the GTK+3 functions and creates an empty window
window = Gtk.Window(title="Hello World!")
#We created a handler that connects window's delete event to ensure the application
#is terminated if we click on the close button
window.connect("destroy",Gtk.main_quit)
#Here we display the window
window.show_all()
#This tells the code to run the main loop until Gtk.main_quit is called
Gtk.main()
```

To run this code, readers can either copy and paste or it save the code into a file named `first_gtk_example.py` and run it in the Terminal as follows:

```py
python3 first_gtk_example.py
```

The readers should be able to create an empty 200x200 pixel window (by default when not specified otherwise), shown as follows:

![](img/493616a6-4cd9-4193-810a-ea81267cca2b.png)

Figure 1

To fully appreciate the usefulness of GTK3+, it is advisable that the code be written as a PyGObject.

The following code demonstrates a modified, slightly more complicated example of having two click buttons in one window, each performing different tasks!

Readers should install `cairocffi` through `pip3` before running the examples in this chapter:

`pip3 install cairocffi`

The `cairocffi` library is a CFFI-based drop-in replacement for Pycairo, and is necessary in this case. Now let's delve into the code:

```py
#Again, here we import the GTK module
import gi
gi.require_version('Gtk', '3.0')
from gi.repository import Gtk

#From here, we define our own class, namely TwoClicks.
#This is a sub-class of Gtk.Window
class TwoClicks(Gtk.Window):

    #Instantiation operation will creates an empty object
    #Therefore, python3 uses __init__() to *construct* an object
    #__init__() will be automatically invoked when the object is being created!
    #You can call this the constructor in Python3
    #Noted that *self* here indicates the reference of the object created from this class
    #Anything starting with self.X refers to the local function or variables of the object itself!
    def __init__(self):

        #In here, we are essentially constructing a Gtk.Window object
        #And parsing the information title="Hello world" to the constructor of Gtk.Window
        #Therefore, the window will have a title of "Hello World"
        Gtk.Window.__init__(self, title="Hello World")

        #Since we have two click buttons, we created a horizontally oriented box container
        #with 20 pixels placed in between children - the two click buttons
        self.box = Gtk.Box(spacing=100)

        #This assigns the box to become the child of the top-level window
        self.add(self.box)

        #Here we create the first button - click1, with the title "Print once!" on top of it
        self.click1 = Gtk.Button(label="Print once!")

        #We assign a handler and connect the *Event* (clicked) with the *callback/function* (on_click1)
        #Noted that, we are now calling the function of the object itself
        #Therefore we are using *self.onclick1 
        self.click1.connect("clicked", self.on_click1)

        #Gtk.Box.pack_start() has a directionality here, it positions widgets from left to right!
        self.box.pack_start(self.click1, True, True, 0)

        #The same applies to click 2, except that we connect it with a different function
        #which prints Hello World 5 times!
        self.click2 = Gtk.Button(label="Print 5 times!")
        self.click2.connect("clicked", self.on_click2)
        self.box.pack_start(self.click2, True, True, 0)

    #Here defines a function on_click1 in the Class TwoClicks
    #This function will be triggered when the button "Print once!" is clicked
    def on_click1(self, widget):
        print("Hello World")

    #Here defines a function on_click2 in the Class TwoClicks
    #This function will be triggered when the button "Print 5 times!" is clicked
    def on_click2(self, widget):
        for i in range(0,5):
            print("Hello World")

#Here we instantiate an object, namely window
window = TwoClicks()
#Here we want the window to be close when the user click on the close button
window.connect("delete-event", Gtk.main_quit)
#Here we display the window!
window.show_all()
#This tells the code to run the main loop until Gtk.main_quit is called
Gtk.main()
```

Here is what you will get from the preceding snippet:

![](img/ae3098a5-3f23-45af-b106-f9d3590881e9.png)

Figure 2

Clicking on different buttons will result in obtaining different outcomes on the Terminal.

This example serves as an introduction to the **object-oriented programming** (**OOP**) style. OOP is fairly sophisticated for novice users, yet it is one of the best ways to arrange one's code, create modules, and make it more readable and usable for other programmers. Although novice users may not have noticed, we have already used a lot of OOP concepts in the first four chapters.

By understanding `init()` and `self`, we are now ready to delve into more advanced programming skills. So, let's try some more advanced examples! What if we want to embed some of the plots we have made into GTK+3 windows? We do the following:

```py
#Same old, importing Gtk module, we are also importing some other stuff this time
#such as numpy and the backends of matplotlib
import gi, numpy as np, matplotlib.cm as cm
gi.require_version('Gtk', '3.0')
from gi.repository import Gtk

#From here, we are importing some essential backend tools from matplotlib
#namely the NavigationToolbar2GTK3 and the FigureCanvasGTK3Agg
from matplotlib.backends.backend_gtk3 import NavigationToolbar2GTK3 as NavigationToolbar
from matplotlib.backends.backend_gtk3agg import FigureCanvasGTK3Agg as FigureCanvas
from matplotlib.figure import Figure

#Some numpy functions to create the polar plot
from numpy import arange, pi, random, linspace

#Here we define our own class MatplotlibEmbed
#By simply instantiating this class through the __init__() function,
#A polar plot will be drawn by using Matplotlib, and embedded to GTK3+ window
class MatplotlibEmbed(Gtk.Window):

    #Instantiation
    def __init__(self):
        #Creating the Gtk Window
        Gtk.Window.__init__(self, title="Embedding Matplotlib")
        #Setting the size of the GTK window as 400,400
        self.set_default_size(400,400)

        #Readers should find it familiar, as we are creating a matplotlib figure here with a dpi(resolution) 100
        self.fig = Figure(figsize=(5,5), dpi=100)
        #The axes element, here we indicate we are creating 1x1 grid and putting the subplot in the only cell
        #Also we are creating a polar plot, therefore we set projection as 'polar
        self.ax = self.fig.add_subplot(111, projection='polar')

        #Here, we borrow one example shown in the matplotlib gtk3 cookbook
        #and show a beautiful bar plot on a circular coordinate system
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.width = pi / 4 * random.rand(30)
        self.bars = self.ax.bar(self.theta, self.radii, width=self.width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)
        #Here we generate the figure
        self.ax.plot()

        #Here comes the magic, a Vbox is created
        #VBox is a containder subclassed from Gtk.Box, and it organizes its child widgets into a single column
        self.vbox = Gtk.VBox()
        #After creating the Vbox, we have to add it to the window object itself!
        self.add(self.vbox)

        #Creating Canvas which store the matplotlib figure
        self.canvas = FigureCanvas(self.fig)  # a Gtk.DrawingArea
        # Add canvas to vbox
        self.vbox.pack_start(self.canvas, True, True, 0)

        # Creating toolbar, which enables the save function!
        self.toolbar = NavigationToolbar(self.canvas, self)
        self.vbox.pack_start(self.toolbar, False, False, 0)

#The code here should be self-explanatory by now! Or refer to earlier examples for in-depth explanation
window = MatplotlibEmbed()
window.connect("delete-event", Gtk.main_quit)
window.show_all()
Gtk.main()
```

In this example, we created a vertical box and put both the canvas (with the figure) and the toolbar in it:

![](img/7182defa-8f5b-48ed-a084-824a35763ff8.png)

Figure 3

It seems like it is fairly easy to incorporate Matplotlib figures right into GTK+3, doesn't it? If you have your own figure that you wish to plug it into the GTK+3 engine, simply expand on the *polar* plot example and then you can start playing with your own figure with this template!

One additional thing we did here is create a toolbar and put it at the bottom of the figure. Remember that we are using a VBox in organizing the widgets? V in this case stands for vertical, which organizes data from top to bottom. Therefore, when putting the toolbar after the canvas, we have such an ordering. The toolbar is a great place to modify and save your figures elegantly.

So let's try a few more examples and see how we can create some interactive plots by combining GTK+3 and Matplotlib. One very important concept is event connections with Matplotlib through canvas; this can be achieved by calling the `mpl_connect()` function.

There are many good examples that can be found on the *Matplotlib Cookbook* online.

Let's walk through one such example that provides an interactive zoom-in function. Here is a preview of the output of the code:

![](img/435f4e70-5bc8-43c8-b69b-037481518c5c.png)

Figure 4

The window comprises two subplots; the plot on the left side is the big picture, while the plot on the right side is the zoomed-in version. The area chosen for enlargement is designated by the gray box on the left, and the gray box is movable along the click of your mouse. This may sound complicated but it can easily be accomplished by just a few lines of code. We would suggest readers first read the following code featuring the class `DrawPoints` and try to trace the logic starting from `window = Gtk.Window()`.

Here's an in-depth explanation of the code:

```py
#Same old, importing Gtk module, we are also importing some other stuff this time
#such as numpy and the backends of matplotlib
import gi, numpy as np, matplotlib.cm as cm
gi.require_version('Gtk', '3.0')
from gi.repository import Gtk

#From here, we are importing some essential backend tools from matplotlib
#namely the NavigationToolbar2GTK3 and the FigureCanvasGTK3Agg
from numpy import random
from matplotlib.backends.backend_gtk3 import NavigationToolbar2GTK3 as NavigationToolbar
from matplotlib.backends.backend_gtk3agg import FigureCanvasGTK3Agg as FigureCanvas
from matplotlib.figure import Figure
from matplotlib.patches import Rectangle

#Here we created a class named DrawPoints
class DrawPoints:

    #Upon initiation, we create 4 randomized numpy array, those are for the coordinates, colors and size of dots
    #on the scatter plot. After that we create a figure object, put in two subplots and create a canvas to store
    #the figure.
    def __init__(self):
        #Namely we are creating 20 dots, therefore n = 20
        self.n = 20
        #X and Y coordinates
        self.xrand = random.rand(1,self.n)*10
        self.yrand = random.rand(1,self.n)*10
        #Sizes
        self.randsize = random.rand(1,self.n)*200
        #Colors
        self.randcolor = random.rand(self.n,3)

        #Here creates the figure, with a size 10x10 and resolution of 80dpi
        self.fig = Figure(figsize=(10,10), dpi=80)
        #Stating that we are creating two plots side by side and adding 
        #self.ax as the first plot by add_subplot(121)
        self.ax = self.fig.add_subplot(121)
        #Adding the second subplot by stating add_subplot(122)
        self.axzoom = self.fig.add_subplot(122)
        #Create a canvas to store the figure object
        self.canvas = FigureCanvas(self.fig)

    #Here draw the scatterplot on the left
    def draw(self):
        #Here is the key - cla(), when we invoke the draw() function, we have to clear the
        #figure and redraw it again
        self.ax.cla()
        #Setting the elements of the left subplot, in this case - grid
        self.ax.grid(True)
        #Set the maximum value of X and Y-axis in the left subplot
        self.ax.set_xlim(0,10)
        self.ax.set_ylim(0,10)
        #Draw the scatter plot with the randomized numpy array that we created earlier in __init__(self)
        self.ax.scatter(self.xrand, self.yrand, marker='o', s=self.randsize, c=self.randcolor, alpha=0.5)

    #This zoom function is invoked by updatezoom() function outside of the class Drawpoints
    #This function is responsible for things:
    #1\. Update X and Y coordinates based on the click
    #2\. invoke the draw() function to redraw the plot on the left, this is essential to update the position
    # of the grey rectangle 
    #3\. invoke the following drawzoom() function, which will "Zoom-in" the designated area by the grey rectangle
    # and will redraw the subplot on the right based on the updated X & Y coordinates
    #4\. draw a transparent grey rectangle based on the mouse click on the left subplot
    #5\. Update the canvas
    def zoom(self, x, y):
        #Here updates the X & Y coordinates
        self.x = x
        self.y = y
        #invoke the draw() function to update the subplot on the left
        self.draw()
        #invoke the drawzoom() function to update the subplot on the right
        self.drawzoom()
        #Draw the transparent grey rectangle at the subplot on the left
        self.ax.add_patch(Rectangle((x - 1, y - 1), 2, 2, facecolor="grey", alpha=0.2))
        #Update the canvas
        self.fig.canvas.draw()

    #This drawzoom function is being called in the zoom function
    #The idea is that, when the user picked a region (rectangle) to zoom, we need to redraw the zoomed panel,
    #which is the subplot on the right
    def drawzoom(self):
        #Again, we use the cla() function to clear the figure, and getting ready for a redraw!
        self.axzoom.cla()
        #Setting the grid
        self.axzoom.grid(True)
        #Do not be confused! Remember that we invoke this function from zoom, therefore self.x and self.y
        #are already updated in that function. In here, we are simply changing the X & Y-axis minimum and 
        #maximum value, and redraw the graph without changing any element!
        self.axzoom.set_xlim(self.x-1, self.x+1)
        self.axzoom.set_ylim(self.y-1, self.y+1)
        #By changing the X & Y-axis minimum and maximum value, the dots that are out of range will automatically
        #disappear!
        self.axzoom.scatter(self.xrand, self.yrand, marker='o', s=self.randsize*5, c=self.randcolor, alpha=0.5)

def updatecursorposition(event):
    '''When cursor inside plot, get position and print to statusbar'''
    if event.inaxes:
        x = event.xdata
        y = event.ydata
        statbar.push(1, ("Coordinates:" + " x= " + str(round(x,3)) + "  y= " + str(round(y,3))))

def updatezoom(event):
    '''When mouse is right-clicked on the canvas get the coordiantes and send them to points.zoom'''
    if event.button!=1: return
    if (event.xdata is None): return
    x,y = event.xdata, event.ydata
    points.zoom(x,y)

#Readers should be familiar with this now, here is the standard opening of the Gtk.Window()
window = Gtk.Window()
window.connect("delete-event", Gtk.main_quit)
window.set_default_size(800, 500)
window.set_title('Interactive zoom')

#Creating a vertical box, will have the canvas, toolbar and statbar being packed into it from top to bottom
box = Gtk.Box(orientation=Gtk.Orientation.VERTICAL)
#Adding the vertical box to the window
window.add(box)

#Instantiate the object points from the Class DrawPoints()
#Remember that at this point, __init__() of DrawPoints() are invoked upon construction!
points = DrawPoints()
#Invoke the draw() function in the object points
points.draw()

#Packing the canvas now to the vertical box
box.pack_start(points.canvas, True, True, 0)

#Creating and packing the toolbar to the vertical box
toolbar = NavigationToolbar(points.canvas, window)
box.pack_start(toolbar, False, True, 0)

#Creating and packing the statbar to the vertical box
statbar = Gtk.Statusbar()
box.pack_start(statbar, False, True, 0)

#Here is the magic that makes it happens, we are using mpl_connect to link the event and the canvas!
#'motion_notify_event' is responsible for the mouse motion sensing and position updating
points.fig.canvas.mpl_connect('motion_notify_event', updatecursorposition)
#'button_press_event' is slightly misleading, in fact it is referring to the mouse button being pressed, 
#instead of a GTK+3 button being pressed in this case
points.fig.canvas.mpl_connect('button_press_event', updatezoom)

window.show_all()
Gtk.main()

```

As you can see from the preceding example, event handling and picking are the elements that makes the interaction part much easier than we imagine. Therefore, it is important to have a quick review of the available event connections that are part of the `FigureCanvasBase`:

| **Event name** | **Class and description** |
| `button_press_event` | **MouseEven****t:** Mouse button is pressed |
| `button_release_event` | **MouseEvent**: Mouse button is released |
| `scroll_event` | **MouseEvent**: Mouse scroll wheel is rolled |
| `motion_notify_event` | **MouseEvent:** Mouse motion |
| `draw_event` | **DrawEvent**: Canvas draw |
| `key_press_event` | **KeyEvent:** Key is pressed |
| `key_release_event` | **KeyEvent:** Key is released |
| `pick_event` | **PickEvent:** An object in the canvas is selected |
| `resize_event` | **ResizeEvent:** Figure canvas is resized |
| `figure_enter_event` | **LocationEvent:** Mouse enters a new figure |
| `figure_leave_event` | **LocationEvent:** Mouse leaves a figure |
| `axes_enter_event` | **LocationEvent:** Mouse enters a new axis |
| `axes_leave_event` | **LocationEvent:** Mouse leaves an axis |

# Installing Glade

Installing Glade is very straightforward; you can either obtain the source file from its web page, or simply use Git to obtain the latest version of source code. Here is the command for obtaining Glade through Git:

```py
git clone git://git.gnome.org/glade
```

# Designing the GUI using Glade

Designing the GUI using Glade is straightforward. Just start the Glade program and you will see this interface (from macOS, or something similar if using another OS):

![](img/44defb87-3489-41e2-b548-8f4065c1da2e.png)

Figure 5

Let's now explore the Glade interface. There are four main buttons that we will be using primarily in Glade; **Toplevels**, **Containers**, **Control**, and **Display**. The preceding screenshot shows that `GtkWindow` is listed in `Toplevels`, which serves as the basic unit for construction. Let's click on `GtkWindow` and see the resulting action:

![](img/69fd8b1c-76b5-4c31-aa33-f3ba79dcbcc3.png)

Figure 6

Now a GtkWindow is being constructed and nothing is inside. Let's set the size of this GtkWindow to: 400x400\. This can be achieved by setting the default width and height as 400 in the lower section of the right panel. The right panel is currently illustrating the **General** properties of this GtkWindow.

Remember that we used vertical boxes a lot in the previous examples? Let's add a vertical box to the GtkWindow! This can be achieved by clicking on **Containers** and choosing GtkBox, as shown here:

![](img/7d4d5098-80b6-4b3b-8625-a29bcf3520aa.png)

Figure 7

After choosing GtkBox, click on the GtkWindow in the middle panel and a GtkBox will be created as a sub-module or a sub-window of the GtkWindow. This can be confirmed by checking the left panel as shown here:

![](img/f42ac166-7752-4787-bde8-08c29974d75f.png)

Figure 8

GtkBox is below GtkWindow and indented on the left panel. Since we are picking vertical box, the *orientation* in **General** is *vertical*. One can even specify the spacing and number of items that will be included in the GtkBox. Let's add a menu bar onto the top vertical box. This can be done as shown in *Figure 9*. In Containers, pick GtkMenubar and click on the top vertical box. It will fit in a menu bar with the following options: File, Edit, View, and Help.

![](img/7b6a0395-6a49-42a7-bc87-e02fe7ac1645.png)

Figure 9

As one can imagine, we can easily design our favorite GUI with the use of Glade. We can import a label with a customized size as shown in *Figure 10*. And there are many more options that we can choose to customize our GUIs.

![](img/8bf02ba5-db87-4d58-b760-84402d510b43.png)

Figure 10

Designing the most effective GUI through Glade is beyond the scope and purview of this book, so we will not go any further into the more advanced options in Glade.

However, we would like to expand on one of the examples that we have worked on previously, and show that it takes just a few lines of code to incorporate a Glade-based GUI into our workflow.

To begin, we will use the class-based polar plot example. First of all, we design the most basic `GtkWindow` with a size of 400x400 (that's all!) from Glade and save it as a file.

The file is very simple and self-explanatory:

```py
<?xml version="1.0" encoding="UTF-8"?>
<!-- Generated with glade 3.22.1 -->
<interface>
  <requires lib="gtk+" version="3.22"/>
  <object class="GtkWindow" id="window1">
    <property name="can_focus">False</property>
    <property name="default_width">400</property>
    <property name="default_height">400</property>
    <signal name="destroy" handler="on_window1_destroy" swapped="no"/>
    <child>
      <object class="GtkScrolledWindow" id="scrolledwindow1">
        <property name="visible">True</property>
        <property name="can_focus">True</property>
        <property name="shadow_type">in</property>
        <child>
          <placeholder/>
        </child>
      </object>
    </child>
  </object>
</interface>
```

Readers may understand that we just created a `GtkWindow` with a size of 400x400 plus a child as `GtkScrolledWindow`. This can be completed in a few clicks in Glade.

And what we have to do now is use `Gtk.Builder()` to read the Glade file; everything will be constructed automatically. This actually saves us from defining all the elements of the vertical box!

```py
#Same old, importing Gtk module, we are also importing some other stuff this time
#such as numpy and the backends of matplotlib
import gi, numpy as np, matplotlib.cm as cm
gi.require_version('Gtk', '3.0')
from gi.repository import Gtk

from matplotlib.figure import Figure
from numpy import arange, pi, random, linspace
import matplotlib.cm as cm
#Possibly this rendering backend is broken currently
from matplotlib.backends.backend_gtk3agg import FigureCanvasGTK3Agg as FigureCanvas

#New class, here is to invoke Gtk.main_quit() when the window is being destroyed
#Necessary to quit the Gtk.main()
class Signals:
    def on_window1_destroy(self, widget):
        Gtk.main_quit()

class MatplotlibEmbed(Gtk.Window):

    #Instantiation, we just need the canvas to store the figure!
    def __init__(self):

        #Readers should find it familiar, as we are creating a matplotlib figure here with a dpi(resolution) 100
        self.fig = Figure(figsize=(5,5), dpi=100)
        #The axes element, here we indicate we are creating 1x1 grid and putting the subplot in the only cell
        #Also we are creating a polar plot, therefore we set projection as 'polar
        self.ax = self.fig.add_subplot(111, projection='polar')

        #Here, we borrow one example shown in the matplotlib gtk3 cookbook
        #and show a beautiful bar plot on a circular coordinate system
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.width = pi / 4 * random.rand(30)
        self.bars = self.ax.bar(self.theta, self.radii, width=self.width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)
        #Here we generate the figure
        self.ax.plot()

        #Creating Canvas which store the matplotlib figure
        self.canvas = FigureCanvas(self.fig)  # a Gtk.DrawingArea

#Here is the magic, we create a GTKBuilder that reads textual description of a user interface
#and instantiates the described objects
builder = Gtk.Builder()
#We ask the GTKBuilder to read the file and parse the information there
builder.add_objects_from_file('/Users/aldrinyim/Dropbox/Matplotlib for Developer/Jupyter notebook/ch05/window1_glade.glade', ('window1', '') )
#And we connect the terminating signals with Gtk.main_quit()
builder.connect_signals(Signals())

#We create the first object window1
window1 = builder.get_object('window1')
#We create the second object scrollwindow
scrolledwindow1 = builder.get_object('scrolledwindow1')

#Instantiate the object and start the drawing!
polar_drawing = MatplotlibEmbed()
#Add the canvas to the scrolledwindow1 object
scrolledwindow1.add(polar_drawing.canvas)

#Show all and keep the Gtk.main() active!
window1.show_all()
Gtk.main()
```

The preceding code demonstrates how we can use Glade to quickly generate a frame and execute it effortlessly.

![](img/57c12f40-5eea-489d-8b6f-a089be59efa8.png)

Figure 11

Hopefully, through this example readers would appreciate the power of Glade in enabling programmers to draw a GUI instead of abstracting it in code. This is particularly useful when the GUI gets complicated.

# Summary

In this chapter, we have worked through examples on embedding Matplotlib figures inside a simple GTK+3 window, adding the Matplotlib navigation toolbar, plotting data in an interactive framework, and using Glade to design a GUI. We have kept the examples simple to highlight the important parts, but we encourage readers to explore further possibilities. GTK+3 is not the only GUI library that can be used. In the coming chapters, we'll see how to use two other important libraries!
