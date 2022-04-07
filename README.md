<p align="center"><img width=30% src="https://github.com/darabigdata/IDWBotswana/blob/master/media/00000022.jpg"></p>


# Challenge 2: Google Image Web-scraping and Classification

In this challenge you will learn how to web-scrape images from Google and use them to train/test a Machine Leaning (ML) model. The aim is to come up with a image classification problem (cats vs dogs, people vs trees, Trump vs an orange cheeto etc), web-scrape the images and then use ML for the classification.

### What's in the repo?

* **Classify_Zebra_Images.ipynb**
    * *A jupyter notebook that implements simple machine learning to identify images of zebras*
* **zebra_urls.txt**
    * *A list of urls of pictures of zebras from Google Image Search*
* **download_images.py**
    * *Code to download images from a list of urls*
* **get_random_images.py**
    * *Code to randomly select pictures from the [Caltech-256 Dataset](http://www.vision.caltech.edu/Image_Datasets/Caltech256/)*
* **zebra**
    * *A directory of ~400 pictures that contain zebras*
* **nozebra**
    * *A directory of ~400 pictures that don't contain zebras*

### Dependencies

* [matplotlib](https://matplotlib.org/)
* [numpy](www.numpy.org/)
* [scipy](https://www.scipy.org/)
* [sklearn](scikit-learn.org/)
* [sklearn-image](https://scikit-image.org/)
* [requests](docs.python-requests.org/en/master/)

------

## Simple Web-scraping and Classification Tutorial

This tutorial is devided into three parts:
1. Image Webscraping: Obtaining a text file of the image urls that will make up the training and test datasets
2. Download all the images: Using python to download and perfom simple checks on the images
3. Image classification: Use Scikit learn to perform image classification



### Step 1: Image Webscraping

For this challenge you're going to need to build your own library of training data. For image data one of the best places to get this kind of data is Google Images. So in this tutorial we'll use Google Images to web-scrape a database of images. The instructions for this step are a simplified version of the excellent blog post [here](https://www.pyimagesearch.com/2017/12/04/how-to-create-a-deep-learning-dataset-using-google-images/). To follow this tutorial you'll need to use Google Chrome, but there are also many nice ways of scraping data from the web using the Python requests library and the BeautifulSoup library, e.g. [here](https://allofyourbases.com/2017/10/08/web-scraping-youtube-in-python/).

1. Open Chrome and navigate to google image search. Now enter your search, e.g. "Zebra"
2. Open the Developer console: either use CTRL+SHIFT+I or go to 'View' --> 'Developer' --> 'Javascript Console'. 
3. The next step is to start scrolling! Keep scrolling until you have found all relevant images to your query.
4. Next is to grab all the urls of the images in your scroll. In the console enter the following commands:

```javascript
// pull down jquery into the JavaScript console
var script = document.createElement('script');
script.src = "https://ajax.googleapis.com/ajax/libs/jquery/2.2.0/jquery.min.js";
document.getElementsByTagName('head')[0].appendChild(script);
```
```javascript
// grab the URLs
var urls = $('.rg_di .rg_meta').map(function() { return JSON.parse($(this).text()).ou; });
```

```javascript
// write the URls to file (one per line)
var textToSave = urls.toArray().join('\n');
var hiddenElement = document.createElement('a');
hiddenElement.href = 'data:attachment/text,' + encodeURI(textToSave);
hiddenElement.target = '_blank';
hiddenElement.download = 'zebra_urls.txt';
hiddenElement.click();
```
The last step will download a text file named: 'zebra_urls.txt'.

### Step 2: Download all the images

This repo includes a [script](https://github.com/darabigdata/IDWBotswana/blob/master/CHALLENGE-2/download_images.py) to download all the images listed in the url file. You can also always write your own!

The script provided here takes two arguments: (1) the url list file, and (2) the location of the directory where you want the images to be stored. You can run it like this:

```bash
> python download_images.py -u zebra_urls.txt -o ./ZEBRA/
```


For classification we're also going to need a set of images that don't contain our target class, i.e. images that are NOT of zebras. A good online database for random images is the [Caltech-256 Dataset](http://www.vision.caltech.edu/Image_Datasets/Caltech256/), it contains about 30,000 images grouped into categories. You can build a "not zebra" dataset by randomly sampling images from there (make sure you avoid images from the zebra category!). Remember to randomly sample approximately the same number of "not zebra" images as "zebra" images, otherwise you'll end up with a [class imbalance problem](https://towardsdatascience.com/dealing-with-imbalanced-classes-in-machine-learning-d43d6fa19d2). 

### Step 3: Image classification

There are many different approaches to image classification. One heavily used method is Convolutional Neural Networks (CNNs) and there's a good example of how to implement a CNN using the [keras library](https://keras.io/) in [this blog](https://www.pyimagesearch.com/2017/12/11/image-classification-with-keras-and-deep-learning/) and [this blog](https://www.pyimagesearch.com/2018/04/16/keras-and-convolutional-neural-networks-cnns/).

In this repo we've provided an example [classifier](https://github.com/darabigdata/IDWBotswana/blob/master/CHALLENGE-2/Classifying_Zebra_Images.ipynb) that uses a combination of [Gabor filters](http://scikit-image.org/docs/dev/auto_examples/features_detection/plot_gabor.html) and [Support Vector Machines (SVMs)](https://medium.com/machine-learning-101/chapter-2-svm-support-vector-machine-theory-f0812effc72). 

-----

This tutorial is based on a [JBCA hack challenge](https://github.com/hrampadarath/JBCA_Hack_Night_Dec/tree/master/google_images_webscraping)