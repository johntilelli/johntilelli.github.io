---
layout: post
title:  "Using R to Reshape Wide Data into Tall Data"
date:   2016-08-11 00:34:00 -0700
categories: blog update
---

Draft of post

     require(reshape2)
     require(readxl)
     require(dplyr)
{% highlight r %}
<p>
#create dataframe of olympics data
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
</p>
{% endhighlight %}
