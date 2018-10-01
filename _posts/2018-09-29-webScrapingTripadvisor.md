---
layout: post
title: Extract top 30 restaurants in Tripadvisor 
tags: [python, web scraping, beautiful soup, tripadvisor]
#image: /img/hello_world.jpeg
---

The extractation of information from web pages is a resource widely used in Data Science world. I usually use web scraping to add information that enriches my analytic dataset.

The web scraping process include two main steps: pull down the content of a webpage and extract the information you need. There are dozens of packages out there to address these tasks. 

If you want to make a script which does not have to extract a lot of information, you don't mind the extraction speed and you don't want to make things harder on yourself, I recommend the use of *urllib* or *requests* and *Beautiful Soup* libraries.

If you need speed change *beautiful soup* for *lxml* library. And if you need something more powerful test *Selenium* (it takes control of your browser, which does all the work) or *Scrapy* (it creates a spider that crawls entire websites).

In this post I'm going to explain how to extract information from web pages using *urllib* or *requests* and *Beautiful Soup* libraries. 

I will use as an example the extraction of the best 30 restaurants in Malaga on Tripadvisor.

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
You can view the response status code and check we don't made a bad request.

```python
print(r.status_code)
```
```python
200
```
You can also read the content of the server’s response (all the html code of the webpage).
```python
texturl = r.text # is a string that contains the web page source
```


#### Option 2: *urllib*
Another option is to employ urllib to get the HTML page of the url declared.
```python
import urllib
url = "https://www.tripadvisor.es/Restaurants-g187438-Malaga_Costa_del_Sol_Province_of_Malaga_Andalucia.html"
r = urllib.request.urlopen(url)
```
The server sent us an HTML document (a string variable that contains the web page source):

```python
texturl = r.read() 
```

### 2. Parse and extract the information you need

Once we have the HTML of the page, we parse the page into BeautifulSoup format and store in variable `soup`. 
 
```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(textourl,"lxml") 
```

The soup object has the find() method, that returns only the first element matching the given criteria. You can search for the exact string value of the class attribute or the id attribute. These attributes can be a string, a regular expression, a function. 

The soup objects present also find_all() method which returns a collection of elements: all the tags which match the given criteria. 


#### *Restaurants names*

Using the page inspector tool of the browser, we find that the first restaurant name has the class *property_title*.

![alt text](/assets/2018-09-29-bs4/restaurante.JPG)

This code finds the first element whose class tag is "property_title".

```python
soup.find(class_="property_title")
```

```html
<a class="property_title" href="/Restaurant_Review-g187438-d13864986-Reviews-Luxalad-Malaga_Costa_del_Sol_Province_of_Malaga_Andalucia.html" onclick="ta.restaurant_list_tracking.clickDetailTitle('/Restaurant_Review-g187438-d13864986-Reviews-Luxalad-Malaga_Costa_del_Sol_Province_of_Malaga_Andalucia.html','tags_category_tag_restaurants','13864986','1','1');" target="_blank">
Luxalad
</a>
```
As we only want the text part of the tag, we use the get_text() method.

```python
soup.find(class_="property_title").get_text()
```
```html
'\nLuxalad\n'
```
We use parameter *strip = True*, to strip whitespace from the beginning and end of each bit of text:

```python
soup.find(class_="property_title").get_text(strip=True)
```
```html
'Luxalad'
```
We use method *find_all* to obtain the names of all the restaurants in the webpage.
```python
[i.get_text(strip=True) for i in soup.find_all(class_="property_title")]
```

#### *Bubble ratings*

We repeat the process and using the page inspector tool of the browser, we find that the bubble ratings have the class *ui_bubble_rating*.

![alt text](/assets/2018-09-29-bs4/bubble.JPG)

```python
bubble = soup.find_all(class_="ui_bubble_rating")
len(bubble)
```
The length of the list is greater than thirty, the number of restaurants we are looking for. If we inspect the html code again, we realize the recommended restaurants at the top of the page have the same label as the restaurants in the ranking. 

![alt text](/assets/2018-09-29-bs4/topRest.JPG)

We look for differences between these two groups of restaurans and we realize the restaurants in the ranking present their information grouped in a block element with *shortSellDetails* as div name. So we limit the search of bubble rating elements to blocks with class *shortSellDetails*. 

![alt text](/assets/2018-09-29-bs4/comparaciondetail.JPG)

```python
[i.find(class_="ui_bubble_rating") for i in soup.find_all("div",class_="shortSellDetails")]
```
```html
[<span alt="5 de 5 burbujas" class="ui_bubble_rating bubble_50"></span>,
 <span alt="5 de 5 burbujas" class="ui_bubble_rating bubble_50"></span>,
 <span alt="5 de 5 burbujas" class="ui_bubble_rating bubble_50"></span>,
 <span alt="5 de 5 burbujas" class="ui_bubble_rating bubble_50"></span>,.....]
 ```

Now we have the bubble ratings of the top 30 restaurants, and we can extract the value with get() method. 

```python
[i.find(class_="ui_bubble_rating").get("alt") for i in soup.find_all("div",class_="shortSellDetails")]
```

#### *Number of reviews*

![alt text](/assets/2018-09-29-bs4/reviewCount.png)


```python
[i.find(class_="reviewCount")for i in soup.find_all("div",class_="shortSellDetails")]
```
As we only want the text part of the tag, we use the get_text() method.

```python
[i.find(class_="reviewCount").get_text(strip=True)
 for i in soup.find_all("div",class_="shortSellDetails")]
```
#### *Ranking position*

![alt text](/assets/2018-09-29-bs4/posicion.png)

```python
[i.find(class_="popIndex rebrand popIndexDefault").get_text(strip=True)
 for i in soup.find_all("div",class_="shortSellDetails")]
```
Extracting the position as a number.

```python
[int(i.find(class_="popIndex rebrand popIndexDefault").get_text(strip=True).split(" de ")[0].replace(".", "")) 
 for i in soup.find_all("div",class_="shortSellDetails")]
```

#### *Price category*

![alt text](/assets/2018-09-29-bs4/itemprice.png)

```python
[i.find(class_="item price")
 for i in soup.find_all("div",class_="shortSellDetails")]
```
As we only want the text part of the tag, we use the get_text() method.

```python
[i.find(class_="item price").get_text(strip=True) for i in soup.find_all("div",class_="shortSellDetails")]
```


#### *Food category*

![alt text](/assets/2018-09-29-bs4/tipoComida.JPG)

We see that one restaurant present different food categories.
```python
[i.get_text() for i in soup.find("div",class_="shortSellDetails").find_all(class_="item cuisine")]
```

#### *Final function*

We join in a function all the individual extractions. In addition, we consider the possibility that the fields could be empty.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

def get_tripadvisor(URL):
    # Name city from url
    nameCity = URL.split("-")[2].split("_")[0]
    # Connection
    peticion_restaurantes = requests.get(URL)
    textourl = peticion_restaurantes.text # is a string that contains the web page source
    # Connection ok
    if peticion_restaurantes.status_code == 200:
        
        # Beautiful soup object
        soup = BeautifulSoup(textourl,"lxml") 
        
        # Initialize empty lists
        name = []
        position = []
        rating = []
        numReview = []
        euros = []
        food = []
        
        # Only look for info in bock with class shortSellDetails
        for sec in soup.find_all(class_="shortSellDetails"):
            
            # Avoid sponsored restaurants
            patro = sec.find(class_="ui_merchandising_pill")
        
            if(patro == None):
                # position
                if sec.find(class_="popIndex rebrand popIndexDefault") is not None:
                    pos = sec.find(class_="popIndex rebrand popIndexDefault").get_text(strip=True) # extract
                    pos = int(pos.split(" de ")[0].replace(".", "")) # processing
                else:
                    pos=""
                position.append(pos) # append
            
                # restaurant name
                nam = sec.find(class_="property_title").get_text(strip=True) # extract
                name.append(nam)      
         
        
                # rating
                if sec.find(class_="ui_bubble_rating") is not None:
                    rate = sec.find(class_="ui_bubble_rating").get("alt") # extract
                    rate = float(rate.split(" de ")[0].replace(",","."))  # processing
                else:
                    rate = 0
                rating.append(rate)
            
                # number or reviews
                if sec.find(class_="reviewCount") is not None:
                    reviews = sec.find(class_="reviewCount").get_text(strip=True) # extract
                    reviews = int(reviews.replace("opiniones", "").replace("opinión","").replace(".", ""))  # processing
                else:
                    reviews = 0
                numReview.append(reviews)                
                     
                # euroscategory
                if sec.find(class_="item price") is not None:
                    price = sec.find(class_="item price").get_text(strip=True)  
                else:
                    price = " "
                euros.append(price)
            
                # food category
                typefood = [secfood.get_text() for secfood in sec.find_all(class_="item cuisine")] # extract
                typefood = ','.join(typefood) # processing
                food.append(typefood)      
          
            
                df = pd.DataFrame({'restaurants':name,"ratings":rating,"number_reviews":numReview, "price_category":euros,
                          'food_category':food,},
                         columns = ["restaurants","ratings","number_reviews","price_category","food_category"], index = position)
                df.index.name = nameCity
        return(df)       
    else:
        return(pd.DataFrame())
```