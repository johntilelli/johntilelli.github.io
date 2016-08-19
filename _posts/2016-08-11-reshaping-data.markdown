---
layout: post
title:  "Using R to Reshape Wide Data into Tall Data"
date:   2016-08-11 00:34:00 -0700
categories: blog update
---

This short post shows the process of reshaping data from wide to tall, and the merits of tall data in Tableau. The topic of this post was inspired by a new series called [Makeover Monday](http://www.vizwiz.com/p/makeover-monday-challenges.html). If you use Tableau you have to give it a try - it's a super fun way to improve your visualization skills - especially for a novice like myself. It gives you the opportunity once a week to really think creatively on every step of the visualiztion process.

<br>

One thing I noticed week to week on Makeover Monday is that the dataset provided was often wide instead of long. For example, take a look at the dataset from Week 32:  [All-Time Summer Olympic Medal Standings](http://www.nbcolympics.com/medals). Each line of data represents the number of medals each country earned each year they participated. There are 8 columns in the dataset. In my mind, the first 4 columns are attributes (or dimensions) of the last 4 columns, which I consider metrics (or measures). 

<br>

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
<p> Using the following R code I reshaped my data: </p>

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
<p>
Here is the resulting "tall" dataset. Two columns were added, variable and amount, that replace the last four columns in the original dataset. The variable column is now used as an attribute to describe the amount gold, silver, or bronze medals awareded for each country for each year they participated. 
</p>
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

<br>

Here is my visualization below using the "tall" dataset. It shows 3 visualizations on the same worksheet, two barcharts and a stacked % of total bar chart, with a complex sort. It wasn't my best work, especially in terms of design, but it shows how much easier tall data is to work with in Tableau. It was incredibly easy to create the table calc - I simply added the "amount" pill to the column pane and created a quick table calculation, percent of total, then added "variable" to the color on the marks pane. I was unable create this same worksheet using the original data. If anyone knows of a way to replicate this in Tableau with the original data please reply/send me a DM on [twitter](https://twitter.com/JohnTilelli). 


<br>

![Picture of Week 32 Makeover Monday](https://johntilelli.github.io/mmwk32.png)

<br>

<div id="disqus_thread"></div>
<script>
    
    var disqus_config = function () {
        this.page.url = johntilelli.com/blog/update/2016/08/11/reshaping-data.html;
        this.page.identifier = '2016-08-11-reshaping-data'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        
        s.src = '//www-johntilelli-com.disqus.com/embed.js';
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
