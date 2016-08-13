---
layout: post
title:  "Using R to Reshape Wide Data into Tall Data"
date:   2016-08-11 00:34:00 -0700
categories: blog update
---

Draft of post

| Edition | Rank | Country       | Country Group | Gold | Silver | Bronze | Total |
|---------|------|---------------|---------------|------|--------|--------|-------|
| 1896    | 1    | United States | United States | 11   | 7      | 2      | 20    |
| 1896    | 2    | Greece        | Greece        | 10   | 17     | 19     | 46    |
| 1896    | 3    | Germany       | Germany       | 6    | 5      | 2      | 13    |
| 1896    | 4    | France        | France        | 5    | 4      | 2      | 11    |
| 1896    | 5    | Great Britain | Great Britain | 2    | 3      | 2      | 7     |
| 1896    | 6    | Hungary       | Hungary       | 2    | 1      | 3      | 6     |
| 1896    | 7    | Austria       | Austria       | 2    | 1      | 2      | 5     |
| 1896    | 8    | Australia     | Australia     | 2    |        |        | 2     |
| 1896    | 9    | Denmark       | Denmark       | 1    | 2      | 3      | 6     |
| 1896    | 10   | Switzerland   | Switzerland   | 1    | 2      |        | 3     |
| 1896    | 11   | Mixed team    | Mixed team    | 1    | 1      | 1      | 3     |

<br>

{% highlight r %}

require(reshape2)
require(readxl)
require(dplyr)
#create dataframe
olympics = readxl::read_excel("Olympic Medal Table.xlsx",sheet = 1)

#remove total column
olympics_temp = subset(olympics
     , select = c("Edition","Rank","Country","Country Group","Gold","Silver","Bronze"))

#melt data
olympics_melted = reshape2::melt(olympics_temp, id.vars=c("Edition","Rank","Country","Country Group")
     ,measure.vars=c("Gold","Silver","Bronze"),
     variable_name ="Medal", value.name="Amount")

#convert to data frame, issue in dplyr requires this
write.table(olympics_melted, "Olympics Medal Table - Melted.csv", sep=",", row.names=FALSE)

{% endhighlight %}

<br>

| Edition | Rank | Country       | Country Group | variable | Amount |
|---------|------|---------------|---------------|----------|--------|
| 1896    | 1    | United States | United States | Gold     | 11     |
| 1896    | 2    | Greece        | Greece        | Gold     | 10     |
| 1896    | 3    | Germany       | Germany       | Gold     | 6      |
| 1896    | 4    | France        | France        | Gold     | 5      |
| 1896    | 5    | Great Britain | Great Britain | Gold     | 2      |
| 1896    | 6    | Hungary       | Hungary       | Gold     | 2      |
| 1896    | 7    | Austria       | Austria       | Gold     | 2      |
| 1896    | 8    | Australia     | Australia     | Gold     | 2      |
| 1896    | 9    | Denmark       | Denmark       | Gold     | 1      |
| 1896    | 10   | Switzerland   | Switzerland   | Gold     | 1      |
| 1896    | 11   | Mixed team    | Mixed team    | Gold     | 1      |
| 1896    | 1    | United States | United States | Silver   | 7      |
| 1896    | 2    | Greece        | Greece        | Silver   | 17     |
| 1896    | 3    | Germany       | Germany       | Silver   | 5      |
| 1896    | 4    | France        | France        | Silver   | 4      |
| 1896    | 5    | Great Britain | Great Britain | Silver   | 3      |
| 1896    | 6    | Hungary       | Hungary       | Silver   | 1      |
| 1896    | 7    | Austria       | Austria       | Silver   | 1      |
| 1896    | 8    | Australia     | Australia     | Silver   | NA     |
| 1896    | 9    | Denmark       | Denmark       | Silver   | 2      |
| 1896    | 10   | Switzerland   | Switzerland   | Silver   | 2      |
| 1896    | 11   | Mixed team    | Mixed team    | Silver   | 1      |
| 1896    | 1    | United States | United States | Bronze   | 2      |
| 1896    | 2    | Greece        | Greece        | Bronze   | 19     |
| 1896    | 3    | Germany       | Germany       | Bronze   | 2      |
| 1896    | 4    | France        | France        | Bronze   | 2      |
| 1896    | 5    | Great Britain | Great Britain | Bronze   | 2      |
| 1896    | 6    | Hungary       | Hungary       | Bronze   | 3      |
| 1896    | 7    | Austria       | Austria       | Bronze   | 2      |
| 1896    | 8    | Australia     | Australia     | Bronze   | NA     |
| 1896    | 9    | Denmark       | Denmark       | Bronze   | 3      |
| 1896    | 10   | Switzerland   | Switzerland   | Bronze   | NA     |
| 1896    | 11   | Mixed team    | Mixed team    | Bronze   | 1      |
