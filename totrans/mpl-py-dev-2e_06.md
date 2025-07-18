
# Embedding Matplotlib in Qt 5

There are several GUI libraries available, and one widely used library is Qt. In this book, we will be using Qt 5, the latest major version of this library. Unless explicitly mentioned, we are referring to Qt 5 when we simply state Qt throughout the chapter.

We will follow a similar progression to that in [Chapter 5](b069fa19-b6c3-4558-acf4-fcc22b4786c1.xhtml), *Embedding Matplotlib in GTK+3*, and we will present similar examples but this time written in Qt.

We believe that this method will allow us to directly compare the libraries, and it has the advantage of not leaving the *How would I write something with library X?* question unanswered.

In this chapter, we will learn how to:

*   Embed a Matplotlib figure into a Qt widget
*   Embed a figure and navigation toolbar into a Qt widget
*   Use events to update a Matplotlib plot in real time
*   Use QT Designer to draw a GUI and then use it with Matplotlib in a simple Python application

We will begin by giving an introduction to the library.

# A brief introduction to Qt 5 and PyQt 5

Qt is a cross-platform application development framework widely used for graphical programs (GUI) and also for non-GUI tools.

Qt was developed by Trolltech (now owned by Nokia), and it's probably best known for being the foundation of the **K Desktop Environment** (**KDE**) for Linux.

The Qt toolkit is a collection of classes made to simplify the creation of programs. Qt is more than just a GUI toolkit. It includes components for abstractions of network sockets, threads, Unicode, regular expressions, SQL databases, SVG, OpenGL, and XML. It also has a fully functional web browser, help system, multimedia framework, and rich collection of GUI widgets.

Qt is available on several platforms, particularly Unix/Linux, Windows, macOS X, and also some embedded devices. As it uses native APIs of the platform to render the Qt controls, applications developed with Qt have a look and feel that fits the running environment (without looking like something alien in it).

Though written in C++, Qt can also be used in several other programming languages through language bindings available for Ruby, Java, Perl, and also Python with PyQt.

PyQt 5 is available for both Python 2.x and 3.x, but in this book, we will consistently use Python 3 in all our code. PyQt 5 has over 620 classes and 6,000 functions and methods. Before we go into some examples, it is important to know the difference between Qt 4/PyQt 4 and Qt 5/PyQt 5.

# Differences between Qt 4 and PyQt 4

PyQt is a comprehensive set of Python bindings for the Qt framework. However, PyQt 5 is not backward compatible with PyQt 4\. It is noteworthy that PyQt 5 does not support any part of the Qt API that are marked as deprecated or obsolete in Qt v5.0\. However, it is possible that some of these are included accidentally. If included, they *are* considered bugs and will be removed when found.

If you are familiar with Qt 4 or have read the first edition of this book, one thing to note is that the signals and slots of are no longer supported. Therefore, the following are not implemented in PyQt 5:

*   `QtScript`
*   `QObject.connect()`
*   `QObject.emit()`
*   `SIGNAL()`
*   `SLOT()`

Also, there is a modification in `disconnect()` as it no longer takes arguments and will disconnect all connections to the `QObject` instance when invoked.

However, new modules have been introduced, such as the following:

*   `QtBluetooth`
*   `QtPositioning`
*   `Enginio`

Let's start with a very simple example—calling a window. Again for best performance, copy the code, paste it in a file, and run the script on the Terminal. Our codes are optimized for running on a Terminal only:

```py
#sys.argv is essential for the instantiation of QApplication!
import sys
#Here we import the PyQt 5 Widgets
from PyQt5.QtWidgets import QApplication, QWidget

#Creating a QApplication object
app = QApplication(sys.argv)
#QWidget is the base class of all user interface objects in PyQt5
w = QWidget()
#Setting the width and height of the window
w.resize(250, 150)
#Move the widget to a position on the screen at x=500, y=500 coordinates
w.move(500, 500)
#Setting the title of the window
w.setWindowTitle('Simple')
#Display the window
w.show()

#app.exec_() is the mainloop of the application
#the sys.exit() is a method to ensure a real exit upon receiving the signal of exit from the app
sys.exit(app.exec_())
```

![](img/7588ada9-d7d9-4146-8bb1-f5b67dce764d.png)

The syntax is very similar to what you see in [Chapter 5](b069fa19-b6c3-4558-acf4-fcc22b4786c1.xhtml), *Embedding Matplotlib in GTK+3*. Once you know how to use one particular GUI library fairly well (such as GTK+3), it is very easy to adapt to a new one readily. The code is very similar to that in GTK+3 and the logic follows as well. `QApplication` manages the GUI application's control flow and main settings. It's the place where the main event loop is executed, processed and dispatched. It is also responsible for application initialization and finalization and handling most of the system-wide and application-wide settings. Since `QApplication` handles the entire initialization phase, it must be created before any other objects related to the UI are created.

The `qApp.exec_()` command enters the Qt main event loop. Once `exit()` or `quit()` is called, it returns the relevant return code. Until the main loop is started, nothing is displayed on the screen. It's necessary to call this function as the main loop handles all events and signals coming from both the application widgets and the window system; essentially, no user interaction can take place before it is called.

Readers may wonder why there is an underscore in `exec_();`. The reason is simple: `exec()` is a reserved word in Python hence the addition of the underscore to the `exec()` Qt method. Wrapping it inside `sys.exit()` allows the Python script to exit with the same return code, informing the environment how the application ended (whether successfully or not).

For more experienced readers, you will find something abnormal in the preceding code. While we were instantiating the `QApplication` class, we were required to parse `sys.argv` (an empty list in this case) to the constructor of `QApplication`. At least I found it unexpected when I first used PyQt, but this is required as the instantiation invokes the constructor of the C++ class `QApplication`, and it uses `sys.argv` to initialize the Qt application. Parsing `sys.argv` during `QApplication` instantiation is a convention in Qt and it is something be aware of. Also every PyQt 5 application must create an application object.

Again, let's try to another one in OOP style:

```py
#Described in earlier examples
import sys
from PyQt5.QtWidgets import QWidget, QPushButton, QHBoxLayout, QVBoxLayout, QApplication

#Here we create a class with the "base" class from QWidget
#We are inheriting the functions of the QWidget from this case
class Qtwindowexample(QWidget):

    #Constructor, will be executed upon instantiation of the object
    def __init__(self):
        #Upon self instantiation, we are calling constructor of the QWidget 
        #to set up the bases of the QWidget's object 
        QWidget.__init__(self)

        #Resizing, moving and setting the window
        self.resize(250, 150)
        self.move(300, 300)
        self.setWindowTitle('2 Click buttons!')

        #Here we create the first button - print1button
        #When clicked, it will invoke the printOnce function, and print "Hello world" in the terminal
        self.print1button = QPushButton('Print once!', self)
        self.print1button.clicked.connect(self.printOnce)

        #Here we create the second button - print5button
        #When clicked, it will invoke the printFive function, and print "**Hello world" 5 times in the terminal
        self.print5button = QPushButton('Print five times!', self)
        self.print5button.clicked.connect(self.printFive)

        #Something very familiar!
        #It is the vertical box in Qt5
        self.vbox=QVBoxLayout()

        #Simply add the two buttons to the vertical box 
        self.vbox.addWidget(self.print1button)
        self.vbox.addWidget(self.print5button)

        #Here put the vertical box into the window
        self.setLayout(self.vbox)
        #And now we are all set, show the window!
        self.show()

    #Function that will print Hello world once when invoked
    def printOnce(self):
        print("Hello World!")

    #Function that will print **Hello world five times when invoked
    def printFive(self):
        for i in range(0,5):
            print("**Hello World!")

#Creating the app object, essential for all Qt usage
app = QApplication(sys.argv)
#Create Qtwindowexample(), construct the window and show!
ex = Qtwindowexample()
#app.exec_() is the mainloop of the application
#the sys.exit() is a method to ensure a real exit upon receiving the signal of exit from the app
sys.exit(app.exec_())

```

The preceding code creates two buttons, and each button will invoke an individual function—print `Hello world` once or print `Hello World` five times in the Terminal. Readers should be able to grasp the event handling system from the code easily.

Here is the output:

![](img/addd7eec-dc3d-4c38-9ab5-0ce2c4faf4e7.png)

This is another implementation of the two buttons example from [Chapter 5](https://cdp.packtpub.com/matplotlib_for_python_developers__second_edition/wp-admin/post.php?post=375&action=edit#post_362), *Embedding Matplotlib in GTK+3*, and the goal of this example is to demonstrate the signal handling approach in PyQt 5 in comparison with GTK+3\. Readers should find this fairly similar as we intentionally write it in a way more similar to the example in GTK+3.

Let's try to embed a Matplotlib figure in a Qt window. Be aware that unlike the example in the previous chapter, this figure will be refreshed every second! Therefore, we also use the `QtCore.QTimer()` function in here and invoke the `update_figure()` function as an event-action pair:

```py
#Importing essential libraries
import sys, os, random, matplotlib, matplotlib.cm as cm
from numpy import arange, sin, pi, random, linspace
#Python Qt5 bindings for GUI objects
from PyQt5 import QtCore, QtWidgets
# import the Qt5Agg FigureCanvas object, that binds Figure to
# Qt5Agg backend.
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
from matplotlib.figure import Figure

#The class DynamicCanvas contains all the functions required to draw and update the figure
#It contains a canvas that updates itself every second with newly randomized vecotrs
class DynamicCanvas(FigureCanvas):

    #Invoke upon instantiation, here are the arguments parsing along
    def __init__(self, parent=None, width=5, height=4, dpi=100):
        #Creating a figure with the requested width, height and dpi
        fig = Figure(figsize=(width,height), dpi=dpi)

        #The axes element, here we indicate we are creating 1x1 grid and putting the subplot in the only cell
        #Also we are creating a polar plot, therefore we set projection as 'polar
        self.axes = fig.add_subplot(111, projection='polar')
        #Here we invoke the function "compute_initial_figure" to create the first figure
        self.compute_initial_figure()

        #Creating a FigureCanvas object and putting the figure into it
        FigureCanvas.__init__(self, fig)
        #Setting this figurecanvas parent as None
        self.setParent(parent)

        #Here we are using the Qtimer function
        #As you can imagine, it functions as a timer and will emit a signal every N milliseconds
        #N is defined by the function QTimer.start(N), in this case - 1000 milliseconds = 1 second
        #For every second, this function will emit a signal and invoke the update_figure() function defined below
        timer = QtCore.QTimer(self)
        timer.timeout.connect(self.update_figure)
        timer.start(1000)

    #For drawing the first figure
    def compute_initial_figure(self):
        #Here, we borrow one example shown in the matplotlib gtk3 cookbook
        #and show a beautiful bar plot on a circular coordinate system
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.plot_width = pi / 4 * random.rand(30)
        self.bars = self.axes.bar(self.theta, self.radii, width=self.plot_width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)
        #Here we generate the figure
        self.axes.plot()

    #This function will be invoke every second by the timeout signal from QTimer
    def update_figure(self):
        #Clear figure and get ready for the new plot
        self.axes.cla()

        #Identical to the code above
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.plot_width = pi / 4 * random.rand(30)
        self.bars = self.axes.bar(self.theta, self.radii, width=self.plot_width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)

        #Here we generate the figure
        self.axes.plot()
        self.draw()

#This class will serve as our main application Window
#QMainWindow class provides a framework for us to put window and canvas
class ApplicationWindow(QtWidgets.QMainWindow):

    #Instantiation, initializing and setting up the framework for the canvas
    def __init__(self):
        #Initializing of Qt MainWindow widget
        QtWidgets.QMainWindow.__init__(self)
        self.setAttribute(QtCore.Qt.WA_DeleteOnClose)
        #Instantiating QWidgets object
        self.main_widget = QtWidgets.QWidget(self)

        #Creating a vertical box!
        vbox = QtWidgets.QVBoxLayout(self.main_widget)
        #Creating the dynamic canvas and this canvas will update itself!
        dc = DynamicCanvas(self.main_widget, width=5, height=4, dpi=100)
        #adding canvas to the vertical box
        vbox.addWidget(dc)

        #This is not necessary, but it is a good practice to setFocus on your main widget
        self.main_widget.setFocus()
        #This line indicates that main_widget is the main part of the application
        self.setCentralWidget(self.main_widget)

#Creating the GUI application
qApp = QtWidgets.QApplication(sys.argv)
#Instantiating the ApplicationWindow widget
aw = ApplicationWindow()
#Set the title
aw.setWindowTitle("Dynamic Qt5 visualization")
#Show the widget
aw.show()
#Start the Qt main loop , and sys.exit() ensure clean exit when closing the window
sys.exit(qApp.exec_())
```

Again, the figure in this example will randomize the data and update the figure every 1 second through `QTimer`, shown as follows:

![](img/46a8be92-2ec5-464a-832b-d46b191de1a1.png)

# Introducing QT Creator / QT Designer

The four preceding figures are screenshots of the PyQt 5 window, which will refresh itself every second.

For simple examples, designing the GUI in the Python code can be good enough, but for more complex applications, this solution does not scale.

There are some tools to help you design the GUI for Qt, and one of the most commonly used tools was QT Designer. In the first edition of this book, this section was about GUI making with QT Designer. Since the late QT4 development, QT Designer has merged with QT Creator. In the following example, we would learn how to open the secret QT Designer in QT Creator and create a UI file.

Similar to Glade, we can design the user interface part of the application using the on-screen form and drag-and-drop interface. Then we can connect the widgets with the backend code, where we develop the logic of the application.

First of all, let us show you how to open QT Designer in QT Creator. When you open QT Creator, you will see the following interface:

![](img/0a97bbe9-7a6e-428d-8d67-c82f04d25895.png)

The tricky part is this: Do not create the project by clicking on the New File or Project button in the Creator. Instead, create a New Project:

![](img/9bdc8606-c103-4b4d-a4c2-f8cca343a861.png)

Select Qt in Files and Classes and Qt Designer Form in the middle panel:

![](img/375d6696-bf0f-49f5-a910-c5a59e86367f.png)

There is a range of template selections, such as Widget or Main Window. In our case, we pick Main Window and simply follow through the remaining steps:

![](img/3c04934e-c10b-42c3-afef-e3322fff65e5.png)

And eventually, we will reach the QT Designer interface. Everything you work on here will be put in your desired folder as a UI file:

![](img/994c10ae-9971-44ad-b8d2-0614edde9ac6.png)

Embedding Matplotlib in a GUI made with QT Creator / QT Designer.

To quickly demonstrate how to embed a Matplotlib figure in Qt 5 using QT Creator, let's use the former example and combine it with the scripts generated by QT Creator.

First of all, adjust the Geometry of the MainWindow at the lower right panel; change the Width and Height to 300x300:

![](img/2bd8cd6c-1bb6-41b4-9438-2298cf4936c6.png)

After that, drag a Widget from Container in the left panel to the MainWindow in the middle. Resize it until it fits well in the MainWindow:

![](img/eac1a19b-7034-4c84-a057-1cf1aa509dff.png)

That is it for the basic design! Now save it as a UI file. When you view the UI file, it should display something like this:

```py
<?xml version="1.0" encoding="UTF-8"?>
<ui version="4.0">
 <class>MainWindow</class>
 <widget class="QMainWindow" name="MainWindow">
  <property name="geometry">
   <rect>
    <x>0</x>
    <y>0</y>
    <width>300</width>
    <height>300</height>
   </rect>
  </property>
  <property name="windowTitle">
   <string>MainWindow</string>
  </property>
  <widget class="QWidget" name="centralwidget">
   <widget class="QWidget" name="widget" native="true">
    <property name="geometry">
     <rect>
      <x>20</x>
      <y>10</y>
      <width>261</width>
      <height>221</height>
     </rect>
    </property>
   </widget>
  </widget>
  <widget class="QMenuBar" name="menubar">
   <property name="geometry">
    <rect>
     <x>0</x>
     <y>0</y>
     <width>300</width>
     <height>22</height>
    </rect>
   </property>
  </widget>
  <widget class="QStatusBar" name="statusbar"/>
 </widget>
 <resources/>
 <connections/>
</ui>
```

This file is in XML format, and we need to convert it to a Python file. This can be done simply by using this command:

```py
pyuic5 mainwindow.ui > mainwindow.py
```

And now we will have a Python file like this:

```py
from PyQt5 import QtCore, QtGui, QtWidgets

class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(300, 300)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.widget = QtWidgets.QWidget(self.centralwidget)
        self.widget.setGeometry(QtCore.QRect(20, 10, 261, 221))
        self.widget.setObjectName("widget")
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 300, 22))
        self.menubar.setObjectName("menubar")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "MainWindow"))
```

Note that this is just the framework for the GUI; we still have to add some things in order to make it work.

We must add `init()` to initialize the `UiMainWindow`, as well as link the `DynamicCanvas` to the widget in the middle of the `MainWindow`. Here it goes:

```py
#Replace object to QtWidgets.QMainWindow
class Ui_MainWindow(QtWidgets.QMainWindow):

    #***Instantiation!
    def __init__(self):
        # Initialize and display the user interface
        QtWidgets.QMainWindow.__init__(self)
        self.setupUi(self)

    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(300, 300)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.widget = QtWidgets.QWidget(self.centralwidget)
        self.widget.setGeometry(QtCore.QRect(20, 10, 261, 221))
        self.widget.setObjectName("widget")
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 300, 22))
        self.menubar.setObjectName("menubar")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

        #***Putting DynamicCanvas into the widget, and show the window!
        dc = DynamicCanvas(self.widget, width=5, height=4, dpi=100)
        self.show()

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "MainWindow"))
```

We have added only five lines of code in here. We can simply replace the class `ApplicationWindow` with this, and here is the outcome:

![](img/40b85b90-afe5-479d-96fa-f9e6febca9cd.png)

And here is the complete code generating the preceding figure:

```py
#Importing essential libraries
import sys, os, random, matplotlib, matplotlib.cm as cm
from numpy import arange, sin, pi, random, linspace
#Python Qt5 bindings for GUI objects
from PyQt5 import QtCore, QtGui, QtWidgets
# import the Qt5Agg FigureCanvas object, that binds Figure to
# Qt5Agg backend.
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
from matplotlib.figure import Figure

#The class DynamicCanvas contains all the functions required to draw and update the figure
#It contains a canvas that updates itself every second with newly randomized vecotrs
class DynamicCanvas(FigureCanvas):

    #Invoke upon instantiation, here are the arguments parsing along
    def __init__(self, parent=None, width=5, height=5, dpi=100):
        #Creating a figure with the requested width, height and dpi
        fig = Figure(figsize=(width,height), dpi=dpi)

        #The axes element, here we indicate we are creating 1x1 grid and putting the subplot in the only cell
        #Also we are creating a polar plot, therefore we set projection as 'polar
        self.axes = fig.add_subplot(111, projection='polar')
        #Here we invoke the function "compute_initial_figure" to create the first figure
        self.compute_initial_figure()

        #Creating a FigureCanvas object and putting the figure into it
        FigureCanvas.__init__(self, fig)
        #Setting this figurecanvas parent as None
        self.setParent(parent)

        #Here we are using the Qtimer function
        #As you can imagine, it functions as a timer and will emit a signal every N milliseconds
        #N is defined by the function QTimer.start(N), in this case - 1000 milliseconds = 1 second
        #For every second, this function will emit a signal and invoke the update_figure() function defined below
        timer = QtCore.QTimer(self)
        timer.timeout.connect(self.update_figure)
        timer.start(1000)

    #For drawing the first figure
    def compute_initial_figure(self):
        #Here, we borrow one example shown in the matplotlib gtk3 cookbook
        #and show a beautiful bar plot on a circular coordinate system
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.plot_width = pi / 4 * random.rand(30)
        self.bars = self.axes.bar(self.theta, self.radii, width=self.plot_width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)
        #Here we generate the figure
        self.axes.plot()

    #This function will be invoke every second by the timeout signal from QTimer
    def update_figure(self):
        #Clear figure and get ready for the new plot
        self.axes.cla()

        #Identical to the code above
        self.theta = linspace(0.0, 2 * pi, 30, endpoint=False)
        self.radii = 10 * random.rand(30)
        self.plot_width = pi / 4 * random.rand(30)
        self.bars = self.axes.bar(self.theta, self.radii, width=self.plot_width, bottom=0.0)

        #Here defines the color of the bar, as well as setting it to be transparent
        for r, bar in zip(self.radii, self.bars):
            bar.set_facecolor(cm.jet(r / 10.))
            bar.set_alpha(0.5)

        #Here we generate the figure
        self.axes.plot()
        self.draw()

#Created by Qt Creator!
class Ui_MainWindow(QtWidgets.QMainWindow):
    def __init__(self):
        # Initialize and display the user interface
        QtWidgets.QMainWindow.__init__(self)
        self.setupUi(self)

    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(550, 550)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.widget = QtWidgets.QWidget(self.centralwidget)
        self.widget.setGeometry(QtCore.QRect(20, 10, 800, 800))
        self.widget.setObjectName("widget")
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 300, 22))
        self.menubar.setObjectName("menubar")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

        dc = DynamicCanvas(self.widget, width=5, height=5, dpi=100)
        #self.centralwidget.setFocus()
        #self.setCentralWidget(self.centralwidget)
        self.show()

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "MainWindow"))

#Creating the GUI application
qApp = QtWidgets.QApplication(sys.argv)
#Instantiating the ApplicationWindow widget
aw = Ui_MainWindow()
#Start the Qt main loop , and sys.exit() ensure clean exit when closing the window
sys.exit(qApp.exec_())
```

# Summary

GUI design by using QT Creator/ QT Designer has enough material for a book on its own. Therefore, in this chapter, we aimed to show you just a glimpse of GUI design through PyQt 5\. Upon finishing this chapter, the readers should now understand how to embed a figure in a QWidget, use the layout manager to pack a figure in a QWidget, create a timer, react to events and update a Matplotlib graph accordingly, and use QT Designer to draw a simple GUI for Matplotlib embedding.

We are now ready to learn another GUI library, wxWidgets.
