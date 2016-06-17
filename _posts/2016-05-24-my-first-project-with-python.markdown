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

{% for js in page.customjs %}
<script type='text/javascript' src='https://public.tableau.com/javascripts/api/viz_v1.js'></script><div class='tableauPlaceholder' style='width: 654px; height: 742px;'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Fi&#47;FirstPublicWorkbook&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz' width='654' height='742' style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='site_root' value='' /><param name='name' value='FirstPublicWorkbook&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Fi&#47;FirstPublicWorkbook&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='showTabs' value='y' /></object></div>
{% endfor %}
