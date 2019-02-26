# Welcome to the Tutorial (and Github)!
For my MKTG 447 Project group members and classmates to see step-by-step the data acquisition, manipulation, and part #1 of cleansing process for the controversial Gilette Ad Twitter Scrape using 2 packages. I appreciate any feedback from the community.

## Some Notes Before the Tutorial
The best way to follow is to follow-along with the code! 

To get started, download Python and Anaconda distribution for Python 3.7. Once finished, launch Jupyter Notebook. 

You can access and upload both notebooks from Thomas and myself from this repository so you can run the code on your own. Remember to install any packages that are not available on your machine already.

Link: https://www.python.org/downloads/
Link: https://www.anaconda.com/distribution/

Lastly, Git is a version control system, or "a tool to manage source code history." Github is the service that hosts Git repositories or projects (Stack Overflow). 

Otherwise you can just follow along with this README.

### What is Text Analytics? 

Essentially, deriving insights from text, also considered unstructured data (not defined by rows and columns). To harness these insights from text into something that can be calculated, you will need to do some programming and understand some linguistic concepts.

### What is Natural Language Processing (NLP)?

"Natural language processing (NLP) is a branch of artificial intelligence that helps computers understand, interpret and manipulate human language" (SAS). Without NLP, we would not be able to find sentiment of tweets, nor a frequency distribution of words. The Python package that utilizes statistical natural language processing is NLTK - or the Natural Language Toolkit.

### What is Twitter-API? 

APIs (Application Programming Interfaces) is basically a set of access points to an app that can access some data from the website's database given certain parameters. 

The Twitter API allows developers to bypass the website interface, and still access the features of Twitter such as posting a tweet, finding a tweet, and so on. 

For our purposes, we are not necessarily "web scraping" because we are getting the data through the API, and not hammering our code using html parsers like BeautifulSoup, but we are retrieving tweets for our project via the API.

To create a Twitter Developer account, see this link: https://developer.twitter.com/en/docs/basics/getting-started

## Let's get started using Python-Twitter Scraping Pt. 1

### First - what is Jupyter Notebook?

Jupyter notebook is an open-source web application and "notebook that integrates code and its output into a single document that combines visualizations, narrative text, mathematical equations, and other rich media" (Dataquest.com). This is where we'll code.

### Installing

Before you start, you want to make sure you have the necessary libraries. If you don't have Python-Twitter, then you will have to install it using the `pip` command. Always good to `#comment` to make your code readable. 

This is how you install python packages into Jupyter Notebook.

```python
#Check if you have pip

pip

#The package we will be using to interface with Twitter

pip install python-twitter
```
### Importing Libraries

If you have already installed the packages you need, then it's time to import them into your notebook to use! Naturally, this would be your first or second cell in your notebook. It's a good practice to import numerous libraries, maybe some you don't think you need now, but could eventually deem useful to you later.

We will be using `python-twitter`, `re` or regular expressions, `string`, and `nltk`. 

```python
#From the python-twitter package. A Python wrapper around the Twitter API. This library provides a pure Python interface for the Twitter API. Twitter exposes a web services API and this library is intended to make it even easier for Python programmers to use.

import twitter

#The package we will be using to interface with Twitter

pip install python-twitter
```


