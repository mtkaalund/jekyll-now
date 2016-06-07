---
layout: post
title: Python and downloading a file
---
Disclamer: This is a old post, but it should still work on python 2.x

So when you are scripting in python, there comes a time when you want to download a file. This is more or less really good documented on the web, not really from the official python documentation. 
So there is two libraries designed to handle web requests (and there is at least one to handle <i>socket</i> requests).
So this two libraries is called <i>urllib</i> and <i>urllib2</i>, the one I would suggest using is <i>urllib2</i>, aleast for web downloads a file or web page.
So to make a simple start:

```python
import urllib2
url = "http://www.python.com"
request = urllib2.Request(url)
urlobj = urllib2.urlopen(request)
readpage = urlobj.read()
print readpage
```

So first we import <i>urllib2</i> into python... yeah I know that you properly know this, but just humor me.
The next we have defined is the url that we would like to download <i>[http://www.python.com](http://www.python.com)</i>, why would we download this web page, just to show it is possible.
Next we make a request for this url with (<i>urllib2.Request</i>), and after that we are going to make an url object with <i>urllib2.urlopen</i>. 
When it is done, we can now read the web page with <i>urllib2.read</i> and we are going to print it out.

This is fine for just printing it out on the terminal, but what if we want to write this to a file? Luckly there is a way:

