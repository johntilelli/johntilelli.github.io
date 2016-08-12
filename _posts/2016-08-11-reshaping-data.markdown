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
