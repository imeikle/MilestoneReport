---
title       : Milestone Report
subtitle    : 
author      : Ian Meikle
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

---

## Text Attributes

The text file attributes are easily derived using Unix commands:

```r
system2(command='wc', args=c('-wl', '../final/en_US/*'), stdout=TRUE)[-4]
```

```
## [1] "  899288 37334690 ../final/en_US/en_US.blogs.txt"  
## [2] " 1010242 34372720 ../final/en_US/en_US.news.txt"   
## [3] " 2360148 30374206 ../final/en_US/en_US.twitter.txt"
```
Here, the first column lists the number of lines, the second the number of words, 
the third is the filepath.

--- .class #id 
## Word Frequency
Taking a representative sample of the blog dataset, removing punctuation and tokenising 
allows a word frequency table to be created.

```r
head(blog_uni_dt)
```

```
##    w_0 count
## 1: the 36829
## 2: and 21959
## 3:  to 21625
## 4:   a 17723
## 5:  of 17397
## 6:   i 15639
```
By inspection it is easy to see that only approximately 7000 words are needed to produce 
ninety percent of the text. This will be used to optimise the words offered as options in the 
final app.

---
## Word Length Plot
This histogram shows the word length distribution for those most common words in the blogs dataset.

![plot of chunk wordLength](assets/fig/wordLength-1.png)

---
## Prediction Algorithm
Text prediction based on single word frequency would be very simplistic. The next stage is 
to calculate the frequency of two- and three-word combinations, know as bigrams and trigrams 
respectively. The relative frequencies of these three *n-grams* 
will form the basis of a **backoff** model with smoothing , allowing the prediction of the next word in a
sequence based on how prevalent the last two words have been. Smoothing in this context refers to a 
redistribution of probability, which does not restrict predictions to solely those phrases encountered i
the training set.

For reasons of optimisation, only those n-grams formed of the most common words will be included in 
the text prediction app.

---
## Shiny App

The app will be provided on the Shiny platform. It will consist of a text box in which the user may type
a phrase. On submission, the next word will be suggested from up to five options in a drop down list, 
sorted in descending order of likelihood.

Performance constraints allowing, the user will be able to choose which type of text they are 
composing, chosen from blog, tweet, news item. The text prediction algorithm will then use the
appropriate statistical model of n-gram frequency.

