---
layout: post
title: Top 30 restaurants in Tripadvisor 
tags: [python, web scraping, beautiful soup, tripadvisor]
#image: /img/hello_world.jpeg
---

The extractation of information from web pages is a resource widely used in Data Science world. I usually use web scraping to add information that enriches my analytic dataset.

The web scraping process include two main steps: pull down the content of a webpage and parse and extract the information you need. There are dozens of packages out there to address these tasks. 

If you want to make a script which does not have to extract a lot of information, you don't mind the extraction speed and you don't want to make things harder on yourself, I recommend the use of *urllib* or *requests* and *Beautiful Soup* libraries.

If you need speed change *beautiful soup* for *lxml* library. And if you need something more powerful test *Selenium* (it takes control of your browser, which does all the work) or *Scrapy* (it creates a spider that crawls entire websites).

In this post I'm going to explain how to extract information from web pages using *urllib* or *requests* and *Beautiful Soup* libraries. Our goal is to extract the top 30 Málaga restaurants in Tripadvisor.  

### 1. Pull down the html code of a webpage

#### Option 1: *requests*

```python
import requests
url = "https://www.tripadvisor.es/Restaurants-g187438-Malaga_Costa_del_Sol_Province_of_Malaga_Andalucia.html"
r = requests.get(url)
```
You can see that the URL has been correctly encoded by printing the URL:
```python
print(r.url)
```
```python
https://www.tripadvisor.es/Restaurants-g187438-Malaga_Costa_del_Sol_Province_of_Malaga_Andalucia.html
```
You can check the response status code and check we don't made a bad request.

```python
print(r.status_code)
```
```python
200
```
You can read the content of the server’s response
```python
texturl = r.text # is a string that contains the web page source
```


#### Option 2: *urllib*
Employ urllib to get the HTML page of the url declared.
```python
import urllib
url = "https://www.tripadvisor.es/Restaurants-g187438-Malaga_Costa_del_Sol_Province_of_Malaga_Andalucia.html"
r = urllib.request.urlopen(url)
```
The server sent us an HTML document:

```python
texturl = r.read() # is a string that contains the web page source
```

### 2. Parse and extract the information you need

Once we have the HTML of the page, we parse the page into BeautifulSoup format and store in variable `soup`. 
 
```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(textourl,"lxml") 
```