
# Morphological analysis in Python

```
pip install pymystem3

pip install pymorphy2
```


```python
from pymystem3 import Mystem
import pymorphy2
```

## pymystem3

`Pymystem3` is the wrapper for the Mystem: https://pythonhosted.org/pymystem3/

First, we need to create an exemplar of the class:


```python
m = Mystem()
```

This class has two methods:

+ `lemmatize` -- returns a list of lemmas
+ `analyze` -- returns the full analysis as a dictionary.

Let's try out the two methods on a simple text:


```python
text = 'Живые выступления Делии де Франс - это камерные шоу,' +\
'в которых классические инструменты - фортепиано и арфа - соединяются с электроникой. ' +\
'Темой музыкального исследования Делии становятся пропорции - ' +\
'как много эксперимента нужно поп-музыке и как много попа нужно экспериментальной музыке.'
```


```python
text
```




    'Живые выступления Делии де Франс - это камерные шоу,в которых классические инструменты - фортепиано и арфа - соединяются с электроникой. Темой музыкального исследования Делии становятся пропорции - как много эксперимента нужно поп-музыке и как много попа нужно экспериментальной музыке.'




```python
lemmas = m.lemmatize(text)
lemmas[10:20]
```




    ['это', ' ', 'камерный', ' ', 'шоу', ',', 'в', ' ', 'который', ' ']



We can easily put the text back together:


```python
print(''.join(lemmas))
```

    живой выступление делий де франс - это камерный шоу,в который классический инструмент - фортепиано и арфа - соединяться с электроника. тема музыкальный исследование делий становиться пропорция - как много эксперимент нужно поп-музыка и как много поп нужный экспериментальный музыка.
    
    


```python
ana = m.analyze(text)
print(ana[:10])
```

    [{'analysis': [{'lex': 'живой', 'wt': 0.9239751582, 'gr': 'A=(вин,мн,полн,неод|им,мн,полн)'}], 'text': 'Живые'}, {'text': ' '}, {'analysis': [{'lex': 'выступление', 'wt': 1, 'gr': 'S,сред,неод=(вин,мн|род,ед|им,мн)'}], 'text': 'выступления'}, {'text': ' '}, {'analysis': [{'lex': 'делий', 'wt': 1, 'gr': 'S,имя,муж,од=(пр,ед|им,мн)'}], 'text': 'Делии'}, {'text': ' '}, {'analysis': [{'lex': 'де', 'wt': 1, 'gr': 'PART='}], 'text': 'де'}, {'text': ' '}, {'analysis': [{'lex': 'франс', 'wt': 1, 'gr': 'S,имя,муж,од=им,ед'}], 'text': 'Франс'}, {'text': ' - '}]
    

The analysis of each word is an element of an array:


```python
for word in ana[:10]:
    print(word)
```

    {'analysis': [{'lex': 'живой', 'wt': 0.9239751582, 'gr': 'A=(вин,мн,полн,неод|им,мн,полн)'}], 'text': 'Живые'}
    {'text': ' '}
    {'analysis': [{'lex': 'выступление', 'wt': 1, 'gr': 'S,сред,неод=(вин,мн|род,ед|им,мн)'}], 'text': 'выступления'}
    {'text': ' '}
    {'analysis': [{'lex': 'делий', 'wt': 1, 'gr': 'S,имя,муж,од=(пр,ед|им,мн)'}], 'text': 'Делии'}
    {'text': ' '}
    {'analysis': [{'lex': 'де', 'wt': 1, 'gr': 'PART='}], 'text': 'де'}
    {'text': ' '}
    {'analysis': [{'lex': 'франс', 'wt': 1, 'gr': 'S,имя,муж,од=им,ед'}], 'text': 'Франс'}
    {'text': ' - '}
    

The field `text` contains the original words, the field `analysis` (that could be absent) contains the grammatical characteristics of the lemma.

What is the meaning of `=` and `|` in the grammatical analysis above?

Let's extract the part of speech information:


```python
for word in ana:
    if 'analysis' in word:
        gr = word['analysis'][0]['gr']
        pos = gr.split('=')[0].split(',')[0]
        print(word['text'], pos)
```

    Живые A
    выступления S
    Делии S
    де PART
    Франс S
    это PART
    камерные A
    шоу S
    в PR
    которых APRO
    классические A
    инструменты S
    фортепиано S
    и CONJ
    арфа S
    соединяются V
    с PR
    электроникой S
    Темой S
    музыкального A
    исследования S
    Делии S
    становятся V
    пропорции S
    как CONJ
    много ADV
    эксперимента S
    нужно ADV
    поп-музыке S
    и CONJ
    как ADVPRO
    много ADV
    попа S
    нужно A
    экспериментальной A
    музыке S
    

#### To sum up:

##### Advantages:

+ great quality of the analysis
+ it resolves part of speech homonymy 
+ it takes into account the context

##### Disadvantages:

+ it is slow (but if you strip the text of the punctuation, it will work much faster)


## pymorphy2

`pymorphy2` is not a wrapper, it is a morphological analyzer, created for and on Python. It can do what `pymystem3` can do, and more: for example, it can change the word form. `pymorphy2` can also deal with unknown words.

https://pymorphy2.readthedocs.io/en/latest/

`pymorphy2` was trained on the OpenCorpora word lists, which is reflected in its tag choice. 

Again, we start by creating an exemplar of the class MorphAnalyzer. It is recommended to create one exemplar and work with it, since it takes up a lot of the memory.


```python
from pymorphy2 import MorphAnalyzer
morph = MorphAnalyzer()
```

To analyze a word we use the method `parse`:


```python
ana = morph.parse('стали')
ana
```




    [Parse(word='стали', tag=OpencorporaTag('VERB,perf,intr plur,past,indc'), normal_form='стать', score=0.984662, methods_stack=((<DictionaryAnalyzer>, 'стали', 904, 4),)),
     Parse(word='стали', tag=OpencorporaTag('NOUN,inan,femn sing,gent'), normal_form='сталь', score=0.003067, methods_stack=((<DictionaryAnalyzer>, 'стали', 13, 1),)),
     Parse(word='стали', tag=OpencorporaTag('NOUN,inan,femn sing,datv'), normal_form='сталь', score=0.003067, methods_stack=((<DictionaryAnalyzer>, 'стали', 13, 2),)),
     Parse(word='стали', tag=OpencorporaTag('NOUN,inan,femn sing,loct'), normal_form='сталь', score=0.003067, methods_stack=((<DictionaryAnalyzer>, 'стали', 13, 5),)),
     Parse(word='стали', tag=OpencorporaTag('NOUN,inan,femn plur,nomn'), normal_form='сталь', score=0.003067, methods_stack=((<DictionaryAnalyzer>, 'стали', 13, 6),)),
     Parse(word='стали', tag=OpencorporaTag('NOUN,inan,femn plur,accs'), normal_form='сталь', score=0.003067, methods_stack=((<DictionaryAnalyzer>, 'стали', 13, 9),))]



The analyzer returned all the possible analyses starting with the most likely analysis.

Each parse has the following attributes: the orginal word, the tag, the lemma, the probability of the parse:


```python
first = ana[0]  # the first parse
print('Слово:', first.word)
print('Тэг:', first.tag)
print('Лемма:', first.normal_form)
print('Вероятность:', first.score)
```

    Слово: стали
    Тэг: VERB,perf,intr plur,past,indc
    Лемма: стать
    Вероятность: 0.984662
    

For each parse, we can get a corresponding lemma and all the information about it:


```python
first.normalized
```




    Parse(word='стать', tag=OpencorporaTag('INFN,perf,intr'), normal_form='стать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'стать', 904, 0),))




```python
last = ana[-1] # the last parse
print('Разбор слова: ', last)
print()
print('Разбор леммы: ', last.normalized)
```

    Разбор слова:  Parse(word='стали', tag=OpencorporaTag('NOUN,inan,femn plur,accs'), normal_form='сталь', score=0.003067, methods_stack=((<DictionaryAnalyzer>, 'стали', 13, 9),))
    
    Разбор леммы:  Parse(word='сталь', tag=OpencorporaTag('NOUN,inan,femn sing,nomn'), normal_form='сталь', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'сталь', 13, 0),))
    

If one prints the contents of the tag, one could think think that this is a string:


```python
first = ana[0]  # первый разбор
print(first.tag)
```

    VERB,perf,intr plur,past,indc
    

In reality, it is an object of the class `OpencorporaTag`, therefore, you can't manipulate it in all the ways you could manipulate a string.

You could, however, check whether a certain grammeme is in the tag:



```python
'NOUN' in first.tag
```




    False




```python
'VERB' in first.tag
```




    True




```python
{'VERB', 'sing'} in first.tag
```




    False




```python
{'VERB', 'plur'} in first.tag
```




    True



Out of each tag one could get more detailed information. If the grammeme is contained within the parse, we will get it, if not, we will get `None`.


```python
p.tag.POS           # Part of Speech, часть речи
p.tag.animacy       # одушевленность
p.tag.aspect        # вид: совершенный или несовершенный
p.tag.case          # падеж
p.tag.gender        # род (мужской, женский, средний)
p.tag.involvement   # включенность говорящего в действие
p.tag.mood          # наклонение (повелительное, изъявительное)
p.tag.number        # число (единственное, множественное)
p.tag.person        # лицо (1, 2, 3)
p.tag.tense         # время (настоящее, прошедшее, будущее)
p.tag.transitivity  # переходность (переходный, непереходный)
p.tag.voice         # залог (действительный, страдательный)
```


```python
print(first.tag)
print('Время: ', first.tag.tense)
print('Падеж: ', first.tag.case)
```

    VERB,perf,intr plur,past,indc
    Время:  past
    Падеж:  None
    

Here you can fird the full list of grammemes used in the module - https://pymorphy2.readthedocs.io/en/latest/user/grammemes.html. If you are searching for a grammeme that is not on the list, you will get an error.

One could also get a string in cyrillic:



```python
first.tag.cyr_repr
```




    'ГЛ,сов,неперех мн,прош,изъяв'



## Inflection

If we have the analysis for the word, we can put the word in a different form by using the `inflect` function. This fuction gets a set of grammemes as an input and tries to apply them to the analysis.


```python
morph.parse('программирую')
```




    [Parse(word='программирую', tag=OpencorporaTag('VERB,impf,tran sing,1per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирую', 168, 1),))]




```python
prog = morph.parse('программирую')[0]
prog.inflect({'plur'})
```




    Parse(word='программируем', tag=OpencorporaTag('VERB,impf,tran plur,1per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируем', 168, 2),))




```python
prog.inflect({'plur', 'past'})
```




    Parse(word='программировали', tag=OpencorporaTag('VERB,impf,tran plur,past,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировали', 168, 10),))




```python
prog.inflect({'past'})
```




    Parse(word='программировал', tag=OpencorporaTag('VERB,impf,tran masc,sing,past,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировал', 168, 7),))




```python
prog.inflect({'past', 'femn'})
```




    Parse(word='программировала', tag=OpencorporaTag('VERB,impf,tran femn,sing,past,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировала', 168, 8),))



## Формы слова

Using the `lexeme` attribute, we could get an array of all the word forms:



```python
prog.lexeme
```




    [Parse(word='программировать', tag=OpencorporaTag('INFN,impf,tran'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировать', 168, 0),)),
     Parse(word='программирую', tag=OpencorporaTag('VERB,impf,tran sing,1per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирую', 168, 1),)),
     Parse(word='программируем', tag=OpencorporaTag('VERB,impf,tran plur,1per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируем', 168, 2),)),
     Parse(word='программируешь', tag=OpencorporaTag('VERB,impf,tran sing,2per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируешь', 168, 3),)),
     Parse(word='программируете', tag=OpencorporaTag('VERB,impf,tran plur,2per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируете', 168, 4),)),
     Parse(word='программирует', tag=OpencorporaTag('VERB,impf,tran sing,3per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирует', 168, 5),)),
     Parse(word='программируют', tag=OpencorporaTag('VERB,impf,tran plur,3per,pres,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируют', 168, 6),)),
     Parse(word='программировал', tag=OpencorporaTag('VERB,impf,tran masc,sing,past,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировал', 168, 7),)),
     Parse(word='программировала', tag=OpencorporaTag('VERB,impf,tran femn,sing,past,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировала', 168, 8),)),
     Parse(word='программировало', tag=OpencorporaTag('VERB,impf,tran neut,sing,past,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировало', 168, 9),)),
     Parse(word='программировали', tag=OpencorporaTag('VERB,impf,tran plur,past,indc'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировали', 168, 10),)),
     Parse(word='программируй', tag=OpencorporaTag('VERB,impf,tran sing,impr,excl'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируй', 168, 11),)),
     Parse(word='программируйте', tag=OpencorporaTag('VERB,impf,tran plur,impr,excl'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируйте', 168, 12),)),
     Parse(word='программирующий', tag=OpencorporaTag('PRTF,impf,tran,pres,actv masc,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующий', 168, 13),)),
     Parse(word='программирующего', tag=OpencorporaTag('PRTF,impf,tran,pres,actv masc,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующего', 168, 14),)),
     Parse(word='программирующему', tag=OpencorporaTag('PRTF,impf,tran,pres,actv masc,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующему', 168, 15),)),
     Parse(word='программирующего', tag=OpencorporaTag('PRTF,impf,tran,pres,actv anim,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующего', 168, 16),)),
     Parse(word='программирующий', tag=OpencorporaTag('PRTF,impf,tran,pres,actv inan,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующий', 168, 17),)),
     Parse(word='программирующим', tag=OpencorporaTag('PRTF,impf,tran,pres,actv masc,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующим', 168, 18),)),
     Parse(word='программирующем', tag=OpencorporaTag('PRTF,impf,tran,pres,actv masc,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующем', 168, 19),)),
     Parse(word='программирующая', tag=OpencorporaTag('PRTF,impf,tran,pres,actv femn,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующая', 168, 20),)),
     Parse(word='программирующей', tag=OpencorporaTag('PRTF,impf,tran,pres,actv femn,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующей', 168, 21),)),
     Parse(word='программирующей', tag=OpencorporaTag('PRTF,impf,tran,pres,actv femn,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующей', 168, 22),)),
     Parse(word='программирующую', tag=OpencorporaTag('PRTF,impf,tran,pres,actv femn,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующую', 168, 23),)),
     Parse(word='программирующей', tag=OpencorporaTag('PRTF,impf,tran,pres,actv femn,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующей', 168, 24),)),
     Parse(word='программирующею', tag=OpencorporaTag('PRTF,impf,tran,pres,actv femn,sing,ablt,V-ey'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующею', 168, 25),)),
     Parse(word='программирующей', tag=OpencorporaTag('PRTF,impf,tran,pres,actv femn,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующей', 168, 26),)),
     Parse(word='программирующее', tag=OpencorporaTag('PRTF,impf,tran,pres,actv neut,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующее', 168, 27),)),
     Parse(word='программирующего', tag=OpencorporaTag('PRTF,impf,tran,pres,actv neut,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующего', 168, 28),)),
     Parse(word='программирующему', tag=OpencorporaTag('PRTF,impf,tran,pres,actv neut,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующему', 168, 29),)),
     Parse(word='программирующее', tag=OpencorporaTag('PRTF,impf,tran,pres,actv neut,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующее', 168, 30),)),
     Parse(word='программирующим', tag=OpencorporaTag('PRTF,impf,tran,pres,actv neut,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующим', 168, 31),)),
     Parse(word='программирующем', tag=OpencorporaTag('PRTF,impf,tran,pres,actv neut,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующем', 168, 32),)),
     Parse(word='программирующие', tag=OpencorporaTag('PRTF,impf,tran,pres,actv plur,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующие', 168, 33),)),
     Parse(word='программирующих', tag=OpencorporaTag('PRTF,impf,tran,pres,actv plur,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующих', 168, 34),)),
     Parse(word='программирующим', tag=OpencorporaTag('PRTF,impf,tran,pres,actv plur,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующим', 168, 35),)),
     Parse(word='программирующих', tag=OpencorporaTag('PRTF,impf,tran,pres,actv anim,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующих', 168, 36),)),
     Parse(word='программирующие', tag=OpencorporaTag('PRTF,impf,tran,pres,actv inan,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующие', 168, 37),)),
     Parse(word='программирующими', tag=OpencorporaTag('PRTF,impf,tran,pres,actv plur,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующими', 168, 38),)),
     Parse(word='программирующих', tag=OpencorporaTag('PRTF,impf,tran,pres,actv plur,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирующих', 168, 39),)),
     Parse(word='программировавший', tag=OpencorporaTag('PRTF,impf,tran,past,actv masc,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавший', 168, 40),)),
     Parse(word='программировавшего', tag=OpencorporaTag('PRTF,impf,tran,past,actv masc,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшего', 168, 41),)),
     Parse(word='программировавшему', tag=OpencorporaTag('PRTF,impf,tran,past,actv masc,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшему', 168, 42),)),
     Parse(word='программировавшего', tag=OpencorporaTag('PRTF,impf,tran,past,actv anim,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшего', 168, 43),)),
     Parse(word='программировавший', tag=OpencorporaTag('PRTF,impf,tran,past,actv inan,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавший', 168, 44),)),
     Parse(word='программировавшим', tag=OpencorporaTag('PRTF,impf,tran,past,actv masc,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшим', 168, 45),)),
     Parse(word='программировавшем', tag=OpencorporaTag('PRTF,impf,tran,past,actv masc,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшем', 168, 46),)),
     Parse(word='программировавшая', tag=OpencorporaTag('PRTF,impf,tran,past,actv femn,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшая', 168, 47),)),
     Parse(word='программировавшей', tag=OpencorporaTag('PRTF,impf,tran,past,actv femn,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшей', 168, 48),)),
     Parse(word='программировавшей', tag=OpencorporaTag('PRTF,impf,tran,past,actv femn,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшей', 168, 49),)),
     Parse(word='программировавшую', tag=OpencorporaTag('PRTF,impf,tran,past,actv femn,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшую', 168, 50),)),
     Parse(word='программировавшей', tag=OpencorporaTag('PRTF,impf,tran,past,actv femn,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшей', 168, 51),)),
     Parse(word='программировавшею', tag=OpencorporaTag('PRTF,impf,tran,past,actv femn,sing,ablt,V-ey'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшею', 168, 52),)),
     Parse(word='программировавшей', tag=OpencorporaTag('PRTF,impf,tran,past,actv femn,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшей', 168, 53),)),
     Parse(word='программировавшее', tag=OpencorporaTag('PRTF,impf,tran,past,actv neut,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшее', 168, 54),)),
     Parse(word='программировавшего', tag=OpencorporaTag('PRTF,impf,tran,past,actv neut,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшего', 168, 55),)),
     Parse(word='программировавшему', tag=OpencorporaTag('PRTF,impf,tran,past,actv neut,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшему', 168, 56),)),
     Parse(word='программировавшее', tag=OpencorporaTag('PRTF,impf,tran,past,actv neut,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшее', 168, 57),)),
     Parse(word='программировавшим', tag=OpencorporaTag('PRTF,impf,tran,past,actv neut,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшим', 168, 58),)),
     Parse(word='программировавшем', tag=OpencorporaTag('PRTF,impf,tran,past,actv neut,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшем', 168, 59),)),
     Parse(word='программировавшие', tag=OpencorporaTag('PRTF,impf,tran,past,actv plur,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшие', 168, 60),)),
     Parse(word='программировавших', tag=OpencorporaTag('PRTF,impf,tran,past,actv plur,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавших', 168, 61),)),
     Parse(word='программировавшим', tag=OpencorporaTag('PRTF,impf,tran,past,actv plur,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшим', 168, 62),)),
     Parse(word='программировавших', tag=OpencorporaTag('PRTF,impf,tran,past,actv anim,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавших', 168, 63),)),
     Parse(word='программировавшие', tag=OpencorporaTag('PRTF,impf,tran,past,actv inan,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшие', 168, 64),)),
     Parse(word='программировавшими', tag=OpencorporaTag('PRTF,impf,tran,past,actv plur,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавшими', 168, 65),)),
     Parse(word='программировавших', tag=OpencorporaTag('PRTF,impf,tran,past,actv plur,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавших', 168, 66),)),
     Parse(word='программируемый', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv masc,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемый', 168, 67),)),
     Parse(word='программируемого', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv masc,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемого', 168, 68),)),
     Parse(word='программируемому', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv masc,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемому', 168, 69),)),
     Parse(word='программируемого', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv anim,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемого', 168, 70),)),
     Parse(word='программируемый', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv inan,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемый', 168, 71),)),
     Parse(word='программируемым', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv masc,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемым', 168, 72),)),
     Parse(word='программируемом', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv masc,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемом', 168, 73),)),
     Parse(word='программируемая', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv femn,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемая', 168, 74),)),
     Parse(word='программируемой', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv femn,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемой', 168, 75),)),
     Parse(word='программируемой', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv femn,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемой', 168, 76),)),
     Parse(word='программируемую', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv femn,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемую', 168, 77),)),
     Parse(word='программируемой', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv femn,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемой', 168, 78),)),
     Parse(word='программируемою', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv femn,sing,ablt,V-oy'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемою', 168, 79),)),
     Parse(word='программируемой', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv femn,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемой', 168, 80),)),
     Parse(word='программируемое', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv neut,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемое', 168, 81),)),
     Parse(word='программируемого', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv neut,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемого', 168, 82),)),
     Parse(word='программируемому', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv neut,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемому', 168, 83),)),
     Parse(word='программируемое', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv neut,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемое', 168, 84),)),
     Parse(word='программируемым', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv neut,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемым', 168, 85),)),
     Parse(word='программируемом', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv neut,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемом', 168, 86),)),
     Parse(word='программируемые', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv plur,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемые', 168, 87),)),
     Parse(word='программируемых', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv plur,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемых', 168, 88),)),
     Parse(word='программируемым', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv plur,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемым', 168, 89),)),
     Parse(word='программируемых', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv anim,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемых', 168, 90),)),
     Parse(word='программируемые', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv inan,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемые', 168, 91),)),
     Parse(word='программируемыми', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv plur,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемыми', 168, 92),)),
     Parse(word='программируемых', tag=OpencorporaTag('PRTF,impf,tran,pres,pssv plur,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемых', 168, 93),)),
     Parse(word='программированный', tag=OpencorporaTag('PRTF,impf,tran,past,pssv masc,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированный', 168, 94),)),
     Parse(word='программированного', tag=OpencorporaTag('PRTF,impf,tran,past,pssv masc,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированного', 168, 95),)),
     Parse(word='программированному', tag=OpencorporaTag('PRTF,impf,tran,past,pssv masc,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированному', 168, 96),)),
     Parse(word='программированного', tag=OpencorporaTag('PRTF,impf,tran,past,pssv anim,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированного', 168, 97),)),
     Parse(word='программированный', tag=OpencorporaTag('PRTF,impf,tran,past,pssv inan,masc,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированный', 168, 98),)),
     Parse(word='программированным', tag=OpencorporaTag('PRTF,impf,tran,past,pssv masc,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированным', 168, 99),)),
     Parse(word='программированном', tag=OpencorporaTag('PRTF,impf,tran,past,pssv masc,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированном', 168, 100),)),
     Parse(word='программированная', tag=OpencorporaTag('PRTF,impf,tran,past,pssv femn,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированная', 168, 101),)),
     Parse(word='программированной', tag=OpencorporaTag('PRTF,impf,tran,past,pssv femn,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированной', 168, 102),)),
     Parse(word='программированной', tag=OpencorporaTag('PRTF,impf,tran,past,pssv femn,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированной', 168, 103),)),
     Parse(word='программированную', tag=OpencorporaTag('PRTF,impf,tran,past,pssv femn,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированную', 168, 104),)),
     Parse(word='программированной', tag=OpencorporaTag('PRTF,impf,tran,past,pssv femn,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированной', 168, 105),)),
     Parse(word='программированною', tag=OpencorporaTag('PRTF,impf,tran,past,pssv femn,sing,ablt,V-oy'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированною', 168, 106),)),
     Parse(word='программированной', tag=OpencorporaTag('PRTF,impf,tran,past,pssv femn,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированной', 168, 107),)),
     Parse(word='программированное', tag=OpencorporaTag('PRTF,impf,tran,past,pssv neut,sing,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированное', 168, 108),)),
     Parse(word='программированного', tag=OpencorporaTag('PRTF,impf,tran,past,pssv neut,sing,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированного', 168, 109),)),
     Parse(word='программированному', tag=OpencorporaTag('PRTF,impf,tran,past,pssv neut,sing,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированному', 168, 110),)),
     Parse(word='программированное', tag=OpencorporaTag('PRTF,impf,tran,past,pssv neut,sing,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированное', 168, 111),)),
     Parse(word='программированным', tag=OpencorporaTag('PRTF,impf,tran,past,pssv neut,sing,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированным', 168, 112),)),
     Parse(word='программированном', tag=OpencorporaTag('PRTF,impf,tran,past,pssv neut,sing,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированном', 168, 113),)),
     Parse(word='программированные', tag=OpencorporaTag('PRTF,impf,tran,past,pssv plur,nomn'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированные', 168, 114),)),
     Parse(word='программированных', tag=OpencorporaTag('PRTF,impf,tran,past,pssv plur,gent'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированных', 168, 115),)),
     Parse(word='программированным', tag=OpencorporaTag('PRTF,impf,tran,past,pssv plur,datv'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированным', 168, 116),)),
     Parse(word='программированных', tag=OpencorporaTag('PRTF,impf,tran,past,pssv anim,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированных', 168, 117),)),
     Parse(word='программированные', tag=OpencorporaTag('PRTF,impf,tran,past,pssv inan,plur,accs'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированные', 168, 118),)),
     Parse(word='программированными', tag=OpencorporaTag('PRTF,impf,tran,past,pssv plur,ablt'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированными', 168, 119),)),
     Parse(word='программированных', tag=OpencorporaTag('PRTF,impf,tran,past,pssv plur,loct'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированных', 168, 120),)),
     Parse(word='программируя', tag=OpencorporaTag('GRND,impf,tran pres'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируя', 168, 121),)),
     Parse(word='программировавши', tag=OpencorporaTag('GRND,impf,tran past,V-sh'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировавши', 168, 122),)),
     Parse(word='программируем', tag=OpencorporaTag('PRTS,impf,pres,pssv masc,sing'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируем', 168, 123),)),
     Parse(word='программируема', tag=OpencorporaTag('PRTS,impf,pres,pssv femn,sing'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируема', 168, 124),)),
     Parse(word='программируемо', tag=OpencorporaTag('PRTS,impf,pres,pssv neut,sing'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемо', 168, 125),)),
     Parse(word='программируемы', tag=OpencorporaTag('PRTS,impf,pres,pssv plur'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программируемы', 168, 126),)),
     Parse(word='программирован', tag=OpencorporaTag('PRTS,impf,past,pssv masc,sing'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирован', 168, 127),)),
     Parse(word='программирована', tag=OpencorporaTag('PRTS,impf,past,pssv femn,sing'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программирована', 168, 128),)),
     Parse(word='программировано', tag=OpencorporaTag('PRTS,impf,past,pssv neut,sing'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программировано', 168, 129),)),
     Parse(word='программированы', tag=OpencorporaTag('PRTS,impf,past,pssv plur'), normal_form='программировать', score=1.0, methods_stack=((<DictionaryAnalyzer>, 'программированы', 168, 130),))]



## Agreement with a numeral

From documentation:

>Слово нужно ставить в разные формы в зависимости от числительного, к которому оно относится. Например: “1 бутявка”, “2 бутявки”, “5 бутявок” Для этих целей используйте метод Parse.make_agree_with_number():




```python
butyavka = morph.parse('бутявка')[0]
```


```python
butyavka.make_agree_with_number(1).word
```




    'бутявка'




```python
butyavka.make_agree_with_number(2).word
```




    'бутявки'




```python
butyavka.make_agree_with_number(100).word
```




    'бутявок'



#### Summing up:

##### Advantages:

+ it parses, it deals with the inclination cases
+ it generates hypotheses for unknown words
+ it is written on Python and is faster than Mystem
+ it can work with Ukranian -- note as an idea for your in-class presentation or your final project

##### Disadvantages:

+ some problems with the quality of the parses
+ it does not take into account the context

#### Exercise:

1. Find a poem
2. Print a list of all the words from the poem with their part of speech tags
3. Print a lemmatized version of the poem
4. Print the poem with all the verbs in the imperative 


## Tokenizing and tagging some text using NLTK


```python
import nltk
nltk.download('punkt')
nltk.download('averaged_perceptron_tagger')
```

    [nltk_data] Downloading package punkt to
    [nltk_data]     C:\Users\Admin\AppData\Roaming\nltk_data...
    [nltk_data]   Package punkt is already up-to-date!
    [nltk_data] Downloading package averaged_perceptron_tagger to
    [nltk_data]     C:\Users\Admin\AppData\Roaming\nltk_data...
    [nltk_data]   Unzipping taggers\averaged_perceptron_tagger.zip.
    




    True




```python
sentence = """At eight o'clock on Thursday morning Arthur didn't feel very good."""
```


```python
tokens = nltk.word_tokenize(sentence)
```


```python
tokens
```




    ['At',
     'eight',
     "o'clock",
     'on',
     'Thursday',
     'morning',
     'Arthur',
     'did',
     "n't",
     'feel',
     'very',
     'good',
     '.']




```python
tagged = nltk.pos_tag(tokens)
tagged[0:13]
```




    [('At', 'IN'),
     ('eight', 'CD'),
     ("o'clock", 'NN'),
     ('on', 'IN'),
     ('Thursday', 'NNP'),
     ('morning', 'NN'),
     ('Arthur', 'NNP'),
     ('did', 'VBD'),
     ("n't", 'RB'),
     ('feel', 'VB'),
     ('very', 'RB'),
     ('good', 'JJ'),
     ('.', '.')]


