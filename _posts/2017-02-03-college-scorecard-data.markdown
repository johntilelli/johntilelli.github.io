---
layout: post
title:  "College Scorecard Data"
date:   2017-02-03 00:34:00 -0700
categories: blog update
tags: [R, Data Visualization]
---

https://collegescorecard.ed.gov/data/

1.	Project Definition 

Dataset includes school and student demographics, degrees awarded by subject area as well as financial information including average degree cost by income bracket, student tuition and scholarships, repayment rates, and post-college average earnings.


2.	Data Collection

Start by placing all data in a SQLite db to easily query.

{% highlight python %}

#set working directory
getwd()
setwd("/Users/Magic/Documents/Introduction to Data Science/Project")
list.files()

library(RSQLite)
#create database variable
dbnm = "/Users/Magic/Documents/Introduction to Data Science/Project/db.sqlite"
db <- dbConnect(SQLite(), dbname=dbnm)

#load data
wp <- read.csv("/Users/Magic/Documents/Introduction to Data Science/Project/CollegeScorecard_Raw_Data/MERGED2014_15_PP.csv", header = TRUE)

# Write a data frame to SQLite
dbWriteTable(db, "MAIN", wp)
dbListTables(db)

#create table from sqllite
tbl <- dbReadTable(db, "MAIN")

#sql function
runsql <- function(sql, dbname=dbnm){
  require(RSQLite)
  driver <- dbDriver("SQLite")
  connect <- dbConnect(driver, dbname=dbname);
  closeup <- function(){
    sqliteCloseConnection(connect)
    sqliteCloseDriver(driver)
  }
  dd <- tryCatch(dbGetQuery(connect, sql), finally=closeup)
  return(dd)
}

#first sql query
sql <- 'SELECT * FROM MAIN LIMIT 10;'
res <- runsql(sql)
sqltbl = head(res,10)


#cleanup as needed
#runsql("DROP TABLE MAIN")
#dbListTables(db)

#test
runsql("SELECT COUNT(*) FROM MAIN")

dbDisconnect(db)

{% endhighlight %}


3.	Data Exploration

4.	Data Modeling
5.	Evaluation Model
6.	Visualize
