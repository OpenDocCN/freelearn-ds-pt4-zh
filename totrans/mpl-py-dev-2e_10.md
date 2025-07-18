
# Integrating Data Visualization into the Workflow

We have now come to the concluding chapter of this book. Throughout the course of this book, you have mastered the techniques to create and customize static and animated plots using real-world data in different formats scraped from the web. To wrap up, we will start a mini-project in this chapter to combine the skills of data analytics with the visualization techniques you've learned. We will demonstrate how to integrate visualization techniques in your current workflow.

In the era of big data, machine learning becomes fundamental to ease analytic work by replacing huge amounts of manual curation with automatic prediction. Yet, before we enter model building, **Exploratory Data Analysis** (**EDA**) is always essential to get a good grasp of what the data is like. Constant review during the optimization process also helps improve our training strategy and results.

High-dimensional data typically requires special processing techniques to be visualized intuitively. Statistical methods such as **Principle Component Analysis** (**PCA**) and **t-Distributed Stochastic Neighbor Embedding** (**t-SNE**) are important skills in reducing the dimension of data for effective visualization.

As a showcase, we will demonstrate the use of various visualization techniques in a workflow involving recognizing handwritten digits using a **Convolutional Neural Network** (**CNN**).

One important note is that we do not intend to illustrate all the mathematics and machine learning approaches in detail in this chapter. Our goal is to visualize some of the processes in between. Hopefully, readers will appreciate the importance of exploring processes such as the `loss` function when training a CNN, or visualizing the dimension reduction results with different parameters.

# Getting started

Recall the MNIST dataset we briefly touched upon in [Chapter 04](456b0dc2-84d5-40f9-bf63-1ab4635cbac8.xhtml), *Advanced Matplotlib*. It contains 70,000 images of handwritten digits, often used in data mining tutorials as *Machine Learning 101*. We will continue using a similar image dataset of handwritten digits for our project in this chapter.

We are almost certain that you had already heard about the popular keywords—deep learning or machine learning in general—before starting with this course. That's why we are choosing it as our showcase. As detailed concepts in machine learning, such as **hyperparameter tuning** to optimize performance, are beyond the scope of this book, we will not go into them. But we will cover the model training part in a cookbook style. We will focus on how visualization helps our workflow. For those of you interested in the details of machine learning, we recommend exploring further resources that are largely available online.

# Visualizing sample images from the dataset

Data cleaning and EDA are indispensable components of data science. Before we begin analyzing our data, it is important to understand some basic properties of what we have input. The dataset we are using comprises standardized images with regular shapes and normalized pixel values. The features are simple, thin lines. Our goal is straightforward as well, to recognize digits from images. Yet, in many cases of real-world practice, the problems can be more complicated; the data we collect is going to be raw and often much more heterogeneous. Before tackling the problem, it is usually worth the time to sample a small amount of input data for inspection. Imagine training a model to recognize Ramen just to get you drooling ;). You will probably take a look at some images to decide what features make a good input sample to exemplify the presence of the bowl. Besides the initial preparatory phase, during model building taking out some of the mislabeled samples to examine may also help us devise strategies for optimization.

In case you wonder where the Ramen idea comes from, a data scientist named Kenji Doi created a model to recognize in which restaurant branch a bowl of Ramen was made. You may read more on the Google Cloud Big Data and Machine Learning Blog post on [https://cloud.google.com/blog/big-data/2018/03/automl-vision-in-action-from-ramen-to-branded-goods](https://cloud.google.com/blog/big-data/2018/03/automl-vision-in-action-from-ramen-to-branded-goods).

# Importing the UCI ML handwritten digits dataset

While we will be using the MNIST dataset as in [Chapter 04](https://cdp.packtpub.com/matplotlib_for_python_developers__second_edition/wp-admin/post.php?post=338&action=edit#post_73), *Advanced Matplotlib* (since we will be demonstrating visualization along with model building in machine learning), we will take a shortcut to speed up the training process. Instead of using the 60,000 images with 28x28 pixels each, we will import another similar dataset of 8x8-pixel images from the scikit-learn package. 

This dataset is obtained from the University of California, Irvine Machine Learning Repository, found at [http://archive.ics.uci.edu/ml/datasets/Optical+Recognition+of+Handwritten+Digits](http://archive.ics.uci.edu/ml/datasets/Optical+Recognition+of+Handwritten+Digits). It is a preprocessed version of images of digits written by 43 people, with 30 and 13 contributing to the training and testing set respectively. The preprocessing method is described in M. D. Garris, J. L. Blue, G. T. Candela, D. L. Dimmick, J. Geist, P. J. Grother, S. A. Janet, and C. L. Wilson, NIST Form-Based Handprint Recognition System, NISTIR 5469, 1994.

The code to import the dataset is as follows:

```py
from sklearn.datasets import load_digits
```

To begin, let's store our dataset into a variable. We will call it `digits` and reuse it throughout the chapter:

```py
digits = load_digits()
```

Let's see what is loaded by the `load_digits()` function by printing out the variable `digits`:

```py
print(type(digits))
print(digits)
```

The type of `digits` is `<class 'sklearn.utils.Bunch'>`, which is specific to loading sample dataset.

As the output for `print(digits)` is somewhat long, we will show its beginning and end in two screenshots:

![](img/913f538a-09d1-4fb8-99c3-78856b08da5a.png)

The following screenshot shows the tail of the output:

![](img/60f31ffd-78ae-406b-837e-c8148acb223c.png)

We can see that there are five members within the class `digits`:

*   `'data'`: Pixel values flattened into 1D NumPy arrays
*   `'target'`: A NumPy array of identity labels of each element in the dataset
*   `'target_names'`: A list of unique labels existing in the dataset—integers 0-9 in this case
*   `'images'`: Pixel values reshaped into 2D NumPy arrays in the dimension of images
*   `'DESCR'`: Description of the dataset

Besides having smaller image dimensions than MNIST, this dataset also has far fewer images. So, how many are there in total? We get the dimensions of a NumPy array in a tuple of dimensions by `nd.shape`, where `nd` is the array. Hence, to inquire about the shape of `digits.image`, we call:

```py
print(digits.images.shape)
```

We get `(1797, 8, 8)` as result.

You may wonder why the number is so peculiar. If you have particularly sharp eyes, you might have seen that there are 5,620 instances in the description. In fact, the description is retrieved from the archive web page. The data we have loaded is actually the testing portion of the full dataset. You may also download the plain text equivalent from [http://archive.ics.uci.edu/ml/machine-learning-databases/optdigits/optdigits.tes](http://archive.ics.uci.edu/ml/machine-learning-databases/optdigits/optdigits.tes).

If you are interested in getting the full MNIST dataset, scikit-learn also offers an API to fetch it:
`from sklearn.datasets import fetch_mldata mnist = fetch_mldata('MNIST original', data_home=custom_data_home)`

# Plotting sample images

Now that we know more about the background of our input data, let's plot out some sample images.

# Extracting one sample each of digits 0-9

To get a feel for what the images look like, we want to extract one image of each digit for inspection. For this, we need to find out where the digits are. We can do so by tracking down the right indices (the data labels) with `digits.target` then we write a few lines of code to call `10` images:

```py
indices = []
for i in range(10):
    for j in digits.target:
        if i==j:
            indices.append(j)
            break
print(indices)
```

Interestingly, `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]` is returned as output, which means the first `10` samples happen to be in numerical order. Is it by chance or is the data ordered? We need to check as the sample distribution can impact our training performance.

# Examining the randomness of the dataset

Because showing all 1,797 data points will make the plot too dense for any meaningful interpretation, we will plot the first `200` data points to check:

```py
import matplotlib.pyplot as plt
plt.scatter(list(range(200)),digits.target[:200])
plt.show()
```

Here we get a scatter plot of the sample distribution. Not quite random, is it? The 0-9 digits are ordered and repeated three times. We also see a repetition of patterns from around the *125^(th)* sample. The structure of the data hints at randomization before our training of machine learning model later. For now, we will first take it as-is and continue with our image inspection:

![](img/276c26f9-b086-4787-b2b8-ea51b1116162.png)

# Plotting the 10 digits in subplots

We will align the images in a grid of two rows and five columns. We first set the rectangular canvas by `plt.figure()`. For each sample image of a digit, we define the axes with the `plt.subplot()` function, and then call `imshow()` to show the arrays of color values as images. Recall that the colormap `'gray_r'` plots values in grayscale from white to black with values from zero to maximum. As there is no need for *x* and *y* tick labels to show the scales, we will remove them by passing an empty list to `plt.xticks()` and `plt.yticks()` to remove the clutter. Here is the code snippet to do so:

```py
import matplotlib.pyplot as plt

nrows, ncols = 2, 5
plt.figure(figsize=(6,3))

for i in range(ncols * nrows):
    ax = plt.subplot(nrows, ncols, i + 1)
    ax.imshow(digits.images[i],cmap='gray_r')
    plt.xticks([])
    plt.yticks([])
    plt.title(digits.target[i])
```

As you can see from the following figure, the images are a bit blurry to human eyes. But, believe it or not, there are enough details for the algorithm to extract features and differentiate between each digit. We will observe this together along with our workflow:

![](img/d98d2ebf-0141-4136-b077-9844f371e7b9.png)

# Exploring the data nature by the t-SNE method

After visualizing a few images and glimpsing of how the samples are distributed, we will go deeper into our EDA.

Each pixel comes with an intensity value, which makes 64 variables for each 8x8 image. The human brain is not good at intuitively perceiving dimensions higher than three. For high-dimensional data, we need more effective visual aids. 

Dimensionality reduction methods, such as the commonly used PCA and t-SNE, reduce the number of input variables under consideration, while retaining most of the useful information. As a result, the visualization of data becomes more intuitive.

In the following section, we will focus our discussion on the t-SNE method by using the scikit-learn library in Python.

# Understanding t-Distributed stochastic neighbor embedding 

The t-SNE method was proposed by van der Maaten and Hinton in 2008 in the publication *Visualizing Data using t-SNE*. It is a nonlinear dimension reduction method that aims to effectively visualize high-dimensional data. t-SNE is based on probability distributions with random walk on neighborhood graphs to find the structure within the data. The mathematical details of t-SNE are beyond the scope of this book, and readers are advised to read the paper for more details.

In short, t-SNE is a way to capture non-linear relationships in a high-dimensional data. This is particularly useful when we are trying to extract features from a high-dimensional matrix such as image processing, biological data, and network information. It enables us to reduce high-dimensional data to two or three dimensions; one interesting feature of t-SNE is that it is stochastic, indicating that the final results it shows each time will be different, but still they are all equally correct. Therefore, in order to get the best performance in t-SNE dimension reduction, it is advisable to first perform PCA dimension reduction on the big dataset, and then incorporate the PCA dimensions into t-SNE for subsequent dimension reduction. Thus, you get more consistent and replicable results. 

# Importing the t-SNE method from scikit-learn

We will implement the t-SNE method by loading the `TSNE` function from scikit-learn, as follows:

```py
from sklearn.manifold import TSNE
```

There are a few hypervariables that the user has to set upon running t-SNE, which include:

*   `'init'` : Initialization of embedding
*   `'method'`: `barnes_hut` or exact
*   `'perplexity'`: Default `30`
*   `'n_iter'`: Default `1000`
*   `'n_components'`: Default `2`

Going into the mathematical details of individual hypervariables would be a chapter on its own, but we do have suggestions on what the parameters should be in general. For `init`, it is recommended to use `'pca'` with the reason given before. For method, `barnes_hut` will be faster and gives very similar results if the provided dataset is not highly similar intrinsically. For perplexity, it reflects on the focus in teasing out local and global substructures of the data. `n_iter` indicates the number of iterations that you will run through the algorithm, and `n_components = 2` indicates that the final outcome is a two-dimensional space.

To track the time use for rounds of experiments, we can use the cell magic `%%timeit` in the Jupyter notebook to track the time needed for a cell to run.

# Drawing a t-SNE plot for our data

Let's first reorder the data points according to the handwritten numbers:

```py
import numpy as np
X = np.vstack([digits.data[digits.target==i]for i in range(10)])
y = np.hstack([digits.target[digits.target==i] for i in range(10)])
```

`y` will become `array([0, 0, 0, ..., 9, 9, 9])`.

Note that the t-SNE transformation can take minutes to compute on a regular laptop, and the `tSNE` command can be simply run as follows. We will first try running t-SNE with `250` iterations:

```py
#Here we run tSNE with 250 iterations and time it
%%timeit
tsne_iter_250 = TSNE(init='pca',method='exact',n_components=2,n_iter=250).fit_transform(X)
```

Let's draw a scatter plot to see how the data cluster:

```py
#We import the pandas and matplotlib libraries
import pandas as pd
import matplotlib
matplotlib.style.use('seaborn')
#Here we plot the tSNE results in a reduced two-dimensional space
df = pd.DataFrame(tsne_iter_250)
plt.scatter(df[0],df[1],c=y,cmap=matplotlib.cm.get_cmap('tab10'))
plt.show()
```

We can see that the clusters are not well separated at `250` iterations:

![](img/0d7d2b79-8a06-4de2-b6ac-6eb559ade1c1.png)

Let's now try running with `2000` iterations:

```py
#Here we run tSNE for 2000 iteractions
tsne_iter_2000 = TSNE(init='pca',method='exact',n_components=2,n_iter=2000).fit_transform(X)
#Here we plot the figure
df2 = pd.DataFrame(tsne_iter_2000)
plt.scatter(df2[0],df2[1],c=y,cmap=matplotlib.cm.get_cmap('tab10'))
plt.show()
```

As seen from the following screenshot, the samples appear as `10` distinct blots of clusters. By running `2000` iterations, we have obtained far more satisfying results:

![](img/d5ae44f2-a6cf-4e9f-ad15-0c5598b077d7.png)

# Creating a CNN to recognize digits

In the following section, we will use Keras. Keras is a Python library for neural networks and provides a high-level interface to TensorFlow libraries. We do not intend to give a complete tutorial on Keras or CNN, but we want to show how we can use Matplotlib to visualize the `loss` function, `accuracy`, and outliers of the results. 

Readers who are not familiar with machine learning should be able to go through the logic of the remaining chapter and hopefully understand why visualizing the `loss` function, `accuracy`, and outliers of the results is important in fine-tuning the CNN model. 

Here is a snippet of code for the CNN; the most important part is the evaluation section after this!

```py
# Import sklearn models for preprocessing input data
from sklearn.model_selection import train_test_split 
from sklearn.preprocessing import LabelBinarizer

# Import the necessary Keras libraries
from keras.models import Sequential
from keras.layers import Dense, Dropout, Flatten
from keras.layers.convolutional import Convolution2D, MaxPooling2D
from keras import backend as K
from keras.callbacks import History 

# Randomize and split data into training dataset with right format to feed to Keras
lb = LabelBinarizer()
X = np.expand_dims(digits.images.T, axis=0).T
y = lb.fit_transform(digits.target)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=100)

# Start a Keras sequential model
model = Sequential()

# Set input format shape as (batch, height, width, channels)
K.set_image_data_format('channels_last') # inputs with shape (batch, height, width, channels)

model.add(Convolution2D(filters=4,kernel_size=(3,3),padding='same',input_shape=(8,8,1),activation='relu'))
model.add(MaxPooling2D(pool_size=(2,2)))

# Drop out 5% of training data in each batch
model.add(Flatten())
model.add(Dropout(0.05))
model.add(Dense(10, activation= 'softmax'))

# Set variable 'history' to store callbacks to track the validation loss
history = History()

# Compile the model
model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])

# Fit the model and save the callbacks of validation loss and accuracy to 'history'
model.fit(X_train,y_train, epochs=100, batch_size= 128, callbacks=[history])
```

# Evaluating prediction results with visualizations

We have specified the callbacks that store the loss and accuracy information for each epoch to be saved as the variable `history`. We can retrieve this data from the dictionary `history.history`. Let's check out the dictionary `keys`:

```py
print(history.history.keys())
```

This will output `dict_keys(['loss', 'acc'])`.

Next, we will plot out the `loss` function and `accuracy` along epochs in line graphs:

```py
import pandas as pd
import matplotlib
matplotlib.style.use('seaborn')

# Here plots the loss function graph along Epochs
pd.DataFrame(history.history['loss']).plot()
plt.legend([])
plt.xlabel('Epoch')
plt.ylabel('Loss')
plt.title('Validation loss across 100 epochs',fontsize=20,fontweight='bold')
plt.show()

# Here plots the percentage of accuracy along Epochs
pd.DataFrame(history.history['acc']).plot()
plt.legend([])
plt.xlabel('Epoch')
plt.ylabel('Accuracy')
plt.title('Accuracy loss across 100 epochs',fontsize=20,fontweight='bold')
plt.show()
```

Upon training, we can say the `loss` function is decreasing, accompanied by an increase in accuracy, which is something we are delighted to see. Here is the first graph showing the `loss` function:

![](img/9f14a248-9cf2-47c6-ae6f-b4077351bcbb.png)

The next graph shows the changes in **Accuracy** across **Epoch**:

![](img/65bc06af-bb45-435a-a020-0dfaca639b24.png)

From these screenshots, we can observe a general trend for decreasing loss and increasing accuracy with each epoch along the training process, with alternating ups and downs. We can observe whether the final accuracy or the learning rate is desirable and optimize the model where necessary.

# Examining the prediction performance for each digit 

We first revert the labels from one-hot format back to lists of integers:

```py
y_test1 = model.predict(X_test)
y_test1 = lb.fit_transform(np.round(y_test1))
y_test1 = np.argmax(y_test1, axis=1)
y_test = np.argmax(y_test, axis=1)
```

We will extract the indices of mislabeled images, and use them to retrieve the corresponding true and predicted labels:

```py
import numpy as np
mislabeled_indices = np.arange(len(y_test))[y_test!=y_test1]
true_labels = np.asarray([y_test[i] for i in mislabeled_indices])
predicted_labels = np.asarray([y_test1[i] for i in mislabeled_indices])
print(mislabeled_indices)
print(true_labels)
print(predicted_labels)
```

The output is as follows, with NumPy arrays of the indices, true and predicted labels of the array of mislabeled images:

```py
[  1   8  56  97 117 186 188 192 198 202 230 260 291 294 323 335 337]
[9 7 8 2 4 4 2 4 8 9 6 9 7 6 8 8 1]
[3 9 5 0 9 1 1 9 1 3 0 3 8 8 1 3 2]
```

Let's count how many samples are mislabeled for each digit. We will store the counts into a list:

```py
mislabeled_digit_counts = [len(true_labels[true_labels==i]) for i in range(10)]
```

Now, we will plot a bar chart of the ratio of mislabeled samples for each digit:

```py
# Calculate the ratio of mislabeled samples
total_digit_counts = [len(y_test[y_test==i]) for i in range(10)]
mislabeled_ratio = [mislabeled_digit_counts[i]/total_digit_counts[i] for i in range(10)]

pd.DataFrame(mislabeled_ratio).plot(kind='bar')
plt.xticks(rotation=0)
plt.xlabel('Digit')
plt.ylabel('Mislabeled ratio')
plt.legend([])
plt.show()
```

This code creates a bar chart showing the ratio of each digit mislabeled by our model:

![](img/4337625d-7d76-4b64-98f2-9a661335fe80.png)

From the preceding figure, we see that the digit 8 is the most easily mis-recognized digit by our model. Let's find out why.

# Extracting falsely predicted images

Similar to what we did at the beginning of this chapter, we will draw the digit images out. This time, we pick out the mislabeled ones because they are the ones we're concerned about. We will again pick 10 images and put them in a grid of subplots. We write the true label in green at the bottom as `xlabel` and the false label predicted in `red` as the `title` at the top for each image in a subplot:

```py
import matplotlib.pyplot as plt
nrows, ncols = 2, 5
plt.figure(figsize=(6,3))
for i in range(ncols * nrows):
    j = mislabeled_indices[i]
    ax = plt.subplot(nrows, ncols, i + 1)
    ax.imshow(X_test[j].reshape(8,8),cmap='gray_r')
    plt.xticks([])
    plt.yticks([])
    plt.title(y_test1[j],color='red')
    plt.xlabel(y_test[j],color='green')
plt.show()
```

Let's see how the images look. Does the handwriting look more like the true or falsely predicted label to you?

![](img/15efdaaa-3fc6-484a-8780-8f1cd93f3501.png)

We can observe that, for some images, it is quite difficult to identify the true label at the 8x8 resolution even with the naked eye, such as the number **4** in the middle of the bottom row. However, the leftmost number **4** on the same row should be legible enough for humans to recognize. From here, we can estimate the maximum possible improvement in accuracy by additional training and optimizing the model. This will guide our decision on whether it is worth while to expend further effort to improve our model, or what kind of training data to get or generate next to obtain better results.

Meanwhile, notice that the training and testing datasets generally contain samples from different distributions. It is left for an exercise for you to repeat the process by downloading the actual training dataset from UCI ML, using the larger MNIST dataset (by downloading it via Keras, or even scraping or creating your own).

# Summary

Congratulations! You have now completed this chapter as well as the whole book. In this chapter, we integrated various data visualization techniques along with an analytic project workflow, from the initial inspection and exploratory analysis of data, to model building and evaluation. Give yourself a huge round of applause, and get ready to leap forward into the journey of data science!