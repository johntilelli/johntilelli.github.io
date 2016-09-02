---
layout: post
title:  "First visualization using R"
date:   2018-08-24 00:34:00 -0700
categories: blog update
tags: [R, Data Visualization]
---

<b> Intro: </b>
<p> This post is the first of a series in which I’ll be applying some concepts from a new book called Business Intelligence with R by Rmadillo(link) to a data set called Pronto(link). I consider the book to a more of toolset from a very intelligent, experienced data scientist/statistician. I highly recommend purchasing the book. It is great for your average analyst that wants to learn more about R and how it can be used in every step of the BI reporting process, from importing the data, to cleaning it, to exploring it, and the different ways you’ll end up using it. </p>

<b> The visualization: </b>
<p> For this very simple first visualization, I’ll be using some concepts from the cleaning and prepping section, as well as the trends and time section. I’m hoping that the graph is fairly intuitive. </p>

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
  #aggregate data daily to make visualizing easier
    #count of trips
    #average distance
    #sum of distance traveled
    #number of bikes used
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
<p> The data comes from a data challenge from a Bike Share Service called Pronto. They winners of last year’s contest are amazing. It will be a long time before I reach their level of expertise, but you have to start somewhere right? </p>

<b> Conclusion: </b>
<p>I think the graph, as simple as it is, does a good job at explaining what it set out to do – display volume of the bike share program in Seattle over time. </p>



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

