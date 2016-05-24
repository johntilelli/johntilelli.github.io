---
layout: post
title:  "My first project with python"
date:   2016-05-24 00:34:00 -0700
categories: blog update
---
For my first post, I will be walking through the beginnings of a simple data analysis project. Path of the project:
1. Use Python to call the NPR API and return a JSON file.
2. Explore the API, find a good data set, and parse the data in a readable format.
3. Convert the JSON file into a clean CSV file
4. Use python to generate a simple graph.
5. Make the graph interactive on this website. 

I'll walk through my experience with part 1 of my project. Here is the code I used to pull a JSON file from the NPR website.
{% highlight python %}
from urllib2 import urlopen
from json import load, dumps

key = "demo"

url = "http://api.npr.org/query?id=3&fields=title,teaser,storyDate,byline,text&date=2016-05-16&dateType=story&sort=dateAsc&output=JSON&apiKey=" + key

response = urlopen(url)
j = load(response)

with open('output3.json', 'w') as f:
    f.write(dumps(j, indent=4))
f.close()

{% endhighlight %}
