
# Crawlers

**Our plan for today:**

1. What is a crawler?
2. How can we create a simple crawler?
3. How to avoid being blocked?

## What is a crawler?

A crawler is a program that crowls across the webpages and collects information. Use it well: don't use it to download the data that the webpage creators don't want you to download; avoid getting your ID blocked; don't overload the server.

## How to create a simple crawler?


```python
import requests
from pprint import pprint

session = requests.session()
```


```python
response = session.get('https://ru.wikipedia.org')
response.headers['X-Client-IP']
```




    '94.228.207.11'




```python
pprint(dict(response.headers))
```

    {'Accept-Ranges': 'bytes',
     'Age': '825',
     'Cache-Control': 'private, s-maxage=0, max-age=0, must-revalidate',
     'Connection': 'keep-alive',
     'Content-Encoding': 'gzip',
     'Content-Language': 'ru',
     'Content-Length': '26513',
     'Content-Type': 'text/html; charset=UTF-8',
     'Date': 'Tue, 06 Oct 2020 15:00:39 GMT',
     'Last-Modified': 'Tue, 06 Oct 2020 15:00:32 GMT',
     'NEL': '{ "report_to": "wm_nel", "max_age": 86400, "failure_fraction": 0.05, '
            '"success_fraction": 0.0}',
     'P3p': 'CP="See https://ru.wikipedia.org/wiki/Special:CentralAutoLogin/P3P '
            'for more info."',
     'Report-To': '{ "group": "wm_nel", "max_age": 86400, "endpoints": [{ "url": '
                  '"https://intake-logging.wikimedia.org/v1/events?stream=w3c.reportingapi.network_error&schema_uri=/w3c/reportingapi/network_error/1.0.0" '
                  '}] }',
     'Server': 'mw2240.codfw.wmnet',
     'Server-Timing': 'cache;desc="hit-front"',
     'Strict-Transport-Security': 'max-age=106384710; includeSubDomains; preload',
     'Vary': 'Accept-Encoding,Cookie,Authorization',
     'X-Cache': 'cp3060 miss, cp3052 hit/319',
     'X-Cache-Status': 'hit-front',
     'X-Client-IP': '94.228.207.11',
     'X-Content-Type-Options': 'nosniff',
     'X-Request-Id': 'd3cc8eb2-2760-4e6f-b351-6848c597e693'}
    

### Strategies of data collection


Basically, crawlers are collecting webpages (their html) in cycles, or cycles of cycles. Some strategies of data collection:
    
**By navigation type**

1. All the webpages have convenient numbers ("https://ficbook.net/fanfiction/no_fandom/originals?p=2"), usually, p=(a number) or page=(a number). Then, you just have to go through relevant numbers.
2. Webpages are named somehow. Then, you need to collect the links to the relevant webpages and then go through them to collect the data.

**By the speed of updating**

1. If the website is updated slowly, you can first collect the links to the webpages and then go through them
2. If the website is updated fast, you need to collect the data right after you got the link and then move on to a new web page.



## Avoiding getting blocked

Btw, Wikipedia doesn't block downloads, can be used as a source of linguistic data

### A pause between requests


```python
import time

for _ in range(5):
    response = session.get('https://ru.wikipedia.org')
    print(response.headers['Date'])
    time.sleep(3)
```

### To present yourself as a well-respected browser


```python
from fake_useragent import UserAgent
ua = UserAgent(verify_ssl=False)

headers = {'User-Agent': ua.random}
print(headers)
response = session.get('https://ru.wikipedia.org', headers=headers)
```

    {'User-Agent': 'Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.2 Safari/537.36'}
    

### A pause between requests (random time)


```python
import random

for _ in range(5):
    response = session.get('https://ru.wikipedia.org')
    print(response.headers['Date'])
    time.sleep(random.uniform(1.1, 5.2))
```

### Use a proxy

There are websites where you can get a proxy address, e.g., [http://www.gatherproxy.com](http://www.gatherproxy.com)


```python
known_proxy_ip = '180.246.205.208:57648'
proxy = {'http': known_proxy_ip, 'https': known_proxy_ip}
response = session.get('https://ru.wikipedia.org', proxies=proxy)
print(response.headers['X-Client-IP'])
```

    180.246.205.208
    

## An example

Let's download some news from the HSE website!

1. The webpages have the following structure: "https://www.hse.ru/news/page1.html", we can loop through them.
2. Let's extract the publication date, the title, a short description, the full text of the publication, tags.
3. Put the data we collected into a database.


```python
import sqlite3
from html import unescape
from bs4 import BeautifulSoup
import re
```


```python
conn = sqlite3.connect('hse_news.db')
cur = conn.cursor()
```


```python
cur.execute("""
CREATE TABLE IF NOT EXISTS texts 
(id int PRIMARY KEY, hse_id text, pub_year int, pub_month int, 
pub_day int, title text, short_text text, full_text text)
""")

cur.execute("""
CREATE TABLE IF NOT EXISTS tags 
(id int PRIMARY KEY, tag_name text) 
""")

cur.execute("""
CREATE TABLE IF NOT EXISTS text_to_tag 
(id INTEGER PRIMARY KEY AUTOINCREMENT, id_text int, id_tag int) 
""")

conn.commit()
conn.close()
```

**Step 1. Finding the webpages**


```python
page_number = 1
url = f'https://www.hse.ru/news/page{page_number}.html'
req = session.get(url, headers={'User-Agent': ua.random})
page = req.text
```


```python
soup = BeautifulSoup(page, 'html.parser')
```


```python
news = soup.find_all('div', {'class': 'post_first'})
```


```python
title = news[0].find('a').text
title
```




    'Стартовал прием заявок на первый Всероссийский чемпионат сочинений «Своими словами»'




```python
attrs = news[0].find('a').attrs
attrs
```




    {'href': '/news/admission/405119387.html',
     'class': ['link', 'link_dark2', 'no-visited']}




```python
href = news[0].find('a').attrs['href']
href
```




    '/news/admission/405119387.html'




```python
short_text = news[0].find('div', {'class': 'post__text'}).text
short_text
```




    'До 15 ноября ученики 9-11 классов могут зарегистрироваться на онлайн-этап чемпионата, который пройдет в формате мультимедийного теста в личном кабинете участника. Чемпионат сочинений – это не типичная олимпиада или проверка на грамотность в рамках школьной программы. Задания не требуют специальной подготовки по типовым заданиям. Участникам необходимо просто рассказать о том, что им интересно, своими словами.'




```python
pub_day = news[0].find('div', {'class': 'post-meta__day'}).text
pub_day
```




    '6'




```python
pub_month = news[0].find('div', {'class': 'post-meta__month'}).text
pub_month
```




    'окт'




```python
pub_year = news[0].find('div', {'class': 'post-meta__year'}).text
pub_year
```




    '2020'



**Step 2. Learn how to parse the webpage of the news article**


```python
url_one = 'http://www.hse.ru'+href
url_one
```




    'http://www.hse.ru/news/admission/405119387.html'




```python
req = session.get(url_one, headers={'User-Agent': ua.random})
page = req.text

soup = BeautifulSoup(page, 'html.parser')
```


```python
full_text = soup.find('div', {'class': 'post__content'}).text
full_text[:200]
```




    'Стартовал прием заявок на первый Всероссийский чемпионат сочинений «Своими словами»© Всероссийский чемпионат сочинений «Своими словами»До 15 ноября ученики 9-11 классов могут зарегистрироваться на онл'




```python
full_text = soup.find('div', {'class': 'post__content'}).text
full_text[:200]
```




    'Стартовал прием заявок на первый Всероссийский чемпионат сочинений «Своими словами»© Всероссийский чемпионат сочинений «Своими словами»До 15 ноября ученики 9-11 классов могут зарегистрироваться на онл'




```python
meta = soup.find('div', {'class': 'articleMeta'})

tags = meta.find_all('a', {'class': 'tag'})
tags = [t.text for t in tags]
tags
```




    ['новое в ВШЭ', 'приглашение к участию', 'довузовская подготовка']



**Step 3. Reformulating the steps in terms of functions**


```python
months = {
    value: key+1
    for key, value in enumerate(
        ['янв', 'фев', 'мар', 'апр', 'мая', 'июн', 'июл', 'авг', 'сен', 'окт', 'ноя', 'дек']
    )
}
```

To parse the information from the webpage with a list of news articles (for one block):


```python
def parse_first_level_info(one_block):
    block = {}
    block['title'] = one_block.find('a').text
    block['href'] = one_block.find('a').attrs['href']
    block['short_text'] = one_block.find('div', {'class': 'post__text'}).text
    block['pub_day'] = int(one_block.find('div', {'class': 'post-meta__day'}).text)
    block['pub_month'] = months[one_block.find('div', {'class': 'post-meta__month'}).text]
    block['pub_year'] = int(one_block.find('div', {'class': 'post-meta__year'}).text)
    return block
```

To parse the webpage of a news article:


```python
def parse_second_level_info(block):
    url_one = 'http://www.hse.ru' + block['href']
    req = session.get(url_one, headers={'User-Agent': ua.random})
    page = req.text
    soup = BeautifulSoup(page, 'html.parser')
    block['full_text'] = soup.find('div', {'class': 'post__content'}).text
    meta = soup.find('div', {'class': 'articleMeta'})
    tags = meta.find_all('a', {'class': 'tag'})
    block['tags'] = [t.text for t in tags]     
    return block
```


```python
regex_hse_id = re.compile('/([0-9]*?).html')
```


```python
def get_nth_page(page_number):
    url = f'https://www.hse.ru/news/page{page_number}.html'
    req = session.get(url, headers={'User-Agent': ua.random})
    page = req.text
    soup = BeautifulSoup(page, 'html.parser')
    news = soup.find_all('div', {'class': 'post'})
    blocks = []
    for n in news:
        try:
            blocks.append(parse_first_level_info(n))
        except Exception as e:
            print(e)
    result = []
    for b in blocks:
        if b['href'].startswith('/'):
            idx = regex_hse_id.findall(b['href'])[0]
            if idx not in seen_news:
                try:
                    res = parse_second_level_info(b)
                    res['hse_id'] = idx
                    result.append(res)
                except Exception as e:
                    print(e)
            else:
                print('Seen', b['href'])
    return result
```

**Step 4. Putting the data into a database**

We need to create a dictionary for tags, a set of the articles we have seen (to not duplicate)


```python
def write_to_db(block):
    tags = []
    for tag in block['tags']:
        if tag in db_tags:
            tags.append(db_tags[tag])
        else:
            db_tags[tag] = len(db_tags) + 1 
            cur.execute('INSERT INTO tags VALUES (?, ?)', (len(db_tags), tag))
            conn.commit()
            tags.append(db_tags[tag])
    text_id = len(seen_news) + 1
    cur.execute(
        'INSERT INTO texts VALUES (?, ?, ?, ?, ?, ?, ?, ?)',
        (text_id, block['hse_id'],
         block['pub_year'], block['pub_month'], block['pub_day'],
         block['title'], block['short_text'], block['full_text'])
    )
    tags = [(text_id, t) for t in tags]
    cur.executemany(
        'INSERT INTO text_to_tag (id_text, id_tag) VALUES (?, ?)',
        tags
    )
    conn.commit()
    seen_news.add(block['hse_id'])
```


```python
conn = sqlite3.connect('hse_news.db')
cur = conn.cursor()
cur.execute('SELECT tag_name, id FROM tags')

db_tags = {}
for name, idx in cur.fetchall():
    db_tags[name] = idx

cur.execute('SELECT hse_id FROM texts')
seen_news = set(i[0] for i in cur.fetchall())
```


```python
from tqdm.auto import tqdm
```


```python
def run_all(n_pages):
    for i in tqdm(range(n_pages)):
        blocks = get_nth_page(i+1)
        for block in blocks:
            write_to_db(block)
```


```python
run_all(100)
```


    HBox(children=(IntProgress(value=0), HTML(value='')))


    Seen /news/edu/377605768.html
    Seen /news/admission/299677085.html
    Seen /news/edu/359798781.html
    Seen /news/edu/316414634.html
    Seen /news/edu/316411087.html
    Seen /news/edu/316331850.html
    Seen /news/admission/316321399.html
    Seen /news/edu/316184838.html
    Seen /news/edu/316181564.html
    Seen /news/edu/316171943.html
    Seen /news/edu/316085154.html
    Seen /news/edu/315366464.html
    Seen /news/admission/315357192.html
    Seen /news/edu/315353288.html
    Seen /news/life/315315228.html
    Seen /news/science/315206335.html
    Seen /news/expertise/315196601.html
    Seen /news/expertise/315184185.html
    Seen /news/expertise/315170202.html
    Seen /news/edu/315092890.html
    Seen /news/science/315081605.html
    Seen /news/community/315061797.html
    Seen /news/edu/314949862.html
    Seen /news/edu/314949875.html
    Seen /news/life/314934132.html
    Seen /news/science/314837885.html
    Seen /news/edu/314807712.html
    Seen /news/edu/314690414.html
    Seen /news/edu/314571117.html
    Seen /news/science/314384109.html
    Seen /news/science/314268319.html
    Seen /news/expertise/314138976.html
    Seen /news/edu/314132648.html
    Seen /news/science/314127600.html
    Seen /news/community/314107805.html
    Seen /news/edu/313263609.html
    Seen /news/edu/312808398.html
    Seen /news/edu/312546181.html
    Seen /news/edu/312386730.html
    Seen /news/science/312290445.html
    Seen /news/community/311982209.html
    Seen /news/science/311948131.html
    Seen /news/edu/311630253.html
    Seen /news/edu/311613915.html
    Seen /news/edu/311622874.html
    Seen /news/science/311550312.html
    Seen /news/edu/311536327.html
    Seen /news/life/310840029.html
    Seen /news/science/310836397.html
    Seen /news/310739455.html
    Seen /news/science/310732009.html
    Seen /news/edu/310732443.html
    Seen /news/expertise/310656474.html
    Seen /news/life/310654188.html
    Seen /news/edu/310054804.html
    Seen /news/community/309431310.html
    Seen /news/community/309370747.html
    Seen /news/admission/309354863.html
    Seen /news/edu/309356830.html
    Seen /news/edu/309353064.html
    Seen /news/edu/309248739.html
    Seen /news/edu/309144820.html
    Seen /news/edu/309131755.html
    Seen /news/edu/309058740.html
    Seen /news/science/309053802.html
    Seen /news/edu/309027309.html
    Seen /news/edu/308944619.html
    Seen /news/edu/308276109.html
    Seen /news/science/308211791.html
    Seen /news/edu/308200408.html
    Seen /news/308126518.html
    Seen /news/science/308122550.html
    Seen /news/expertise/308048719.html
    Seen /news/life/308038740.html
    Seen /news/community/307961768.html
    Seen /news/edu/307959377.html
    Seen /news/edu/307233013.html
    Seen /news/edu/307230429.html
    Seen /news/science/307217525.html
    Seen /news/edu/307157278.html
    Seen /news/edu/307142607.html
    Seen /news/edu/307082961.html
    Seen /news/edu/307081965.html
    Seen /news/community/307077549.html
    Seen /news/edu/307009583.html
    Seen /news/306997603.html
    Seen /news/science/306993204.html
    Seen /news/life/306928716.html
    Seen /news/science/306917338.html
    Seen /news/306244698.html
    Seen /news/life/306173624.html
    Seen /news/edu/306028434.html
    Seen /news/edu/306019457.html
    Seen /news/science/305941917.html
    
    


```python
cur.execute("""
SELECT count(text_to_tag.id) as cnt, tags.tag_name 
    FROM text_to_tag 
        JOIN tags ON tags.id = text_to_tag.id_tag 
            GROUP BY text_to_tag.id_tag 
            ORDER BY cnt DESC
            LIMIT 10;
""")
cur.fetchall()
```




    [(277, 'студенты'),
     (205, 'новое в ВШЭ'),
     (204, 'магистратура'),
     (201, 'идеи и опыт'),
     (201, 'репортаж о событии'),
     (195, 'исследования и аналитика'),
     (147, 'бакалавриат'),
     (145, 'достижения'),
     (131, 'приглашение к участию'),
     (128, 'онлайн-образование')]




```python
cur.execute("""
SELECT count(pub_month) as cnt, pub_month
    FROM texts
        GROUP BY pub_month
        ORDER BY cnt DESC;
""")
cur.fetchall()
```




    [(166, 9),
     (159, 4),
     (152, 7),
     (127, 8),
     (125, 6),
     (112, 10),
     (104, 5),
     (93, 3),
     (86, 12),
     (83, 11),
     (78, 2),
     (67, 1)]


