---
layout: post
title:  "Python Project, part 1"
date:   2016-05-24 00:34:00 -0700
categories: blog update
---

Path of the project:
1. Call the NPR API and create a JSON file.
2. Explore the API, find a good data set, and parse the data in a readable format.
3. Convert the JSON file into a clean CSV file.
4. Get around the limits of the NPR API (namely, maximum of 20 posts returned) by iterating through date in a for loop.
5. Make a simple interactive graph using Bokeh library and import to blog. 

Part1: Call the NPR API and create a JSON file.
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

{% include tableau1.html %}
