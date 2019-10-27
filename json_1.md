
# JSON

## json format

**JSON** -- is a file format that uses human-readable text to transmit data objects. 

JSON stands for *JavaScript Object Notation.*

JSON is computer- and human-friendly.

JSON is a language-independent data format. It was derived from JavaScript, but many modern programming languages include code to generate and parse JSON-format data.

JSON is more compact than XML. 

![](https://pics.me.me/json-statham-new-json-object-44070723.png)

## Data types and syntax

A JSON string can contain an __object__, then it starts with a `{` and ends with a `}`. Such an object resembles a python *dictionary*: it consists of attribute–value pairs devided by a comma. For example:


```python
{"first_name": "Guido", "last_name":"Rossum"}
```

A JSON string can contain an __array__, then it starts with a `[` and ends with a `]`. This object resembles a python array, all the values are separated by a comma. For example:


```python
["Guido van Rossum", "Diana Clarke", "Naomi Ceder", "Van Lindberg", "Ewa Jodlowska"]
```

JSON's basic data types are:
* number (an integer or a float)
* string (strings are delimited with double-quotation marks and support a backslash escaping syntax)
* boolean (`true` or `false`)
* array (an ordered list of zero or more values, each of which may be of any type. Arrays use square bracket notation with comma-separated elements)
* object (in {})
* null (an empty value, using the word null)

To include special symbols, use `\`, e.g., `\"` or `\r\n`. If you want to, you can find all the rules here: http://www.json.org/

One could mistake a json string for a python data type, e.g., a dictionary. But it is not the case. 
First, json is not a code, it is a text. Second, not all valid python objects form a valid json. For example, the following object is not a valid json, but it is a valid python dictionary: `{(1, 'a'): u'12345'}`. (Can you think of more examples?)

Another example of a longer json string:


```python
{"organisation": "Python Software Foundation",
 "officers": [
            {"first_name": "Guido", "last_name":"Rossum", "position":"president"},
            {"first_name": "Diana", "last_name":"Clarke", "position":"chair"},
            {"first_name": "Naomi", "last_name":"Ceder", "position":"vice chair"},
            {"first_name": "Van", "last_name":"Lindberg", "position":"vice chair"},
            {"first_name": "Ewa", "last_name":"Jodlowska", "position":"director of operations"}
            ],
"type": "non-profit",
"country": "USA",
"founded": 2001,
"members": 244,
"budget": 750000,
"url": "www.python.org/psf/"}
```

## json module

Python has a standard `json` module. You will most likely need the following functions:

* `loads`  - to convert a json string into a python object -- a dictionary or an array. This function takes one obligatory argument -- a json string.
* `dumps`  - to convert a python dictionary or an array into a json string. This function takes one obligatory argument -- a dictionary or an array.
* `load` - to read a json from a file and convert it into a python object. This function takes two obligatory arguments -- a json string and a file.
* `dump` - to convert a python object into a json and save it into a file. This function takes two obligatory arguments - a file and a python object.

The word "file" refers to any file-like object, to anything that we can apply the `.read()` method to.

## An example

Let's convert our json string into a python object:


```python
json_string = """{"organisation": "Python Software Foundation",
                 "officers": [
                            {"first_name": "Guido", "last_name":"Rossum", "position":"president"},
                            {"first_name": "Diana", "last_name":"Clarke", "position":"chair"},
                            {"first_name": "Naomi", "last_name":"Ceder", "position":"vice chair"},
                            {"first_name": "Van", "last_name":"Lindberg", "position":"vice chair"},
                            {"first_name": "Ewa", "last_name":"Jodlowska", "position":"director of operations"}
                            ],
                "type": "non-profit",
                "country": "USA",
                "founded": 2001,
                "members": 244,
                "budget": 750000,
                "url": "www.python.org/psf/"}"""
```


```python
import json

data = json.loads(json_string)
print(type(data))  # let's print out the type of the object to make sure that that's a dictionary and not a string
```

    <class 'dict'>
    


```python
from pprint import pprint

pprint(data) # let's take a look at the dictionary
```

    {'budget': 750000,
     'country': 'USA',
     'founded': 2001,
     'members': 244,
     'officers': [{'first_name': 'Guido',
                   'last_name': 'Rossum',
                   'position': 'president'},
                  {'first_name': 'Diana',
                   'last_name': 'Clarke',
                   'position': 'chair'},
                  {'first_name': 'Naomi',
                   'last_name': 'Ceder',
                   'position': 'vice chair'},
                  {'first_name': 'Van',
                   'last_name': 'Lindberg',
                   'position': 'vice chair'},
                  {'first_name': 'Ewa',
                   'last_name': 'Jodlowska',
                   'position': 'director of operations'}],
     'organisation': 'Python Software Foundation',
     'type': 'non-profit',
     'url': 'www.python.org/psf/'}
    


```python
# let's print the keys of the dictionary
for key in data: 
    print(key, end=' ')
```

    organisation officers type country founded members budget url 


```python
# now let's convert a python dictionary into a json string

d = {"John": 51, "Kate": 12, "Bill": 27}
json_string = json.dumps(d)
print(type(json_string)) # убедимся, что теперь наши данные превратились в строку
```

    <class 'str'>
    


```python
# let's print out the string
print(json_string)
```

    {"John": 51, "Kate": 12, "Bill": 27}
    


```python
# let's do the same with an array
arr = ['hello', 'world']
json_string = json.dumps(arr)
print(type(json_string)) 
print(json_string)
```

    <class 'str'>
    ["hello", "world"]
    


```python
# not all valid python objects can be converted into a valid json 
d = {("A", 21): "John"}
json_string = json.dumps(d)
print(json_string)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-8-70c5ac5384b5> in <module>()
          1 # not all valid python objects can be converted into a valid json
          2 d = {("A", 21): "John"}
    ----> 3 json_string = json.dumps(d)
          4 print(json_string)
    

    C:\ProgramData\Anaconda3\lib\json\__init__.py in dumps(obj, skipkeys, ensure_ascii, check_circular, allow_nan, cls, indent, separators, default, sort_keys, **kw)
        229         cls is None and indent is None and separators is None and
        230         default is None and not sort_keys and not kw):
    --> 231         return _default_encoder.encode(obj)
        232     if cls is None:
        233         cls = JSONEncoder
    

    C:\ProgramData\Anaconda3\lib\json\encoder.py in encode(self, o)
        197         # exceptions aren't as detailed.  The list call should be roughly
        198         # equivalent to the PySequence_Fast that ''.join() would do.
    --> 199         chunks = self.iterencode(o, _one_shot=True)
        200         if not isinstance(chunks, (list, tuple)):
        201             chunks = list(chunks)
    

    C:\ProgramData\Anaconda3\lib\json\encoder.py in iterencode(self, o, _one_shot)
        255                 self.key_separator, self.item_separator, self.sort_keys,
        256                 self.skipkeys, _one_shot)
    --> 257         return _iterencode(o, 0)
        258 
        259 def _make_iterencode(markers, _default, _encoder, _indent, _floatstr,
    

    TypeError: keys must be a string



```python
# writing the code into a file

d = {'абв': 1, 'где': 2, 'ёжз': 3}

with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(d, f)

# the result (the contents of the file):

{"\u0433\u0434\u0435": 2, "\u0430\u0431\u0432": 1, "\u0451\u0436\u0437": 3}
```




    {'где': 2, 'абв': 1, 'ёжз': 3}




```python
# let's include the ensure_ascii parameter:

with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(d, f, ensure_ascii = False)

# the result:

{"где": 2, "абв": 1, "ёжз": 3}
```




    {'где': 2, 'абв': 1, 'ёжз': 3}




```python
# let's include the indent parameter (the number stands for the number of spaces in the indentation):

with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(d, f, ensure_ascii = False, indent = 4)

# the result

{
    "абв": 1,
    "где": 2,
    "ёжз": 3
}
```




    {'абв': 1, 'где': 2, 'ёжз': 3}



## How to check the validity of a json?

When you work with a long json, it is hard to spot a mistake. To make sure your json is valid, use one of the following tools:
* https://jsonlint.com/
* https://jsoncompare.com/ 
* http://www.jsonschemavalidator.net/
* https://jsonformatter.curiousconcept.com/#

## JSON in the wild

### 1. Sending the data from the server to the browser



As an example, let's use the github. For instance, let's determine the number of repositories of a given github user.


```python
import json
import urllib.request

user = "agricolamz"  # the user 
url = 'https://api.github.com/users/%s/repos' % user  
# the link to the json

response = urllib.request.urlopen(url)  # sending a request to the server and getting an answer
text = response.read().decode('utf-8')  # reading the answer into a string
data = json.loads(text) # converting the json string into a python object

print(len(data))  # printing the number of the user's repositories
for i in data:
    print(i["name"]) # printing the names of all the repositories
```

    30
    2017-MAG_R_course
    2017_ANDAN_course
    2017_ConCorT_lingtypology
    2017_HSE_SPb_R_introduction
    2017_m_Instrumental_Phonetics
    2017_WCAD_talk
    2017_Zilo_fieldwork_prezi
    2018-MAG_R_course
    2018.04.21_MSU_acceptability
    2018.05.11_Yerevan_Zilo_classes
    2018.07.29_ANDAN_Agreement
    2018.07.30_LINGDAN_Praat_Elan
    2018.08.02_ANDAN_Shiny
    2018.11.22_SPb_HSE_Cartography
    2018.11.23_Bayesian_Typology
    2018.11.24-25_PublicData_hakathon_8
    2018.12.19_City_data_Lingtypology
    2018_18.03.20_lingtypology_news
    2018_18.04.28_Grammaticality_shiny
    2018_adyghe_phonology
    2018_ANDAN_course_winter
    2018_Andia_adjectives
    2018_Andi_relative_clause
    2018_Areal_Patterns
    2018_clearspending_hackathon_59
    2018_clearspending_hackathon_63
    2018_data_analysis_for_linguists
    2018_Digital_literacy
    2018_FE_R_statistics
    2018_lingdan_organisation
    

## Twitter

The documentation for the twitter json: https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object


```python
# reading the data from the twitter.json file
twitter = []
for line in open('twitter.json'):
    twitter.append(json.loads(line))
```


```python
# how many tweets do we have?
len(twitter)
```




    2556




```python
twitter[0]
```




    {'created_at': 'Wed Oct 03 05:00:00 +0000 2018',
     'id': 1047350533454012417,
     'id_str': '1047350533454012417',
     'text': 'RT @ELISSEsifieds: Nothing can stop us from supporting you. When we say all the way, it will be indeed. Hello Elissesifieds Cebu. \nThank yo…',
     'source': '<a href="http://twitter.com/download/iphone" rel="nofollow">Twitter for iPhone</a>',
     'truncated': False,
     'in_reply_to_status_id': None,
     'in_reply_to_status_id_str': None,
     'in_reply_to_user_id': None,
     'in_reply_to_user_id_str': None,
     'in_reply_to_screen_name': None,
     'user': {'id': 937522240488443905,
      'id_str': '937522240488443905',
      'name': "DonnaArabe'Efieds🇸🇦",
      'screen_name': 'ArabiDonna',
      'location': 'Jubail Industrial City, Kingdo',
      'url': None,
      'description': "Don't think too hard,just have fun with it.",
      'translator_type': 'none',
      'protected': False,
      'verified': False,
      'followers_count': 104,
      'friends_count': 132,
      'listed_count': 0,
      'favourites_count': 8372,
      'statuses_count': 6896,
      'created_at': 'Mon Dec 04 03:21:35 +0000 2017',
      'utc_offset': None,
      'time_zone': None,
      'geo_enabled': False,
      'lang': 'en',
      'contributors_enabled': False,
      'is_translator': False,
      'profile_background_color': 'F5F8FA',
      'profile_background_image_url': '',
      'profile_background_image_url_https': '',
      'profile_background_tile': False,
      'profile_link_color': '1DA1F2',
      'profile_sidebar_border_color': 'C0DEED',
      'profile_sidebar_fill_color': 'DDEEF6',
      'profile_text_color': '333333',
      'profile_use_background_image': True,
      'profile_image_url': 'http://pbs.twimg.com/profile_images/1047119228942393344/Px7FPs-3_normal.jpg',
      'profile_image_url_https': 'https://pbs.twimg.com/profile_images/1047119228942393344/Px7FPs-3_normal.jpg',
      'profile_banner_url': 'https://pbs.twimg.com/profile_banners/937522240488443905/1538487685',
      'default_profile': True,
      'default_profile_image': False,
      'following': None,
      'follow_request_sent': None,
      'notifications': None},
     'geo': None,
     'coordinates': None,
     'place': None,
     'contributors': None,
     'retweeted_status': {'created_at': 'Wed Oct 03 04:56:30 +0000 2018',
      'id': 1047349651915960322,
      'id_str': '1047349651915960322',
      'text': 'Nothing can stop us from supporting you. When we say all the way, it will be indeed. Hello Elissesifieds Cebu. \nTha… https://t.co/Crn6xJlhJQ',
      'display_text_range': [0, 140],
      'source': '<a href="http://twitter.com/download/android" rel="nofollow">Twitter for Android</a>',
      'truncated': True,
      'in_reply_to_status_id': None,
      'in_reply_to_status_id_str': None,
      'in_reply_to_user_id': None,
      'in_reply_to_user_id_str': None,
      'in_reply_to_screen_name': None,
      'user': {'id': 1424795701,
       'id_str': '1424795701',
       'name': 'ELISSEsifieds OFC',
       'screen_name': 'ELISSEsifieds',
       'location': 'Followed by Elisse : 08/19/13',
       'url': 'http://Facebook.com/ELISSEsifiedOfficial',
       'description': 'OFC Fansclub of @ElisseJoson ♡ Acknowledged and Recognized by Dreamscape .  (Be ELISSEsifieds just click the link and fill:http://bit.ly/1U8V5eR )',
       'translator_type': 'none',
       'protected': False,
       'verified': False,
       'followers_count': 14110,
       'friends_count': 277,
       'listed_count': 2,
       'favourites_count': 15075,
       'statuses_count': 42618,
       'created_at': 'Mon May 13 06:39:55 +0000 2013',
       'utc_offset': None,
       'time_zone': None,
       'geo_enabled': True,
       'lang': 'en',
       'contributors_enabled': False,
       'is_translator': False,
       'profile_background_color': 'C0DEED',
       'profile_background_image_url': 'http://abs.twimg.com/images/themes/theme1/bg.png',
       'profile_background_image_url_https': 'https://abs.twimg.com/images/themes/theme1/bg.png',
       'profile_background_tile': True,
       'profile_link_color': 'F099F0',
       'profile_sidebar_border_color': 'FFFFFF',
       'profile_sidebar_fill_color': 'DDEEF6',
       'profile_text_color': '333333',
       'profile_use_background_image': True,
       'profile_image_url': 'http://pbs.twimg.com/profile_images/975532647349854209/glaBAm_T_normal.jpg',
       'profile_image_url_https': 'https://pbs.twimg.com/profile_images/975532647349854209/glaBAm_T_normal.jpg',
       'profile_banner_url': 'https://pbs.twimg.com/profile_banners/1424795701/1490279660',
       'default_profile': False,
       'default_profile_image': False,
       'following': None,
       'follow_request_sent': None,
       'notifications': None},
      'geo': None,
      'coordinates': None,
      'place': None,
      'contributors': None,
      'is_quote_status': False,
      'extended_tweet': {'full_text': 'Nothing can stop us from supporting you. When we say all the way, it will be indeed. Hello Elissesifieds Cebu. \nThank you guys, for rising so early just to visit Elisse on her last taping day sa humble place nyo💓 https://t.co/cLpJ2ifyGw',
       'display_text_range': [0, 212],
       'entities': {'hashtags': [],
        'urls': [],
        'user_mentions': [],
        'symbols': [],
        'media': [{'id': 1047349579832688641,
          'id_str': '1047349579832688641',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju5M2VAAESKaQ.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju5M2VAAESKaQ.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'large': {'w': 720, 'h': 960, 'resize': 'fit'},
           'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'}}},
         {'id': 1047349589253083136,
          'id_str': '1047349589253083136',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju5v8U0AAhr9-.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju5v8U0AAhr9-.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'large': {'w': 720, 'h': 960, 'resize': 'fit'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'}}},
         {'id': 1047349602142150657,
          'id_str': '1047349602142150657',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju6f9UYAEfHDd.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju6f9UYAEfHDd.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'},
           'large': {'w': 720, 'h': 960, 'resize': 'fit'}}},
         {'id': 1047349625907109889,
          'id_str': '1047349625907109889',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju74fU4AEpxTQ.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju74fU4AEpxTQ.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'},
           'large': {'w': 720, 'h': 960, 'resize': 'fit'}}}]},
       'extended_entities': {'media': [{'id': 1047349579832688641,
          'id_str': '1047349579832688641',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju5M2VAAESKaQ.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju5M2VAAESKaQ.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'large': {'w': 720, 'h': 960, 'resize': 'fit'},
           'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'}}},
         {'id': 1047349589253083136,
          'id_str': '1047349589253083136',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju5v8U0AAhr9-.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju5v8U0AAhr9-.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'large': {'w': 720, 'h': 960, 'resize': 'fit'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'}}},
         {'id': 1047349602142150657,
          'id_str': '1047349602142150657',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju6f9UYAEfHDd.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju6f9UYAEfHDd.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'},
           'large': {'w': 720, 'h': 960, 'resize': 'fit'}}},
         {'id': 1047349625907109889,
          'id_str': '1047349625907109889',
          'indices': [213, 236],
          'media_url': 'http://pbs.twimg.com/media/Doju74fU4AEpxTQ.jpg',
          'media_url_https': 'https://pbs.twimg.com/media/Doju74fU4AEpxTQ.jpg',
          'url': 'https://t.co/cLpJ2ifyGw',
          'display_url': 'pic.twitter.com/cLpJ2ifyGw',
          'expanded_url': 'https://twitter.com/ELISSEsifieds/status/1047349651915960322/photo/1',
          'type': 'photo',
          'sizes': {'thumb': {'w': 150, 'h': 150, 'resize': 'crop'},
           'medium': {'w': 720, 'h': 960, 'resize': 'fit'},
           'small': {'w': 510, 'h': 680, 'resize': 'fit'},
           'large': {'w': 720, 'h': 960, 'resize': 'fit'}}}]}},
      'quote_count': 0,
      'reply_count': 0,
      'retweet_count': 3,
      'favorite_count': 9,
      'entities': {'hashtags': [],
       'urls': [{'url': 'https://t.co/Crn6xJlhJQ',
         'expanded_url': 'https://twitter.com/i/web/status/1047349651915960322',
         'display_url': 'twitter.com/i/web/status/1…',
         'indices': [117, 140]}],
       'user_mentions': [],
       'symbols': []},
      'favorited': False,
      'retweeted': False,
      'possibly_sensitive': False,
      'filter_level': 'low',
      'lang': 'en'},
     'is_quote_status': False,
     'quote_count': 0,
     'reply_count': 0,
     'retweet_count': 0,
     'favorite_count': 0,
     'entities': {'hashtags': [],
      'urls': [],
      'user_mentions': [{'screen_name': 'ELISSEsifieds',
        'name': 'ELISSEsifieds OFC',
        'id': 1424795701,
        'id_str': '1424795701',
        'indices': [3, 17]}],
      'symbols': []},
     'favorited': False,
     'retweeted': False,
     'filter_level': 'low',
     'lang': 'en',
     'timestamp_ms': '1538542800664'}




```python
twitter[-1]
```




    {'delete': {'status': {'id': 729296586933678081,
       'id_str': '729296586933678081',
       'user_id': 2740164192,
       'user_id_str': '2740164192'},
      'timestamp_ms': '1538542860162'}}




```python
ok = twitter[0]
```


```python
# the language of the tweet
ok['lang']
```




    'en'




```python
# the text of the tweet
ok['retweeted_status']['extended_tweet']['full_text']
```




    'Nothing can stop us from supporting you. When we say all the way, it will be indeed. Hello Elissesifieds Cebu. \nThank you guys, for rising so early just to visit Elisse on her last taping day sa humble place nyo💓 https://t.co/cLpJ2ifyGw'




```python
# the percentage of the deleted tweets
sum('delete' in t for t in twitter)/len(twitter)
```




    0.14162754303599373




```python
langs = [t['lang'] for t in twitter if 'lang' in t]
```


```python
# the most popular languages of the tweets
from collections import Counter
Counter(langs).most_common(10)
```




    [('en', 719),
     ('ja', 438),
     ('es', 173),
     ('ko', 149),
     ('th', 123),
     ('ar', 119),
     ('und', 117),
     ('in', 71),
     ('pt', 69),
     ('fr', 35)]




```python
# Do we have tweets from the same user?
cnt = Counter([t['user']['id'] for t in twitter if 'user' in t])
for key in sorted(cnt, key=cnt.get, reverse=True):
    if cnt[key] > 1:
        print(cnt[key], key)
```

    2 992084216350294016
    2 581282101
    2 2317193324
    2 978499715657445377
    2 2245928100
    2 993031040
    2 290401936
    2 3067130479
    2 849417895109156869
    2 958056194366754816
    2 1017442172495331328
    2 702487896935104513
    2 995683537197158401
    2 121016179
    2 947288315375394817
    2 2464271844
    2 1009443285176340482
    2 860202971266772992
    2 2734975298
    2 4311188534
    2 1290792062
    2 897067178754686976
    2 4179415159
    2 772081812109570048
    2 1006114081739288577
    


```python
# Top 20 hashtags
hashtags = []
for t in twitter:
    if 'entities' in t:
        if 'hashtags' in t['entities'] and t['entities']['hashtags']:
            hashtags.extend([i['text'] for i in t['entities']['hashtags']])
Counter(hashtags).most_common(20)
```




    [('BTS', 17),
     ('방탄소년단', 13),
     ('AMAs', 11),
     ('人気投票ガチャ', 8),
     ('태형', 7),
     ('뷔', 6),
     ('BTSinChicago', 5),
     ('BTSLoveYourselfTour', 5),
     ('오늘의방탄', 5),
     ('PledgeForSwachhBharat', 5),
     ('MPN', 5),
     ('PCAs', 4),
     ('V', 4),
     ('시카고1회차공연', 4),
     ('เป๊กผลิตโชค', 4),
     ('JIMIN', 4),
     ('running', 3),
     ('NCT', 3),
     ('지민', 3),
     ('WajahmuPlastik', 3)]




```python
# preprocessing the text of the original tweets (not retweeted tweets)
from string import punctuation
texts = []
for t in twitter:
    if 'retweeted_status' in t:
        pass
    elif 'text' in t and t['lang'] == 'en':
        texts.append(' '.join([w.strip(punctuation) for w in t['text'].lower().split()]))

all_tweets = ' '.join(texts)
d = Counter(all_tweets.split())
d.most_common(25)
```




    [('the', 125),
     ('to', 86),
     ('a', 75),
     ('i', 73),
     ('and', 64),
     ('is', 50),
     ('you', 48),
     ('of', 45),
     ('for', 42),
     ('it', 41),
     ('in', 38),
     ('that', 33),
     ('this', 31),
     ('my', 30),
     ('me', 27),
     ('be', 26),
     ('on', 26),
     ('are', 21),
     ('what', 20),
     ('so', 20),
     ('with', 20),
     ('have', 19),
     ('not', 17),
     ('more', 17),
     ('but', 17)]




```python
# print out the top 10 users with the most followers 
d = {}
for t in twitter:
    if 'user' in t:
        d[t['user']['name']] = t['user']['followers_count']
for key in sorted(d, key=d.get, reverse=True)[:10]:
    print(d[key], key)
```

    2521403 Filosofía♕
    1491309 FITNESS Magazine
    1206759 malaysiakini.com
    1137374 NYT Science
    625463 Gramática
    392472 TGRT Haber
    383698 The Sun Football ⚽
    374222 Melbourne, Australia
    318189 Roznama Express
    311319 💞 ცųཞɠɛཞცơơɠıɛ 💞
    


```python
# top-10 sources of the tweets
import re

reg = re.compile('^.*?>(.*?)<.*?$')

sources = []
for t in twitter:
    if 'source' in t:
        res = reg.findall(t['source'])
        if res:
            sources.extend(res)
Counter(sources).most_common(20)
```




    [('Twitter for iPhone', 800),
     ('Twitter for Android', 695),
     ('Twitter Web Client', 140),
     ('twittbot.net', 122),
     ('Twitter Lite', 51),
     ('Twitter for iPad', 28),
     ('TweetDeck', 23),
     ('Facebook', 17),
     ('IFTTT', 14),
     ('تطبيق قرآني', 10),
     ('dlvr.it', 10),
     ('Buffer', 8),
     ('Google', 8),
     ('autotweety.net', 7),
     ('Hootsuite Inc.', 7),
     ('WordPress.com', 6),
     ('Twittascope', 6),
     ('Botbird tweets', 6),
     ('تطبيق دعـاء', 5),
     ('Zapier.com', 5)]


