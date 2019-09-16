
## Python Web Scraping Using BeautifulSoup

(https://www.dataquest.io/blog/web-scraping-tutorial-python/)

When performing data science tasks, it's common to want to use data found on the internet. You'll usually be able to access this data in csv format, or via an Application Programming Interface (API). However, there are times when the data you want can only be accessed as part of a web page. In cases like this, you'll want to use a technique called web scraping to get the data from the web page into a format you can work with in your analysis.

We'll be performing web scraping using the BeautifulSoup library.

#### The components of a web page

When we visit a web page, our web browser makes a request to a web server. This request is called a `GET` request, since we're getting files from the server. The server then sends back files that tell our browser how to render the page for us. The files fall into a few main types:

+ HTML — contain the main content of the page.
+ CSS — add styling to make the page look nicer.
+ JS — Javascript files add interactivity to web pages.
+ Images — image formats, such as JPG and PNG allow web pages to show pictures.

After our browser receives all the files, it renders the page and displays it to us. There's a lot that happens behind the scenes to render a page nicely, but we don't need to worry about most of it when we're web scraping. When we perform web scraping, we're interested in the main content of the web page, so we look at the HTML.

#### HTML

HyperText Markup Language (HTML) is a language that web pages are created in. HTML isn't a programming language, like Python — instead, it's a markup language that tells a browser how to layout content. HTML allows you to do similar things to what you do in a word processor like Microsoft Word — make text bold, create paragraphs, and so on. Because HTML isn't a programming language, it isn't nearly as complex as Python.

Let's take a quick tour through HTML so we know enough to scrape effectively. HTML consists of elements called tags. The most basic tag is the `<html>` tag. This tag tells the web browser that everything inside of it is HTML. We can make a simple HTML document just using this tag:

```
<html>
</html>
```
Right inside an `html` tag, we put two other tags, the `head` tag, and the `body` tag. The main content of the web page goes into the `body` tag. The `head` tag contains data about the title of the page, and other information that generally isn't useful in web scraping:
```
<html>
<head>
</head>
<body>
</body>
</html>
```
You may have noticed above that we put the `head` and `body` tags inside the `html` tag. In HTML, tags are nested, and can go inside other tags.

We'll now add our first content to the page, in the form of the `p` tag. The `p` tag defines a paragraph, and any text inside the tag is shown as a separate paragraph:

```
<html>
<head>
</head>
<body>
<p>
Here's a paragraph of text!
</p>
<p>
Here's a second paragraph of text!
</p>
</body>
</html>
```
Tags have commonly used names that depend on their position in relation to other tags:

+ child — a child is a tag inside another tag. So the two `p` tags above are both children of the `body` tag.
+ parent — a parent is the tag another tag is inside. Above, the `html` tag is the parent of the `body` tag.
+ sibiling — a sibiling is a tag that is nested inside the same parent as another tag. For example, `head` and `body` are siblings, since they're both inside `html`. Both `p` tags are siblings, since they're both inside `body`.

We can also add properties to HTML tags that change their behavior:
```
<html>
<head>
</head>
<body>
<p>
Here's a paragraph of text!
<a href="https://www.dataquest.io">Learn Data Science Online</a>
</p>
<p>
Here's a second paragraph of text!
<a href="https://www.python.org">Python</a> </p>
</body></html>
```
In the above example, we added two `a` tags. `a` tags are links, and tell the browser to render a link to another web page. The `href` property of the tag determines where the link goes.

`a` and `p` are extremely common html tags. Here are a few others:

+ div — indicates a division, or area, of the page.
+ b — bolds any text inside.
+ i — italicizes any text inside.
+ table — creates a table.
+ form — creates an input form.

For a full list of tags, look [here](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

Before we move into actual web scraping, let's learn about the class and id properties. These special properties give HTML elements names, and make them easier to interact with when we're scraping. One element can have multiple classes, and a class can be shared between elements. Each element can only have one id, and an id can only be used once on a page. Classes and ids are optional, and not all elements will have them.

We can add classes and ids to our example:
```
<html>
<head>
</head>
<body>
<p class="bold-paragraph">
Here's a paragraph of text!
<a href="https://www.dataquest.io" id="learn-link">Learn Data Science Online</a>
</p>
<p class="bold-paragraph extra-large">
Here's a second paragraph of text!
<a href="https://www.python.org" class="extra-large">Python</a>
</p>
</body>
</html>
```
As you can see, adding classes and ids doesn't change how the tags are rendered at all.

#### The requests library

The first thing we'll need to do to scrape a web page is to download the page. We can download pages using the Python [requests library](https://2.python-requests.org//en/master/). The requests library will make a `GET` request to a web server, which will download the HTML contents of a given web page for us.

Let's try downloading a simple sample website, http://dataquestio.github.io/web-scraping-pages/simple.html. We'll need to first download it using the `requests.get` method.


```python
import requests
page = requests.get("http://dataquestio.github.io/web-scraping-pages/simple.html")
page
```




    <Response [200]>



After running our request, we get a `Response` object. This object has a `status_code` property, which indicates if the page was downloaded successfully:


```python
page.status_code
```




    200



A `status_code` of `200` means that the page downloaded successfully. We won't fully dive into status codes here, but a status code starting with a 2 generally indicates success, and a code starting with a 4 or a 5 indicates an error.

We can print out the HTML content of the page using the content property:


```python
page.content
```




    b'<!DOCTYPE html>\n<html>\n    <head>\n        <title>A simple example page</title>\n    </head>\n    <body>\n        <p>Here is some simple content for this page.</p>\n    </body>\n</html>'



#### Parsing a page with BeautifulSoup

As you can see above, we now have downloaded an HTML document.

We can use the `BeautifulSoup` library to parse this document, and extract the text from the `p` tag. We first have to import the library, and create an instance of the `BeautifulSoup` class to parse our document:


```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(page.content, 'html.parser')
```

We can now print out the HTML content of the page, formatted nicely, using the `prettify` method on the `BeautifulSoup` object:


```python
print(soup.prettify())
```

    <!DOCTYPE html>
    <html>
     <head>
      <title>
       A simple example page
      </title>
     </head>
     <body>
      <p>
       Here is some simple content for this page.
      </p>
     </body>
    </html>
    

As all the tags are nested, we can move through the structure one level at a time. We can first select all the elements at the top level of the page using the `children` property of `soup`. Note that children returns a list generator, so we need to call the `list` function on it:


```python
list(soup.children)
```




    ['html', '\n', <html>
     <head>
     <title>A simple example page</title>
     </head>
     <body>
     <p>Here is some simple content for this page.</p>
     </body>
     </html>]



The above tells us that there are two tags at the top level of the page — the initial `<!DOCTYPE html>` tag, and the `<html>` tag. There is a newline character (`\n`) in the list as well. Let's see what the type of each element in the list is:


```python
[type(item) for item in list(soup.children)]
```




    [bs4.element.Doctype, bs4.element.NavigableString, bs4.element.Tag]



As you can see, all of the items are `BeautifulSoup` objects. The first is a `Doctype` object, which contains information about the type of the document. The second is a `NavigableString`, which represents text found in the HTML document. The final item is a `Tag` object, which contains other nested tags. The most important object type, and the one we'll deal with most often, is the `Tag` object.

The `Tag` object allows us to navigate through an HTML document, and extract other tags and text. You can learn more about the various `BeautifulSoup` objects [here](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#kinds-of-objects).

We can now select the `html` tag and its children by taking the third item in the list:


```python
html = list(soup.children)[2]
```

Each item in the list returned by the children property is also a `BeautifulSoup` object, so we can also call the `children` method on `html`.

Now, we can find the children inside the `html` tag:


```python
list(html.children)
```




    ['\n', <head>
     <title>A simple example page</title>
     </head>, '\n', <body>
     <p>Here is some simple content for this page.</p>
     </body>, '\n']



As you can see above, there are two tags here, `head`, and `body`. We want to extract the text inside the `p` tag, so we'll dive into the `body`:


```python
body = list(html.children)[3]
```

Now, we can get the `p` tag by finding the children of the `body` tag:


```python
list(body.children)
```




    ['\n', <p>Here is some simple content for this page.</p>, '\n']



We can now isolate the `p` tag:


```python
p = list(body.children)[1]
```

Once we've isolated the tag, we can use the `get_text` method to extract all of the text inside the tag:


```python
p.get_text()
```




    'Here is some simple content for this page.'



#### Finding all instances of a tag at once

What we did above was useful for figuring out how to navigate a page, but it took a lot of commands to do something fairly simple. If we want to extract a single tag, we can instead use the `find_all` method, which will find all the instances of a tag on a page.


```python
soup = BeautifulSoup(page.content, 'html.parser')
soup.find_all('p')
```




    [<p>Here is some simple content for this page.</p>]



Note that `find_all` returns a list, so we’ll have to loop through, or use list indexing, it to extract text:


```python
soup.find_all('p')[0].get_text()
```




    'Here is some simple content for this page.'



If you instead only want to find the first instance of a tag, you can use the `find` method, which will return a single `BeautifulSoup` object:


```python
soup.find('p')
```




    <p>Here is some simple content for this page.</p>



#### Searching for tags by class and id

We introduced classes and ids earlier, but it probably wasn't clear why they were useful. Classes and ids are used by CSS to determine which HTML elements to apply certain styles to. We can also use them when scraping to specify specific elements we want to scrape. To illustrate this principle, we'll work with the following page:
```
<html>
<head>
<title>A simple example page</title>
</head>
<body>
<div>
<p class="inner-text first-item" id="first">
First paragraph.
</p>
<p class="inner-text">
Second paragraph.
</p>
</div>
<p class="outer-text first-item" id="second">
<b>
First outer paragraph.
</b>
</p>
<p class="outer-text">
<b>
Second outer paragraph.
</b>
</p>
</body>
</html>
```

We can access the above document at the URL http://dataquestio.github.io/web-scraping-pages/ids_and_classes.html. Let's first download the page and create a `BeautifulSoup` object:


```python
page = requests.get("http://dataquestio.github.io/web-scraping-pages/ids_and_classes.html")
soup = BeautifulSoup(page.content, 'html.parser')
soup
```




    <html>
    <head>
    <title>A simple example page</title>
    </head>
    <body>
    <div>
    <p class="inner-text first-item" id="first">
                    First paragraph.
                </p>
    <p class="inner-text">
                    Second paragraph.
                </p>
    </div>
    <p class="outer-text first-item" id="second">
    <b>
                    First outer paragraph.
                </b>
    </p>
    <p class="outer-text">
    <b>
                    Second outer paragraph.
                </b>
    </p>
    </body>
    </html>



Now, we can use the `find_all` method to search for items by `class` or by `id`. In the below example, we'll search for any `p` tag that has the class `outer-text`:


```python
soup.find_all('p', class_='outer-text')
```




    [<p class="outer-text first-item" id="second">
     <b>
                     First outer paragraph.
                 </b>
     </p>, <p class="outer-text">
     <b>
                     Second outer paragraph.
                 </b>
     </p>]



In the below example, we'll look for any tag that has the class `outer-text`:


```python
soup.find_all(class_="outer-text")
```




    [<p class="outer-text first-item" id="second">
     <b>
                     First outer paragraph.
                 </b>
     </p>, <p class="outer-text">
     <b>
                     Second outer paragraph.
                 </b>
     </p>]



We can also search for elements by `id`:


```python
soup.find_all(id="first")
```




    [<p class="inner-text first-item" id="first">
                     First paragraph.
                 </p>]



#### Using CSS Selectors

You can also search for items using CSS selectors. These selectors are how the CSS language allows developers to specify HTML tags to style. Here are some examples:

+ `p a` — finds all `a` tags inside of a `p` tag.
+ `body p a` — finds all `a` tags inside of a `p` tag inside of a `body` tag.
+ `html body` — finds all `body` tags inside of an `html` tag.
+ `p.outer-text` — finds all `p` tags with a class of `outer-text`.
+ `p#first` — finds all `p` tags with an id of `first`.
+ `body p.outer-text` — finds any `p` tags with a class of `outer-text` inside of a `body` tag.

You can learn more about CSS selectors [here](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors).

`BeautifulSoup` objects support searching a page via CSS selectors using the `select` method. We can use CSS selectors to find all the `p` tags in our page that are inside of a `div` like this:


```python
soup.select("div p")
```




    [<p class="inner-text first-item" id="first">
                     First paragraph.
                 </p>, <p class="inner-text">
                     Second paragraph.
                 </p>]



Note that the `select` method above returns a list of `BeautifulSoup` objects, just like `find` and `find_all`.

#### Example
We now know enough to download a page and start parsing it. In the below code, we:

+ Download the web page containing the forecast.
+ Create a `BeautifulSoup` class to parse the page.
+ Find the `div` with id `seven-day-forecast`, and assign to `seven_day`
+ Inside `seven_day`, find each individual forecast item.
+ Extract and print the first forecast item.

https://forecast.weather.gov/MapClick.php?lat=37.7772&lon=-122.4168#.XX-KXH--nIU


```python
page = requests.get("http://forecast.weather.gov/MapClick.php?lat=37.7772&lon=-122.4168")
soup = BeautifulSoup(page.content, 'html.parser')
seven_day = soup.find(id="seven-day-forecast")
forecast_items = seven_day.find_all(class_="tombstone-container")
tonight = forecast_items[0]
print(tonight.prettify())
```

    <div class="tombstone-container">
     <p class="period-name">
      Today
      <br/>
      <br/>
     </p>
     <p>
      <img alt="Today: Rain likely, mainly before 8am.  Cloudy through mid morning, then gradual clearing, with a high near 69. West wind 6 to 15 mph, with gusts as high as 20 mph.  Chance of precipitation is 60%. New precipitation amounts of less than a tenth of an inch possible. " class="forecast-icon" src="DualImage.php?i=ra&amp;j=few&amp;ip=60" title="Today: Rain likely, mainly before 8am.  Cloudy through mid morning, then gradual clearing, with a high near 69. West wind 6 to 15 mph, with gusts as high as 20 mph.  Chance of precipitation is 60%. New precipitation amounts of less than a tenth of an inch possible. "/>
     </p>
     <p class="short-desc">
      Rain Likely
      <br/>
      then Sunny
     </p>
     <p class="temp temp-high">
      High: 69 °F
     </p>
    </div>
    

As you can see, inside the forecast item tonight is all the information we want. There are 4 pieces of information we can extract:

+ The name of the forecast item — in this case, Tonight.
+ The description of the conditions — this is stored in the title property of img.
+ A short description of the conditions — in this case, Rain Likely.
+ The temperature high — in this case, 69 degrees.

We'll extract the name of the forecast item, the short description, and the temperature first, since they're all similar:


```python
period = tonight.find(class_="period-name").get_text()
short_desc = tonight.find(class_="short-desc").get_text()
temp = tonight.find(class_="temp").get_text()
print(period)
print(short_desc)
print(temp)
```

    Today
    Rain Likelythen Sunny
    High: 69 °F
    

Now, we can extract the title attribute from the `img` tag. To do this, we just treat the `BeautifulSoup` object like a dictionary, and pass in the attribute we want as a key:


```python
img = tonight.find("img")
desc = img['title']
print(desc)
```

    Today: Rain likely, mainly before 8am.  Cloudy through mid morning, then gradual clearing, with a high near 69. West wind 6 to 15 mph, with gusts as high as 20 mph.  Chance of precipitation is 60%. New precipitation amounts of less than a tenth of an inch possible. 
    

Now that we know how to extract each individual piece of information, we can combine our knowledge with css selectors and list comprehensions to extract everything at once.

In the below code, we:

+ Select all items with the class `period-name` inside an item with the class `tombstone-container` in `seven_day`.
+ Use a list comprehension to call the `get_text` method on each `BeautifulSoup` object.



```python
period_tags = seven_day.select(".tombstone-container .period-name")
periods = [pt.get_text() for pt in period_tags]
periods
```




    ['Today',
     'Tonight',
     'Tuesday',
     'TuesdayNight',
     'Wednesday',
     'WednesdayNight',
     'Thursday',
     'ThursdayNight',
     'Friday']



As you can see above, our technique gets us each of the period names, in order. We can apply the same technique to get the other 3 fields:


```python
short_descs = [sd.get_text() for sd in seven_day.select(".tombstone-container .short-desc")]
temps = [t.get_text() for t in seven_day.select(".tombstone-container .temp")]
descs = [d["title"] for d in seven_day.select(".tombstone-container img")]
print(short_descs)
print(temps)
print(descs)
```

    ['Rain Likelythen Sunny', 'Partly Cloudy', 'Sunny', 'IncreasingClouds', 'Partly Sunny', 'Mostly Clear', 'Sunny', 'Clear', 'Sunny']
    ['High: 69 °F', 'Low: 57 °F', 'High: 71 °F', 'Low: 59 °F', 'High: 69 °F', 'Low: 57 °F', 'High: 71 °F', 'Low: 58 °F', 'High: 75 °F']
    ['Today: Rain likely, mainly before 8am.  Cloudy through mid morning, then gradual clearing, with a high near 69. West wind 6 to 15 mph, with gusts as high as 20 mph.  Chance of precipitation is 60%. New precipitation amounts of less than a tenth of an inch possible. ', 'Tonight: Partly cloudy, with a low around 57. West wind 8 to 16 mph, with gusts as high as 21 mph. ', 'Tuesday: Sunny, with a high near 71. Light and variable wind becoming west 9 to 14 mph in the afternoon. ', 'Tuesday Night: Increasing clouds, with a low around 59. West wind 8 to 15 mph, with gusts as high as 20 mph. ', 'Wednesday: Partly sunny, with a high near 69. West wind 5 to 15 mph, with gusts as high as 20 mph. ', 'Wednesday Night: Mostly clear, with a low around 57.', 'Thursday: Sunny, with a high near 71.', 'Thursday Night: Clear, with a low around 58.', 'Friday: Sunny, with a high near 75.']
    

#### A teaser for a Pandas Dataframe


```python
import pandas as pd
weather = pd.DataFrame({
    "period": periods,
    "short_desc": short_descs,
    "temp": temps,
    "desc":descs
})
weather
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>period</th>
      <th>short_desc</th>
      <th>temp</th>
      <th>desc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Today</td>
      <td>Rain Likelythen Sunny</td>
      <td>High: 69 °F</td>
      <td>Today: Rain likely, mainly before 8am.  Cloudy...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Tonight</td>
      <td>Partly Cloudy</td>
      <td>Low: 57 °F</td>
      <td>Tonight: Partly cloudy, with a low around 57. ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tuesday</td>
      <td>Sunny</td>
      <td>High: 71 °F</td>
      <td>Tuesday: Sunny, with a high near 71. Light and...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>TuesdayNight</td>
      <td>IncreasingClouds</td>
      <td>Low: 59 °F</td>
      <td>Tuesday Night: Increasing clouds, with a low a...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Wednesday</td>
      <td>Partly Sunny</td>
      <td>High: 69 °F</td>
      <td>Wednesday: Partly sunny, with a high near 69. ...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>WednesdayNight</td>
      <td>Mostly Clear</td>
      <td>Low: 57 °F</td>
      <td>Wednesday Night: Mostly clear, with a low arou...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Thursday</td>
      <td>Sunny</td>
      <td>High: 71 °F</td>
      <td>Thursday: Sunny, with a high near 71.</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ThursdayNight</td>
      <td>Clear</td>
      <td>Low: 58 °F</td>
      <td>Thursday Night: Clear, with a low around 58.</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Friday</td>
      <td>Sunny</td>
      <td>High: 75 °F</td>
      <td>Friday: Sunny, with a high near 75.</td>
    </tr>
  </tbody>
</table>
</div>


