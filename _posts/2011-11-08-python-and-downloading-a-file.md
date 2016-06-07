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

```python
import urllib2
import shutil

url = "http://www.python.com"
fname = "python.com.html"
request = urllib2.Request(url)
urlobj = urllib2.urlopen(request)
try:
	with open(fname, 'wb') as f:
		shutil.copyfileobj(urlobj, f)
finally:
	urlobj.close()
```

When we open this file in a editor, like <b>gedit</b> we see something like:
![python.com.html in gedit](/images/2011-11-08-python-and-downloading-a-file/python.com.html.png)
Now the script opens a local file <i>python.com.html</i> using python's <i>open</i> function and using <i>'wb'</i> statement. The <i>'w'</i> standsfor writeable, and <i>'b'</i> is binary mode. To read more on the built-in <i>open</i> function see this reference [Python open function](http://docs.python.org/library/functions.html#open), and we are using this with statement to open our local file, I'm not going into depth on this, want to know more about it. Please see reference [Python with statement](http://www.effbot.org/zone/python-with-statement.htm)

# Creating a class to handle our download
Now we know that our code works to download a file. But if we are going to download more than one file, it can be useful to put it in a function, or make a class that can handle all the downloads.
If you are writing on a larger script, where you need to download more than one file. So if you need to read up on pythonic classes, please see reference [Python classes](http://docs.python.org/tutorial/classes.html).
Here is my take on a class to handle downloads.

```python
# Filename: DownloadFile.py
from urllib2 import Request, urlopen, URLError, HTTPError
from shutil import copyfileobj
from os.path import isfile, exists

class DownloadFile:
	def __init__(self, url, tofile):
		self.link = url
		self.filename = tofile
		self.urlobj = None

	def __connect(self):
		returncode = True

		try:
			request = Request(self.link)
			self.urlobj = urlopen(request)
		except URLError, e:
			# Need to have some code, handle error code
			returncode = false
		except HTTPError, e:
			returncode = false

		return returncode

	def __write(self):
		try:
			with open(self.filename, 'wb') as f:
				copyfileobj(self.urlobj, f)
		finally:
			self.urlobj.close()

	def run(self):
		if not exists(self.filename) and not isfile(self.filename):
			gets = self.__connect()

			if gets:
				self.__write()
				return "Downloaded"
			else:
				return "Failed"
		else:
			return "Exists"

	def info(self):
		if self.fileobj is None:
			self.__connect()

		return self.urlobj.info()

if __name__ == "__main__":
	url = "https://upload.wikimedia.org/wikipedia/commons/b/bb/Megan-fox-coffee.jpg"
	lname = "Megan_Fox_Walking_with_Coffee.jpg"
	print "Will try to download a picture of Megan Fox:", 
	print DownloadFile(url,lname).run()
```

To run it just do:

```bash
python /path_to_file/DownloadFile.py
```

It should return something like

```bash
"Will try to download a picture of Megan Fox: Downloaded"
```

Now you can also import it into a script that can use the download class.

That is it for now, hope it can be useful for somebody :D.

