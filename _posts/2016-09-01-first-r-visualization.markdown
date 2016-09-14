---
layout: post
title:  "First Visualization Using R: Daily Volume of Pronto Cycle Share in 2015"
date:   2016-09-01 00:34:00 -0700
categories: blog update
tags: [R, Data Visualization]
---

<b> Intro: </b>

This is the first post in a series in which I’ll be applying concepts from <i>Business Intelligence with R</i> by [Rmadillo](https://twitter.com/healthstatsdude) to a data set from the Pronto Cycle Share program in Seattle. I highly recommend the book for the average analyst who wants to learn more about R and how it can be used throughout the BI reporting process— from importing and cleaning to exploring and actually analyzing your data.

<b> The visualization: </b>

For my first visualization, I used concepts from the book’s cleaning and prepping section and the trends and time section to create a simple graph. My goal was for it to be fairly intuitive. 

![Picture of Week 32 Makeover Monday](http://johntilelli.com/first_graph_in_R.png)

<b> The code: </b>

{% highlight python %}

require(lubridate) #do we need this for as.Date?
require(dplyr) #to group our days for the frequency table
require(ggplot2) #to graph
setwd("/Users/Magic/Documents/Projects/Bike Share Seattle/open_data_year_one")
list.files()
#load all trip data
tripdata = read.table("2015_trip_data.csv", header=T, quote="\"",sep=",")
#https://www.prontocycleshare.com/datachallenge
summary(tripdata)
#convert tripdata$stoptime to ymd date
tripdata$date = as.Date(tripdata$stoptime, format="%m/%d/%Y")
#create table with frequency by day
daily_trips = count(tripdata, date)
#group by day
daily_trips_group = group_by(daily_trips, date)
#plot
total_use_plot = ggplot(daily_trips_group, aes(date,n))+
  geom_line(col="blue", lwd=1, alpha = .3)+
  labs(x="Date",y="Number of Rides")+
  ggtitle("Daily Usage of Pronto Bike Share")
#view
total_use_plot

{% endhighlight %}

<b> The data: </b>
The data comes from the [Pronto Data Challenge](https://www.prontocycleshare.com/datachallenge). The winners’ entries are amazing and definitely worth checking out.

<b> Conclusion: </b>

As simple as the graph is, it does a good job achieving what I set out to do: display the volume of the bike sharing program over time. There are clearly a lot of improvements I could make to my analysis of the bike sharing program, such as the correlation with weather patterns and seasons, breakdown of volume, or average time spent riding by day of week. But hey, you have to start somewhere, right?



<br>

<div id="disqus_thread"></div>
<script>
    
    var disqus_config = function () {
        this.page.url = 'http://johntilelli.com/blog/update/2018/08/24/first-r-visualization.html';
        this.page.identifier = '2016-09-01-first-r-visualization'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        
        s.src = '//www-johntilelli-com.disqus.com/embed.js';
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>

