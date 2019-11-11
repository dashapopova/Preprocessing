
# Краулеры

**План**

1. Что такое краулеры?
2. Как написать простой краулер?
3. Блокировки и способы их обхода

## Что такое краулеры?

Краулеры - это боты/рпограммы, которые "ползают" по страницам сайта и собирают информацию. Все чаще использование таких программ запрещается правилами пользования сайтами, поэтому это формально нехорошо. Но так продолжают делать и это надо уметь. Запрещают по 2 основным причинам: не хотят делиться данными и боятся, что вы уроните сервер (если сайт маленький и сервер не очень, то это довольно легко). Поэтому нужно собирать данные аккуратно, чтобы вас а) не заблокировали по IP и б) вы не навредили серверу

## Как написать простой краулер?


```python
import requests
from pprint import pprint

session = requests.session()
```


```python
response = session.get('https://ru.wikipedia.org')
response.headers['X-Client-IP']
```




    '188.18.72.7'




```python
pprint(dict(response.headers))
```

    {'Accept-Ranges': 'bytes',
     'Age': '3397',
     'Backend-Timing': 'D=185045 t=1573383643718757',
     'Cache-Control': 'private, s-maxage=0, max-age=0, must-revalidate',
     'Connection': 'keep-alive',
     'Content-Encoding': 'gzip',
     'Content-Length': '26617',
     'Content-Type': 'text/html; charset=UTF-8',
     'Content-language': 'ru',
     'Date': 'Sun, 10 Nov 2019 11:57:22 GMT',
     'Last-Modified': 'Sun, 10 Nov 2019 11:00:27 GMT',
     'P3P': 'CP="See https://ru.wikipedia.org/wiki/Special:CentralAutoLogin/P3P '
            'for more info."',
     'Server': 'mw1333.eqiad.wmnet',
     'Server-Timing': 'cache;desc="hit-front"',
     'Strict-Transport-Security': 'max-age=106384710; includeSubDomains; preload',
     'Vary': 'Accept-Encoding,Cookie,Authorization',
     'X-Analytics': 'ns=0;page_id=4401;WMF-Last-Access=10-Nov-2019;WMF-Last-Access-Global=10-Nov-2019;https=1',
     'X-Cache': 'cp1083 pass, cp3060 hit/5, cp3062 hit/1473',
     'X-Cache-Status': 'hit-front',
     'X-Client-IP': '188.18.72.7',
     'X-Content-Type-Options': 'nosniff',
     'X-Powered-By': 'PHP/7.2.22-1+0~20190902.26+debian9~1.gbpd64eb7+wmf1',
     'X-Varnish': '1033826911, 388705147 384416680, 307640691 245377865'}
    

### Стратегии сбора данных


По сути краулеры выполняют сбор страниц (их html) как мы это делали на прошлом занятии, но делают они это циклами (или циклами циклов). Можно выделить разные стратегии сбора данных:
    
**По типу навигации**

1. Все страницы со ссылками имеют удобные номера ("https://ficbook.net/fanfiction/no_fandom/originals?p=2"), обычно просто p=(число) или page=(число). В этом случае вам нужно просто подставлять цифры
2. Страницы называются как-то не структурированно (например, по названиям блоков). Тут нужно собирать ссылки на эти страницы и потом по ним ходить и собирать конечные странички.

**По скорости обновления**

1. Если сайт довольно статичный по контенту (медленно появляются и удаляются материалы), то можно сначал собрать ссылки, а потом по ним ходить
2. Если сайт очень динамичный по контенту (например, объявления на крупном сайте), вам нужно при получении страничкии ссылок сразу их обходить, а потом переходить к следующей, потому что ко времени получения исчерпывающего списка ссылок по сайту многие будут уже удалены или недоступны



## Блокировки и способы их обхода

Для того, чтобы предотвратить автоматический сбор информации с некого сайта, применяются различные инструменты, которые определяют роботов и блокируют запросы с адресов, которые были классифицированы как роботы. Чтобы не заблокировали домашний/учебный ip, лучше сразу задуматься об этих мерах и предотвратить возможные проблемы. Кстати, Википедия не блокирует и можно спокойно скачивать без каких-либо проблем.

Чтобы их обойти, можно попробовать несколько инструментов:
1. time.sleep(x) - задержка между запросами, чтобы слишком большая скорость запросов не показалась подозрительной или ваши запросы не уронили сервер небольшого ресурса (например, региональной газеты)
2. time.sleep(случайный промежуток времени) - это более хитрая версия, когда время задержки - это случайное число из некоторого отрезка (модуль random)
3. изобразить браузер - при запросе отправляется информация о том, из какого приложения пришел запрос (например, Googlr Chrome), запросы сделанные из браузера больше похожи на человеческие, для этого нужно задать user-agent в параметрах (а его выбирать случайно с помощью fake_useragent)
4. использовать прокси - существуют ресурсы с бесплатными списками открытых прокси, через которые можно пропускать ваш запрос и сервер будет думать, что запросы приходят из разных мест (anonymous и elite классы прокси)

### Пауза между запросами


```python
import time

for _ in range(5):
    response = session.get('https://ru.wikipedia.org')
    print(response.headers['Date'])
    time.sleep(3)
```

    Sun, 10 Nov 2019 11:57:22 GMT
    Sun, 10 Nov 2019 11:57:25 GMT
    Sun, 10 Nov 2019 11:57:28 GMT
    Sun, 10 Nov 2019 11:57:32 GMT
    Sun, 10 Nov 2019 11:57:35 GMT
    

### Притвориться нормальным браузером


```python
from fake_useragent import UserAgent
ua = UserAgent(verify_ssl=False)

headers = {'User-Agent': ua.random}
print(headers)
response = session.get('https://ru.wikipedia.org', headers=headers)
```

    {'User-Agent': 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.93 Safari/537.36'}
    

### Пауза между запросами (случайное время)


```python
import random

for _ in range(5):
    response = session.get('https://ru.wikipedia.org')
    print(response.headers['Date'])
    time.sleep(random.uniform(1.1, 5.2))
```

    Sun, 10 Nov 2019 11:57:38 GMT
    Sun, 10 Nov 2019 11:57:42 GMT
    Sun, 10 Nov 2019 11:57:47 GMT
    Sun, 10 Nov 2019 11:57:50 GMT
    Sun, 10 Nov 2019 11:57:55 GMT
    

### Подключение через прокси

Адреса прокси можно взять со специальных сайтов, например, [http://www.gatherproxy.com](http://www.gatherproxy.com)


```python
known_proxy_ip = '180.246.205.208:57648'
proxy = {'http': known_proxy_ip, 'https': known_proxy_ip}
response = session.get('https://ru.wikipedia.org', proxies=proxy)
print(response.headers['X-Client-IP'])
```

    180.246.205.208
    

## Примеры

### Пример 1

Давайте обкачаем немного новостей с сайта вышки.

1. Страницы имеют вид "https://www.hse.ru/news/page1.html", поэтому можно просто идти циклом.
2. Достанем дату публикации, заголовок, краткое описание (из станицы со списком новостей), текст полной статьи и метки (из самой страницы новости)
3. Положим в базу


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

**Шаг 1. Найти страницы**


```python
page_number = 2
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




    '«Куратор выявляет то, что “разлито” в современности, но еще не сформулировано»'




```python
attrs = news[0].find('a').attrs
attrs
```




    {'href': '/news/edu/316171943.html',
     'class': ['link', 'link_dark2', 'no-visited']}




```python
href = news[0].find('a').attrs['href']
href
```




    '/news/edu/316171943.html'




```python
short_text = news[0].find('div', {'class': 'post__text'}).text
short_text
```




    'В этом году НИУ ВШЭ и Музей современного искусства «Гараж» открыли магистерскую программу «Практики кураторства в современном искусстве», которая готовит кураторов и координаторов выставок. Что представляет собой профессия куратора в искусстве, в интервью новостной службе рассказали преподаватели программы – куратор Музея «Гараж» Валентин Дьяконов и главный редактор портала «Артгид» Мария Кравцова.'




```python
pub_day = news[0].find('div', {'class': 'post-meta__day'}).text
pub_day
```




    '6'




```python
pub_month = news[0].find('div', {'class': 'post-meta__month'}).text
pub_month
```




    'ноя'




```python
pub_year = news[0].find('div', {'class': 'post-meta__year'}).text
pub_year
```




    '2019'



**Шаг 2. Научиться парсить страничку самой новости**


```python
url_one = 'http://www.hse.ru'+href
url_one
```




    'http://www.hse.ru/news/edu/316171943.html'




```python
req = session.get(url_one, headers={'User-Agent': ua.random})
page = req.text

soup = BeautifulSoup(page, 'html.parser')
```


```python
full_text = soup.find('div', {'class': 'post__content'}).text
full_text[:200]
```




    '«Куратор выявляет то, что “разлито” в современности, но еще не сформулировано»В этом году НИУ ВШЭ и Музей современного искусства «Гараж» открыли магистерскую программу\xa0«Практики кураторства в современ'




```python
full_text = soup.find('div', {'class': 'post__content'}).text
full_text[:200]
```




    '«Куратор выявляет то, что “разлито” в современности, но еще не сформулировано»В этом году НИУ ВШЭ и Музей современного искусства «Гараж» открыли магистерскую программу\xa0«Практики кураторства в современ'




```python
meta = soup.find('div', {'class': 'articleMeta'})

tags = meta.find_all('a', {'class': 'tag'})
tags = [t.text for t in tags]
tags
```




    ['студенты', 'магистратура', 'Практики кураторства в современном искусстве']



**Шаг 3. Оформляем нормально в функции**


```python
months = {
    value: key+1
    for key, value in enumerate(
        ['янв', 'фев', 'мар', 'апр', 'мая', 'июн', 'июл', 'авг', 'сен', 'окт', 'ноя', 'дек']
    )
}
```

Парсим информацию из страницы со списком новостей (блок одной новости)


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

Парсим отдельную страницу новости


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

**Шаг 4. Пишем в базу**

Надо завести словарь для тегов (сначала читаем из базы, а потом дозаписываем), множество виденных статей (чтобы при перезаупске не дублировать)


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




    [(96, 'магистратура'),
     (84, 'студенты'),
     (72, 'приемная кампания 2019'),
     (68, 'бакалавриат'),
     (67, 'репортаж о событии'),
     (51, 'достижения'),
     (50, 'выпускники'),
     (42, 'приглашение к участию'),
     (36, 'новое в ВШЭ'),
     (36, 'рейтинг вузов')]




```python
cur.execute("""
SELECT count(pub_month) as cnt, pub_month
    FROM texts
        GROUP BY pub_month
        ORDER BY cnt DESC;
""")
cur.fetchall()
```




    [(99, 10),
     (87, 9),
     (78, 7),
     (68, 4),
     (62, 8),
     (55, 11),
     (44, 5),
     (42, 6),
     (40, 3),
     (37, 12),
     (35, 2),
     (29, 1)]


