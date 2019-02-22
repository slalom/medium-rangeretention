# Calculating and rendering range retention in Pyhton


2/12/2019

Raphael Vannson


## Abstract
This post explains what range retention is, provides a strategy to calculate it and a link to a Python / Jupyter notebook providing a complete hands on example.



## What is `Range Retention`?
### Simple question... simple answer?

Retention is an observational metric used by organizations to evaluate how much of their members come back after a  first contact. For example: brick and mortar shop owners want to know if customers come back after their first purchase, web platforms want to know if users visit come back after they have created an account, etc...

So what's the big deal I hear you ask? Sounds like retention can easily be defined as a single number given by this formula:

$$
\begin{equation}
R = \frac{Number\ of\ unique\ users\ who\ interacted\ more\ than\ once \ with\ the\ organization}{Number\ of\ unique\ users\ who\ interacted\ with\ the\ organization}
\end{equation}
$$


### Not so fast...
The problem with this definition of retention is that it is an average over all users since the organization first day of business. It does not say much about the organization's current ability to get its members to come back nor does it measure the (potential) effect of actions taken by the organization to improve retention. 


### `Range Retention` to the rescue!

One (popular) way to address this is to get a tad more refined by calculating multiple (relevant) retention rates - `Range Retention` does just that. It provides a granular view into a long period of interest by breaking it down into small windows. This makes it possible to track how users of each window behaved throughout the rest of the long period of interest. Sounds cryptic? Nah... Here is a simple example: Say our long period of interest is 2018 and that we break it down by month, then our range retention is going to be a bunch of retention rates focusing on a specific user cohort at a specific month: 

 * Users who made first contact in January: what proportion came back in February, March, ..., December?
 * Users who made first contact in February: what proportion came back in March, April, ..., December?
 * ...
 * Users who made first contact in November: what proportion came back in December?


So, `Range Retention` consists in:

 * Defining a period of interest (ex: the last 12 months) 
 * Breaking down the period of interest into small chunck (ex: month, week)
 * Identifying the cohort of users or each chunk (all the users who made first contact during the chunk)
 * For each cohort calculate the percentage of users in the cohort who came back in the subsequent chunks
 
 
`Range Retention` will present itself as a triangular table with one row per user cohort and one column per chunk.



## How to calculate `Range Retention`?

In this section we provide a data manipulation approach to calculate `Range Retention`.

### The basics
Ensure your data is loaded in a dataframe of your favorite flavor and that the column types are what they need to be. In addition make sure there is one column capturing the "chunk ID" (ex: month or week number). 



### Define the cohorts

You will want to create a dataframe wich maps


