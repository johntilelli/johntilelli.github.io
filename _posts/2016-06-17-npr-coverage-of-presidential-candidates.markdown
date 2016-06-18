---
layout: post
title:  "NPR Coverage of Presidential Candidates"
date:   2016-05-24 00:34:00 -0700
categories: blog update
---

{% include tableau1.html %}

Here is the Python code used to call the NPR API, load the JSON output, and parse into a CSV file to load into Tableau.

{% highlight python %}

from urllib2 import urlopen
from json import load, dumps
import csv
from datetime import timedelta, date

def daterange(start_date, end_date):
    for n in range(int ((end_date - start_date).days)):
        yield start_date + timedelta(n)

#we have to iterate our api call through a for loop because the return size limit is 20 on the npr API
start_date = date(2016, 5, 1)
end_date = date(2016, 6, 1)
output_file_name = 'output.csv'
try:
    outputFile = open(output_file_name,'w') #write a csv
    outputWriter = csv.writer(outputFile)
    outputWriter.writerow(['Title','Date','Trump','Clinton','Sanders']) 
    outputFile.close()
    print "Initial File Created"
except:
    print "Initial Create File Error"
    outputFile.close()

for single_date in daterange( start_date, end_date):
	date = single_date.strftime("%Y-%m-%d")
	key = "demo" #go to npr.org/api/index to get your own key
	#date = '2016-06-14'
	url = "http://api.npr.org/query?id=1014&fields=title,storyDate&date=" + date + "&dateType=story&sort=dateAsc&output=JSON&apiKey=" + key


	#load json object from url
	response = urlopen(url)
	j = load(response)

	outputFile = open(output_file_name,'a') #append
	outputWriter = csv.writer(outputFile)	


	#create csv and input csvRows 
	try:
	    for story in j['list']['story']:
	        title = story['title']['$text']
	        outputWriter.writerow([title
	        ,date
			,(0 if title.find("Trump") == -1 else 1)
			,(0 if title.find("Clinton") == -1 else 1)
			,(0 if title.find("Sanders") == -1 else 1)
			])
			#story['storyDate']['$text']]) #this pulls the date from the json, but it's a bad format
	    print date + " Success... closing file"
	    outputFile.close()
	except:
	    print date + " Error... closing file"
	    outputFile.close()
	    
{% endhighlight %}


