- title : Spice up your website with machine learning!
- description : Spice up your website with machine learning.
- author : Evelina Gabasova
- theme : white
- transition : none

****************************************************************************************************

<img src="images/chilli_pepper.png" style="width:200px" />

# Spice up your website

#  with Machine Learning!

<br/>

Evelina Gabasova 

[@evelgab](http://www.twitter.com/evelgab)  

****************************************

# F# Snippets

<img src="images/fssnip1.png" style="width:1000px" />

' F# snippets are like Gist on steroids
' Started 5 years ago by Tomas Petricek

****************************************

# F# Snippets

[fssnip.net](http://fssnip.net/1k)


' Demo of F# snippets website
' It all works nice until you start searching for something

****************************************

- data-background : images/fssnip-tags.png

' You can search by tag

****************************************

# Searching through F# snippets

over 1600 snippets

over 1100 different tags

****************************************

# Searching through F# snippets

![](images/google_fssnip.png)

****************************************

- data-background : images/fssnip-tags.png


****************************************

# Why we need a custom system

<br/>

	let rec amazingFunction work resultOk retries = async {
	  let! res = work
	  if (resultOk res) || (retries = 0) then return res
	  else return! amazingFunction work resultOk (retries - 1) }


' F# is a statically typed language!
' I'm not interested in how is this function actually called,
' but I'm interested in that it takes asyncronous workflow
' and returns asyncronous workflow.
' This type of information is not available as text and google 
' doesn't index it for search.
' Great opportunity to create a custom machine learning system!!!

****************************************

![](images/hoogle.png)


****************************************

## Great opportunity to create a custom machine learning system!

' We're programmers, we like to automate - alternative would be do 
' go through the tags manually and curate them!
' what can possibly go wrong?
' There's this company that also does a lot of machine learning...

****************************************

![](images/russia_mordor.png)

****************************************

<table><tr><td class="noborder">

![](images/lavrov.jpg)

</td>
<td class = "noborder fragment">

<img src="images/sad_little_horse.jpg" style="width:900px" />

</td>
</tr></table>


' Sergey Lavrov (Russian foreign minister) = sad little horse

****************************************

<img src="images/nn.png" style="width:400px" />

Nguyen A et al.: Deep Neural Networks are Easily Fooled:
High Confidence Predictions for Unrecognizable Images. 2015.

****************************************

# Using machine learning in production

<br />

- dependence on training data

- unpredictable black-box 


' incoming data is not controlled, users do many different things
' Black boxes can be unpredictable

****************************************

# Finding related snippets

<br/>

If you liked this _F# code_, you'll also like ...

****************************************

# Simple information retrieval

*common terms*

' Looking at common terms is not enough
' Two snippets may have a large overlap if they use common words

****************************************

# Bag of words

<br />

- ignore order of words

- separate *text* and *code*

' This allows us to look at code and text differently
' Text - free structured
' Code - fixed structure with keywords

****************************************

# Term frequency

<br />

<table><tr><td class="noborder">

## Snippet 1

| Term  | Frequency |
|-------|-----------|
| async | 3         |
| x     | 15        |
| The   | 2         |
| code  | 1         |
| ...   |           |

</td><td class="noborder">

## Snippet 2

| Term  | Frequency |
|-------|-----------|
| async | 0         |
| x     | 15        |
| The   | 2         |
| code  | 1         |
| ...   |           |

</td>
</tr>
</table>
' Why comparing snippets based on their term frequency only is not sufficient
' async is informative, x is not

****************************************

# Inverse document frequency
## Relative importance of terms

<br />

$$$ 
idf(\text{term}) = \log \frac{\text{number of snippets}}{\text{number of snippets with term}} 

' Weight - how informative a term is
' Look at what features are the least informative

****************************************

# Vector representation: TF-IDF
## Term frequency - inverse document frequency

<br />

$$$
tfidf(\text{term}, \text{snippet}) = tf(\text{term}, \text{snippet}) \times idf(\text{term})

' This makes it comparable between different documents
' By computing this for every snippet, we get vector representation

****************************************

# Vector representation of snippets

![](images/angle.png)

' angle between snippet vectors shows how related they are

****************************************

# Demo

' Show one snippet - vector representation & result
' And show the resulting web page presentation

****************************************************************************************************

# Suggesting tags

<br />

<img src="images/tags_box.png" style="width:800px" />

****************************************************************************************************

# Suggesting tags

<br />

<img src="images/clippy.png" style="width:400px" />

' One of the most popular tags is F#

****************************************

# Making sense of user-generated tags

async, #async, async mailprocessor, async paraller, Async sequences, asyncseq, asynchronous, Asynchronous Processing, Asynchronous Programming, asynchronous sequence, asynchronous workflows 

' Capitalization, plural vs singular, typos, different transcriptions
' singular
' but then Window Form and other things like this


****************************************
# Edit distance

*regex* vs. *regexp*

<div class="fragment">

*sports* vs. *ports* <br/>
*pi* vs. *API*

</div>

' insertion, deletion

****************************************
# Machine learning
## From snippets to tags


' Mapping 
' For each snippet, I have a set of tags
' I can use the data to train a predictor

****************************************
# Naive Bayes

![](images/bayes.gif)

## Why do you call me naive?

****************************************
# Why naive?

<br/>

*string* and *parser*

*async* and *MailboxProcessor*

<br/>

*sequence* and *exception*

' naive means we're making our lives easier because we can
' ignore how individual terms depend on each other

****************************************
# Building a predictor

![](images/async_tag.png)

' We can actually estimate this from data

****************************************
# Building a predictor

![](images/async_tag2.png)

' Maybe this should have the "async" tag!

****************************************
# Tag probabilities
## Bayes theorem

<br />

$$$
p(\text{tag} \mid \text{snippet}) \propto p(\text{tag}) \prod_{\text{term}} p(\text{term} \mid \text{tag})

****************************************
# 1. Prior probabilities

<br />

$$$
p(\text{tag}) \approx \frac{\text{Number of snippets with the tag}}{\text{Number of snippets}}


' p(async) = 5%

****************************************

# 2. Tag likelihood

<br />

How frequent is the *term* among snippets that have the *tag* ?

<br />

$$$
p(\text{term} \mid \text{tag}) = \frac{\text{Number of snippets with the term and tag}}{\text{Number of snippets with the tag}}

****************************************

# Naive Bayes prediction

<br />

$$$
p(\text{tag} \mid \text{snippet}) \propto p(\text{tag}) \prod_{\text{term}} p(\text{term} \mid \text{tag})

<br />

$$$
\frac{p(\text{tag} \mid \text{snippet})}{p(\neg\text{tag} \mid \text{snippet})} > 1 ?

' Working in logarithm space
' The whole prediction reduces to summations

****************************************

# The theory is always nicer 

What if there is no snippet tagged *async* that contains *List*?

' Introduce numerical fixes that mean that it is a bit messiers

****************************************

# Demo


****************************************

# Summary

<br />

* Domain representation

* What are important features

* Machine learning is fun!

<br /> <br />

## Contribute to F# snippets website 
[github.com/tpetricek/FsSnip.Website](https://github.com/tpetricek/FsSnip.Website)

' Tf-idf as a reasonable representation 
' Choosing which features are important


****************************************************************************************************

# Learning more 

 - F# snippets [fssnip.net](http://fssnip.net)

 - The F# Foundation [www.fsharp.org](http://www.fsharp.org)
 - FsLab Package [www.fslab.org](http://www.fslab.org)

 - Introduction to information retrieval [informationretrieval.org](http://informationretrieval.org/) 
 
****************************************

# Thank you!

<table>
<tr>
  <td class="noborder"><img src="images/twitter.png" style="width:70px" /></td>
  <td style="vertical-align:middle" class="noborder"> [@evelgab](https://twitter.com/evelgab)</td>
</tr>
<tr>
  <td class="noborder"><img src="images/github.png" style="width:70px" /></td>
  <td class="noborder" style="vertical-align:middle" > [github.com/evelinag](https://github.com/evelinag)</td>
</tr>
<tr>
  <td class="noborder"></td>
  <td class="noborder" style="vertical-align:middle" > [evelinag.com](http://evelinag.com)</td>
</tr>
</table>