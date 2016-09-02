---
layout: post
title:  "Twitter Streaming API: Hello World"
date:   2016-08-24 00:34:00 -0700
categories: blog update
tags: [Twitter API, Python]
---

This is my first attempt at using the Twitter Streaming API. When executed, this python script captures all tweets that include the string input.

{% highlight python %}

import tweepy
from tweepy.api import API
import time


#authorization
consumer_key = #<insert consumer key here>
consumer_secret = 	#insert consumer secret here>
access_token = #<insert access_token here>
access_token_secret = #<insert access_token_secret here>
key = tweepy.OAuthHandler(consumer_key, consumer_secret)
key.set_access_token(access_token, access_token_secret)

class Stream2Screen(tweepy.StreamListener):
    def __init__(self, api=None):
        self.api = api or API()
        self.n = 0
        self.m = 100

    def on_status(self, status):
        print  str(time.time()) + ' ' + status.user.screen_name.encode('utf8') + ' ' + status.text.encode('utf8')
        self.n = self.n+1
        if self.n < self.m: 
            return True
        else:
            return False

stream = tweepy.streaming.Stream(key, Stream2Screen())
stream.filter(track=['tableau'])

{% endhighlight %}

<br>

<div id="disqus_thread"></div>
<script>
    
    var disqus_config = function () {
        this.page.url = 'http://johntilelli.com/blog/update/2018/08/24/twitter-streaming-api-first-time.html';
        this.page.identifier = '/2016-08-24-twitter-streaming-api-first-time'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        
        s.src = '//www-johntilelli-com.disqus.com/embed.js';
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>

