---
layout: post
title:  "NPR Coverage of Presidential Candidates"
date:   2016-05-24 00:34:00 -0700
categories: blog update
---

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
