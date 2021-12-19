Project 3: Web APIs & NLP

### Description

In week four of General Assembly's DSI course we learned about a few different classifiers. In week five we learned about webscraping, APIs, and Natural Language Processing (NLP). This project put those skills to the test.

For this project, the goal was two-fold:
1. Using [Pushshift's](https://github.com/pushshift/api) API, we collected submissions and comments from the two subreddits, 'dndnext' and 'dmacademy'
2. We then used NLP to train a classifier on which subreddit a given comment came from, a classic binary classification problem.


#### From Assignment: About the API

Pushshift's API is fairly straightforward. For example, if I want the posts from [`/r/boardgames`](https://www.reddit.com/r/boardgames), all I have to do is use the following url: https://api.pushshift.io/reddit/search/submission?subreddit=boardgames

To help you get started, we have a primer video on how to use the API: https://youtu.be/AcrjEWsMi_E

**NOTE:** Pushshift now limits you to 100 posts per request (no longer the 500 in the screencast).

---

### From Assignment: Requirements

- Gather and prepare your data using the `requests` library.
- **Create and compare two models**. One of these must be a Random Forest classifier, however the other can be a classifier of your choosing: logistic regression, KNN, SVM, etc.
- A Jupyter Notebook with your analysis for a peer audience of data scientists.
- An executive summary of your results.
- A short presentation outlining your process and findings for a semi-technical audience.

---

### Deliverables for Submission

- Four commented Jupyter Notebooks: pushshift_subs.ipynb, pushshift_comments.ipynb, cleaning_and_eda.ipynb, modeling.ipynb
- A readme/executive summary in markdown (this readme file)
- A .pdf of the slide presentation of the project

---

To be submitted by: **10:00 AM on Friday, October 23rd**

---

## Problem Statement
We have been contracted by Wizards of the Coast, the owners of Dungeons & Dragons, to analyze text to see if we can determine whether the poster is simply a D&D fan or a D&D Dungeon Master (DM), a driver for game sessions. Reviewing comments from the subreddits for the 5th addition of the game ‘dndnext’ and a general subreddit for DMs ‘dmacademy’, we will attempt to build a model to fit their needs. 

Our goal will be to find the model with the best accuracy metric for this binary classification problem. Additionally, we want to observe what words and n-grams are trending among both subreddits.

---

## Data Collection

Using the Pushshift API we first attempted to gather submissions from the two selected subreddits. Regrettably, each subreddit only yielded approximately 3000 text-only submissions, so we moved onto comments. We were easily able to scrape approximately 20,000 comments from each subreddit for further analysis

---

### Sentiment Analysis

We hypothesized that Sentiment Analysis would not be a good way to examine this data but made the attempt. As often D&D can involve violent or morally ambiguous actions done for good purposes, it would be difficult for a computer to analyze exactly what a player's intention and sentiment is solely from text. This data lends itself well to visual representation, however, so several plots were generated based on sentiment analysis. 

---

### Cleaning and EDA

From the data gathered there were no null values for 'author' or 'body'. To clean the data we did examine instances of duplicate posts. As those we explored seemed to be mainly substantive we only removed duplicates with a frequency of 12 or more instances. 

A majority of the cleaning done was specifically text cleaning the comments' 'body'. We build a function called 'mrclean' to apply to the dataframe. This took care of putting the body of text in lower case, contractions, special characters, and 1- and 2-letter words. Upon reflection this may have cut needed data, like references to 3e, 4e, 5e meaning third edition, fourth edition, fifith edition: the different versions of D&D. 

Thereafter we took a look at stop words with purpose of adding additional stop words. The vocabulary around D&D can be very specific so we added the most common 35 words appearing in both subreddits to our stop word list. 

---

## Preprocessing, Modeling, and Evaluation

blah blah blah

---

## Conclusion

Both CountVectorizer (cvec) and TF-IDFVectorizer (tfid) yielded incredibly close results: 

cvec:
- MultinomialNB()
    - Mean cross_val_score of 0.7359
    - Training score of 0.7680
    - Testing score of 0.7365

- LogisticRegression(max_iter = 10_000)
    - Mean cross_val_score of 0.7210
    - Training score of 0.8620
    - Testing score of 0.7235
    
tfid:
- MultinomialNB()
    - Mean cross_val_score of 0.7388
    - Training score of 0.7890
    - Testing score of 0.7243

- LogisticRegression(max_iter = 10_000)
    - Mean cross_val_score of 0.7403
    - Training score of 0.8158
    - Testing score of 0.7243
    
Were the company to run additional analysis in the future I would encourage them to use MultinomialNB for their model as it produces the least over-fit results. Should we re-collect our data to attempt this analysis again, we would want to look at a greater date range of posts. In this analysis we looked at comments going back to 10/22/21 and 10/19/21 for the 'dndnext' and 'dmacademy' subreddits, respectively. Had we had the computing power (and time) available to a company like Wizards of the Coast, the model would have been built on additional data, likely going back to the last module release (The Wild Beyond the Witchlight, September 21, 2021), or specific to whatever Wizards was researching, eg. looking at comments leading up to the last holiday season to see what's trending for that busy retail season. 