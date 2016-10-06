---
layout: post
title:  "Pronto Cycle Share Exploratory Data Analysis Part 1"
date:   2016-10-05 00:34:00 -0700
categories: blog update
tags: [R]
---

Draft

https://www.prontocycleshare.com/datachallenge

{% highlight python %}
require(dplyr) #to group
require(ggplot2) #to graph
require(beanplot) #for beanplots
#data comes from https://www.prontocycleshare.com/datachallenge
setwd("/Users/Magic/Documents/R/Bike Share Seattle/open_data_year_one")

#load all trip data
tripdata = read.table("2015_trip_data.csv", header=T, quote="\"",sep=",")

#convert tripdata$stoptime to ymd date
tripdata$date = as.Date(tripdata$stoptime, format="%m/%d/%Y")

#create aggregated table
daily_trips = tripdata %>%
  group_by(date) %>%
  summarize(total_trips = n()
            ,mean_duration = mean(tripduration)
            ,max_duration = max(tripduration)
            ,min_duration = min(tripduration))

member_trips = tripdata %>%
  filter(usertype=="Annual Member") %>%
  group_by(date) %>%
  summarize(member_trips = n()
            ,member_mean_dur = mean(tripduration)
            ,member_max_dur = max(tripduration)
            ,member_min_dur = min(tripduration))

casual_trips = tripdata %>%
  filter(usertype=="Short-Term Pass Holder") %>%
  group_by(date) %>%
  summarize(casual_trips = n()
            ,casual_mean_dur = mean(tripduration)
            ,casual_max_dur = max(tripduration)
            ,casual_min_dur = min(tripduration))

daily_trips = left_join(daily_trips,member_trips, by = c("date" = "date"))
daily_trips = left_join(daily_trips,casual_trips, by = c("date" = "date"))
rm(casual_trips,member_trips)

#adding weekday num and workday y/n
daily_trips$weekday = as.POSIXlt(daily_trips$date)$wday 
daily_trips$workday = factor(daily_trips$weekday)
levels(daily_trips$workday) <- list("yes" = c("1","2","3","4","5"),"no" = c("0","6"))

#create month field
daily_trips$month = as.POSIXlt(daily_trips$date)$mon+1
daily_trips$month = ordered(daily_trips$month, 1:12) 
levels(daily_trips$month) = c(month.abb)

#create a new daily frequency table with weather data
require(sqldf)
weatherdata = read.table("2015_weather_data.csv", header=T, quote="\"",sep=",")
weatherdata$Date = as.Date(weatherdata$Date, format="%m/%d/%Y")
daily_trips = left_join(daily_trips,weatherdata, by = c("date" = "Date"))

#create season in new frequency table
require(zoo)
daily_trips$season = factor(format(as.yearqtr(as.yearmon(daily_trips$date
                                                         , "%m/%d/%Y") + 1/12), "%q"), levels = 1:4, 
                            labels = c("Winter", "Spring", "Summer", "Fall"))





#graph
total_use_plot = ggplot(daily_trips, aes(date,total_trips))+
  geom_line(col="blue", lwd=1, alpha = .3)+
  labs(x="Date",y="Number of Rides")+
  ggtitle("Daily Usage of Pronto Cycle Share")+
#  geom_point(col="gray50") +
  geom_smooth(method="loess") 
#view
total_use_plot


#double density plot - workday
ddensity_workday = ggplot(daily_trips, aes(total_trips, fill=workday, color=workday)) +
  geom_density(alpha=0.4) +
  theme_minimal() +
  xlab("Daily Bike Use Count") +
  ylab("Density") +
  scale_fill_discrete(name="Work Day?") +
  scale_color_discrete(name="Work Day?") +
  theme(axis.ticks.y = element_blank(), axis.text.y = element_blank(),
        legend.position="top")
ddensity_workday




#Boxplot
boxplot_daily = ggplot(daily_trips, aes(month, total_trips, fill=workday)) +
  xlab("Month") +
  ylab("Daily Cycle Use Count") +
 geom_boxplot() +
  theme_minimal() +
  scale_fill_discrete(name="Work Day?") +
  scale_color_discrete(name="Work Day?") +
  theme(legend.position="bottom")
boxplot_daily

#beanplot
#beanplot_daily = #creating a beanplot variable doesnt work?
beanplot(n ~ month
         , data = daily_trips, side="second"
         , overallline="median", what=c(1,1,1,0)
         , col=c("gray70", "transparent", "transparent", "blue")
         , ylab = "Month",
         xlab = "Daily Cycle Use Count", horizontal=TRUE) + 
  title("2015 Pronto Cycle Share Beanplot") #title throws error?
#beanplot_daily #no reason to call variable yet



#seasonal density chart
seasonal_density = ggplot(daily_trips, aes(total_trips, fill=season)) +
  geom_histogram(aes(y = ..density..), alpha=0.2, color="gray50") +
  geom_density(alpha=0.5, size=0.5) +
  facet_wrap(~season) +
  theme_light() +
  xlab("Daily Cycle Share Use Count") +
  ylab("") +
  theme(legend.position="none") +
  theme(axis.ticks.y = element_blank(), axis.text.y = element_blank(),
        axis.title = element_text(size=9, face=2, color="gray30"),
        axis.title.x = element_text(vjust=-0.5))
seasonal_density


require(zoo)
tripdata$season = factor(format(as.yearqtr(as.yearmon(tripdata$date
                                                      , "%m/%d/%Y") + 1/12), "%q"), levels = 1:4, 
                         labels = c("Winter", "Spring", "Summer", "Fall"))

#plot some non-temporal data
temp_use_scatter =ggplot(daily, aes(x=Mean_Temperature_F, y=n)) +
  xlab("Daily Mean Normalized Air Temperature") +
  ylab("Number of Total Bike Uses") +
  geom_point(col="gray50") +
  geom_smooth(method="loess") +
  theme_bw()

require(GGally)
ggpairs(data=daily_trips
        , columns=c(2,7,5) #count,workday,mean temp, season
        , upper = list(continuous = "density")
        , mapping=aes(color=season, alpha=0.3))

#add duration stats
	    
{% endhighlight %}
