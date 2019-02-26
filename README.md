# Welcome to the Tutorial (and Github)! Pt. 1
For my MKTG 447 Project group members and classmates to see step-by-step the data acquisition, manipulation, and part #1 of cleansing process for the controversial **Gilette Ad Twitter Scrape**. I appreciate any feedback from the community.

## Some Notes Before the Tutorial
The best way to follow is to follow-along with the code! 

To get started, download **Python** and **Anaconda** distribution for Python 3.7. Once finished, launch **Jupyter Notebook**. 

You can access and upload both notebooks from Thomas and myself from this repository so you can run the code on your own. Remember to install any packages that are not available on your machine already.

Python: https://www.python.org/downloads/

Anaconda: https://www.anaconda.com/distribution/

Lastly, **Git** is a version control system, that manages source code history. **Github** is the hosting service for Git repositories.

### What is Text Analytics? 

Essentially, deriving insights from text, also considered unstructured data (not defined by rows and columns). To harness these insights from text into something that can be calculated, you will need to do some programming and understand some linguistic concepts.

### What is Natural Language Processing (NLP)?

"Natural language processing (NLP) is a branch of artificial intelligence that helps computers understand, interpret and manipulate human language" (SAS). Without NLP, we would not be able to find sentiment of tweets, nor a frequency distribution of words. The Python package that utilizes statistical natural language processing is NLTK - or the **Natural Language Toolkit.**

### What is Twitter-API? 

APIs (Application Programming Interfaces) is basically a set of access points to an app that can access some data from the website's database given certain parameters. 

The **Twitter API** allows developers to bypass the website interface, and still access the features of Twitter such as posting a tweet, finding a tweet, and so on. 

For our purposes, we are not necessarily "web scraping" because we are getting the data through the API, and not hammering our code using html parsers like BeautifulSoup, but we are retrieving tweets for our project via the API.

To create a Twitter Developer account, see this link: https://developer.twitter.com/en/docs/basics/getting-started

For our project, we are going to grab tweets that discuss the controversial Gillette Ad, calculate sentiment, and run a model.

## Let's Get Started using Python-Twitter Scraping Pt. 1

### First - What is Jupyter Notebook?

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
#From python-twitter, a Python wrapper around the Twitter API. 
#This library provides a pure Python interface for the Twitter API. 
#Twitter exposes a web services API and this library is intended to make it even easier to use.

import twitter

#from the regular expression *re* module. A regular expression is a special sequence of characters that 
#helps you match or find other strings or sets of strings, using a specialized syntax held in a pattern. 

import re

#from the string package that uses common string operations such as lowercasing or uppercasing string
#another way to think of string is the text of the tweet, or characters, or words

import string

#nltk includes many useful packages

import nltk 

#Tokenization is the act of breaking up a sequence of strings into pieces such as words, keywords, phrases
#symbols and other elements called tokens.

from nltk.tokenize import word_tokenize #tokenizes and breaks down tokens into words
from nltk.stem import WordNetLemmatizer #lemmas are the canonical form of a set of words. The form of a word
#that appears at the beginning of a dictionary or glossary entry. For example run, ran, and running all 
#have the semantic representation of run
from nltk.tokenize import sent_tokenize #tokenizes and breaks down tokens into sentences *what we will be using*

```

With **any** language, it's important to understand the **documentation** to familiarize yourself with what you're using.

- Docs for **python** *general* : https://www.python.org/doc/

- Docs for **python-twitter**: https://github.com/bear/python-twitter

- Docs for **re**: https://docs.python.org/3/library/re.html

- Docs for **string**: https://docs.python.org/3/library/string.html

- Docs for **nltk** resources: http://www.nltk.org/ &&  https://www.nltk.org/book/ch01.html

### Verifying Credentials

Similarily to Tweepy and Twython, Python-Twitter also needs to verify your credentials via your consumer key and secret, as well as your token key and token secret. Everyone with a developer account is given access to the API using the 4 keys.

```python
#Feel free to use your own credentials

#Find out your credentials: https://developer.twitter.com/en/docs/basics/authentication/guides/access-tokens.html

api = twitter.Api(consumer_key="   ", #plug in your keys
  consumer_secret="  ",
  access_token_key="  ",
  access_token_secret="   ")

#You can verify them after here

print(api.VerifyCredentials())
```
### Let's Look at What @Gillette Has to Say

Python-Twitter allows you use to retrieve statuses by typing in a Twitter handle using the `GetUserTimeline` function. As you'll see in the code, we assign `statuses` as a **list** of Gillette's 100 most recent tweets. However, to access the elements in the list, you have to use a *list comprehension*. 

Read here: https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions

```python

#When printing a list, you must iterate through the list to get all the elements in it.
#This is represented in s.text FOR s in statuses. This is called a list comprehension

#Let's see what the people at Gillette are saying first and compare it to the public opinion
statuses = api.GetUserTimeline(screen_name="Gillette", count=100)
print([s.text for s in statuses])

```

Here are some of the results

```python

["@username We're so happy to hear that, Crayton. We believe in the best in men and want to use our platform to… 
'@username Thank you! We appreciate your support.', 
"@username We're so glad you liked it, Aviva. Men everywhere are already working to re-write the rules...
"@username We appreciate your kind words, Jimmy. We're happy to hear you liked our Pure Shave Gel...]

```
Notice the tweets within the brackets? The brackets represent the start and end of the list, while the tweets are strings inside.

### Let's Look at What *Everyone Else* Has to Say

Everyone has their own opinion about the ad, but what are users saying about it on Twitter?

#### Let's Strip out Some Emojis First

This is where understanding more the `re` library and its packages are important. Emojis are based on unicode characters, which for our purposes won't be helpful to our model (creating a binary label if an emoji is present is another story). So, knowing their codes will be important in finding out how to strip them from our tweets. `def` here defines a function to strip the emoji with one argument `text`.

```python

#Courtesy of the good people of Stack Overflow: http://stackoverflow.com/a/13752628/6762004
RE_EMOJI = re.compile('[\U00010000-\U0010ffff]', flags=re.UNICODE) #this compiles a pretty good range of emojis

#Full emoji list: https://unicode.org/emoji/charts/full-emoji-list.html

def strip_emoji(text): #def defines a new function you create, you can call it anything as long as you pass an
                       #argument in it
    return RE_EMOJI.sub(r'', text) #that function will then return the desired output, which is to substitute
    #them as blanks

print(strip_emoji('🙄🤔'))

```

### Putting it all Together

Another function is Python-Twitter is to GetSearch, which grabs hashtags or keywords of your choice. This is what we will
be using for our purposes. Here we are finding #gillette, getting english only "en" tweets, and about 100 of them. 

```python
search = api.GetSearch("#gillette", lang="en",count=100) #search is a list of tweets, similar to last time

```

However here we need to create a `for` loop in our list to iterate through the elements to do the other tasks we don't want
to do ourselves :D, such as cleaning! Again, here is where NLTK + regular expressions + the string module come into play.

Read more about for loops: https://www.w3schools.com/python/python_for_loops.asp

(For my group) **Take your time analyzing this one before moving on...**

```python

for t in search: #t could be literally called anything, like x, since its calling the list element of search

    tweets = t.text.lower() #defining tweets as a new list that lowers all the text to lowercase with string
    
    tweets = re.sub(r"http\S+", "", tweets) #using regular expressions to replace more symbols for blanks
    
    tweets = tweets.replace("…","") #The replace() method returns a copy of the string where all 
    #occurrences of a substring is replaced with another substring. In this case ... with blanks
    
    tweets = strip_emoji(tweets) #calling the strip_emoji function on the tweets
    
    sentences = sent_tokenize(tweets.replace('\n',' ')) #tokenizes tweets into sentences, replaces the line 
    #break '\n' with blank
    
    clean_words = [word for word in sentences if word not in set(string.punctuation)] #another list comprehension
    #in the sentences list that will clean out the words with string punctuation and return clean_words
    
    characters_to_remove = ["''",'``','...', "'rt",'"rt', '#','@',','] #creating another list with other characters
    #to remove like quotes and more. put things in double quotes when a quote is used in the characters removed
    
    clean_words = [word for word in clean_words if word not in set(characters_to_remove)] #applying that list to
    #clean_words to take out the characters we want to remove, characters_to_remove can be further added as well
    
    characters_to_remove2 = [word for word in clean_words if any(letter in sentences for letter in '\\')] 
    #taking out other slashes
    clean_words = [word for word in clean_words if word not in set(characters_to_remove2)] #again applying list
    print(clean_words) #print to see the final output of your new clean_words list from the original search
    
```
The results look something like this

```python
['we’re told that there is a crisis of masculinity.', 'from #metoo to #gillette - something isn’t working.', 'garrett j wh']
["@username @username a country run by women who don't approve of masculinity.", '#gillette #procterandgamble']
['rt @username: last month, #gillette released a controversial ad campaign calling on men to "be better" for the men 
of tomorrow.','but what']
```
*Not quite the sentiment of Gillette's statuses*, but we aren't there yet. I have ran the code for gillette (no hashtag) and #gilette with one l, since it appears have to been mispelled.

### Important Note

If you notice under `characters_to_remove` the `rt`s and the `@`s were not removed in the result. That is because the list can't identify different variations of rt or the handles or hashtags in which those characters are used. This is where tokenizing them into `word_tokenize` and separating them will be best in eliminating them. You can also use Excel to find and replace with space. 

### That ends Pt 1. LMK what you think (TBA Pt2 Sentiment Analysis, Pt3 Modelling)


