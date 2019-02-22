# Calculating and rendering range retention in Pyhton


2/12/2019

Raphael Vannson


## Abstract
This post explains what range retention is, provides a strategy to calculate it and a link to a Python / Jupyter notebook providing a complete hands on example.



## What is `Range Retention`?
### Simple question... simple answer?

Retention is an observational metric used by organizations to evaluate how much of their members come back after a  first contact. For example: brick and mortar shop owners want to know if customers come back after their first purchase, web platforms want to know if users visit come back after they have created an account, etc...

So what's the big deal I hear you ask? Sounds like retention can easily be defined as a single number given by this formula:

![](img/formula.png)


### Not so fast...
The problem with this definition of retention is that it is an average over all users since the organization first day of business. It does not say much about the organization's current ability to get its members to come back nor does it measure the (potential) effect of actions taken by the organization to improve retention. 


### `Range Retention` to the rescue!

One (popular) way to address this is to get a tad more refined by calculating multiple (relevant) retention rates - `Range Retention` does just that. It provides a granular view into a long period of interest by breaking it down into small windows. This makes it possible to track how users of each window behaved throughout the rest of the long period of interest. Sounds cryptic? Nah... Here is a simple example: Say our long period of interest is 2018 and that we break it down by month, then our range retention is going to be a bunch of retention rates focusing on a specific user cohort at a specific month: 

 * Users who made first contact in January: what proportion came back in February, March, ..., December?
 * Users who made first contact in February: what proportion came back in March, April, ..., December?
 * ...
 * Users who made first contact in November: what proportion came back in December?

Our formula is still valid it just no longer applies to all users and all time, instead it is specific to users who made first contact during a select chunck and came back during a later one.

So, `Range Retention` consists in:

 * Defining a period of interest (ex: the last 12 months) 
 * Breaking down the period of interest into small chunck (ex: month, week)
 * Identifying the cohort of users or each chunk (all the users who made first contact during the chunk)
 * For each cohort calculate the percentage of users in the cohort who came back in the subsequent chunks
 
 
`Range Retention` will present itself as a triangular table with one row per user cohort and one column per chunk.



## How to calculate `Range Retention`?

In this section we provide an approach to calculate `Range Retention`.

This [Jupyter notebook](https://github.com/slalom/medium-rangeretention/blob/master/notebook/range-retention.ipynb) provides all the code. Feel free to clone this [Github repo](https://github.com/slalom/medium-rangeretention.git) to get all the artifacts of this post:

```bash
$ git clone https://github.com/slalom/medium-rangeretention.git
```


### Raw data

Let's start with a table containing orders data for year 2004 filtered to retain only customers who made first contact in 2004. The table could look like this:

![](img/rawdata.png)


### The basics
Ensure your data is loaded in a dataframe of your favorite flavor and that the column types are what they need to be. In addition make sure there is one column capturing the "chunk ID" (ex: month or week number). 



### Define the cohorts
Let's pick the months as our "chunks" so the `cohort ID` are going to be the month when the user first joined the as a customer: `joined_month`. You will want to create a dataframe mapping the `customer_id` with the cohort. The head of this dataframe would look like this:

![](img/cohorts_df.png)


### Get the cohort sizes

Next, you want to create another dataframe which maps each `joined_month` with the number of unique `customer_id`.

![](img/cohort_sizes_df.png)


### Update the raw data with `cohort ID` and `activity index`

The next step is to go back to the raw orders data and to add a couple of useful columns to it. For each order row, we will want to add `joined_month` (= `cohort ID`) based on the customer ID and an `activity_index` which is based on order date and the join date. The `activity_index` captures how many chunks after the join date the order was placed at, ex: for a `join_month` of Janurary (`1`), then an order placed in March will have an `activity_index` of `2` (*activity is 2 months after first contact*).

Selecting only the columns of interest, we now have one row per order identifying which customer placed the order, which cohort (`join_month`) and how many months after joining the order was placed (`activity_index`):

![](img/df.png)


### Get the number of returning customers 

Count the number of returning customers per cohort, per month:

![](img/activity_size_df.png)


### Calculate the `Range Retention` table

Join the counts, make a division...
![](img/retention_df.png)


...pivot and you're done!

![](img/retention_tbl.png)

