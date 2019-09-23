

```python
import re
import gensim
import logging
import nltk.data
import pandas as pd
import urllib.request
from bs4 import BeautifulSoup
from nltk.corpus import stopwords
from gensim.models import word2vec
```


```python
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)
```


```python
from tqdm import tqdm
from pymystem3 import Mystem
from string import punctuation

m = Mystem()
sw = stopwords.words("russian")

def preprocess_text(text):
    tokens = m.lemmatize(text.lower())
    tokens = [token for token in tokens if token not in sw \
              and token != " " \
              and token.strip() not in punctuation] 
    text = " ".join(tokens)
    
    return text

with open('C:\!!_info_!!\Desktop\liza.txt', 'r', encoding='utf8') as f:
    text1 = f.readlines()

new_lines = []

for line in tqdm(text1):
    newline = preprocess_text(line)
    newline1 = re.sub(r'[^\w\s]','',newline)
    new_lines.append(newline1)
        
with open('C:\!!_info_!!\Desktop\liza_lem.txt', 'w', encoding='utf8') as f1:
    for line in new_lines:
        f1.write(line)
```

    100%|████████████████████████████████████████████| 1/1 [00:03<00:00,  3.11s/it]
    


```python
f = 'C:\!!_info_!!\Desktop\liza_lem.txt'
data = gensim.models.word2vec.LineSentence(f)
```


```python
%time model_liza = gensim.models.Word2Vec(data, size=300, window=5, min_count=2)
```

    C:\ProgramData\Anaconda3\lib\site-packages\gensim\models\base_any2vec.py:743: UserWarning: C extension not loaded, training will be slow. Install a C compiler and reinstall gensim for fast training.
      "C extension not loaded, training will be slow. "
    2019-04-20 07:06:54,596 : INFO : collecting all words and their counts
    2019-04-20 07:06:54,600 : WARNING : this function is deprecated, use smart_open.open instead
    2019-04-20 07:06:54,608 : INFO : PROGRESS: at sentence #0, processed 0 words, keeping 0 word types
    2019-04-20 07:06:54,612 : INFO : collected 1186 word types from a corpus of 3041 raw words and 1 sentences
    2019-04-20 07:06:54,614 : INFO : Loading a fresh vocabulary
    2019-04-20 07:06:54,620 : INFO : effective_min_count=2 retains 463 unique words (39% of original 1186, drops 723)
    2019-04-20 07:06:54,624 : INFO : effective_min_count=2 leaves 2318 word corpus (76% of original 3041, drops 723)
    2019-04-20 07:06:54,636 : INFO : deleting the raw counts dictionary of 1186 items
    2019-04-20 07:06:54,640 : INFO : sample=0.001 downsamples 82 most-common words
    2019-04-20 07:06:54,642 : INFO : downsampling leaves estimated 1746 word corpus (75.4% of prior 2318)
    2019-04-20 07:06:54,647 : INFO : estimated required memory for 463 words and 300 dimensions: 1342700 bytes
    2019-04-20 07:06:54,650 : INFO : resetting layer weights
    2019-04-20 07:06:54,675 : INFO : training model with 3 workers on 463 vocabulary and 300 features, using sg=0 hs=0 sample=0.001 negative=5 window=5
    2019-04-20 07:06:54,682 : WARNING : this function is deprecated, use smart_open.open instead
    2019-04-20 07:06:54,708 : INFO : worker thread finished; awaiting finish of 2 more threads
    2019-04-20 07:06:54,722 : INFO : worker thread finished; awaiting finish of 1 more threads
    2019-04-20 07:06:55,615 : INFO : worker thread finished; awaiting finish of 0 more threads
    2019-04-20 07:06:55,620 : INFO : EPOCH - 1 : training on 3041 raw words (1726 effective words) took 0.9s, 1872 effective words/s
    2019-04-20 07:06:55,628 : WARNING : this function is deprecated, use smart_open.open instead
    2019-04-20 07:06:55,663 : INFO : worker thread finished; awaiting finish of 2 more threads
    2019-04-20 07:06:55,679 : INFO : worker thread finished; awaiting finish of 1 more threads
    2019-04-20 07:06:56,580 : INFO : worker thread finished; awaiting finish of 0 more threads
    2019-04-20 07:06:56,586 : INFO : EPOCH - 2 : training on 3041 raw words (1742 effective words) took 0.9s, 1873 effective words/s
    2019-04-20 07:06:56,594 : WARNING : this function is deprecated, use smart_open.open instead
    2019-04-20 07:06:56,628 : INFO : worker thread finished; awaiting finish of 2 more threads
    2019-04-20 07:06:56,632 : INFO : worker thread finished; awaiting finish of 1 more threads
    2019-04-20 07:06:57,494 : INFO : worker thread finished; awaiting finish of 0 more threads
    2019-04-20 07:06:57,501 : INFO : EPOCH - 3 : training on 3041 raw words (1718 effective words) took 0.9s, 1950 effective words/s
    2019-04-20 07:06:57,508 : WARNING : this function is deprecated, use smart_open.open instead
    2019-04-20 07:06:57,554 : INFO : worker thread finished; awaiting finish of 2 more threads
    2019-04-20 07:06:57,567 : INFO : worker thread finished; awaiting finish of 1 more threads
    2019-04-20 07:06:58,500 : INFO : worker thread finished; awaiting finish of 0 more threads
    2019-04-20 07:06:58,505 : INFO : EPOCH - 4 : training on 3041 raw words (1737 effective words) took 1.0s, 1804 effective words/s
    2019-04-20 07:06:58,517 : WARNING : this function is deprecated, use smart_open.open instead
    2019-04-20 07:06:58,550 : INFO : worker thread finished; awaiting finish of 2 more threads
    2019-04-20 07:06:58,558 : INFO : worker thread finished; awaiting finish of 1 more threads
    2019-04-20 07:06:59,459 : INFO : worker thread finished; awaiting finish of 0 more threads
    2019-04-20 07:06:59,466 : INFO : EPOCH - 5 : training on 3041 raw words (1754 effective words) took 0.9s, 1897 effective words/s
    2019-04-20 07:06:59,467 : INFO : training on a 15205 raw words (8677 effective words) took 4.8s, 1812 effective words/s
    2019-04-20 07:06:59,470 : WARNING : under 10 jobs per worker: consider setting a smaller `batch_words' for smoother alpha decay
    

    Wall time: 4.88 s
    


```python
model_liza.init_sims(replace=True)
model_path = "liza.bin"

print("Saving model...")
model_liza.wv.save_word2vec_format(model_path, binary=True)
```

    2019-04-20 07:06:59,497 : INFO : precomputing L2-norms of word weight vectors
    

    Saving model...
    

    2019-04-20 07:06:59,517 : INFO : storing 463x300 projection weights into liza.bin
    2019-04-20 07:06:59,522 : WARNING : this function is deprecated, use smart_open.open instead
    


```python
print(len(model_liza.wv.vocab))
```

    463
    


```python
print(sorted([w for w in model_liza.wv.vocab]))
```

    ['ангел', 'анюта', 'армия', 'ах', 'барин', 'бедный', 'белый', 'берег', 'березовый', 'беречь', 'бесчисленный', 'благодарить', 'бледный', 'блеснуть', 'блестящий', 'близ', 'бог', 'богатый', 'большой', 'бояться', 'брать', 'бросать', 'бросаться', 'бывать', 'важный', 'ввечеру', 'вдова', 'велеть', 'великий', 'великолепный', 'верить', 'верно', 'весело', 'веселый', 'весна', 'вести', 'весь', 'весьма', 'ветвь', 'ветер', 'вечер', 'взглядывать', 'вздох', 'вздыхать', 'взор', 'взять', 'вид', 'видеть', 'видеться', 'видный', 'вместе', 'вода', 'возвращаться', 'воздух', 'война', 'воображать', 'воображение', 'воспоминание', 'восторг', 'восхищаться', 'время', 'вслед', 'вставать', 'встречаться', 'всякий', 'высокий', 'выть', 'выходить', 'глаз', 'глубокий', 'гнать', 'говорить', 'год', 'голос', 'гора', 'горе', 'горестный', 'горлица', 'город', 'горький', 'господин', 'гром', 'грусть', 'давать', 'давно', 'далее', 'дверь', 'движение', 'двор', 'девушка', 'дело', 'день', 'деньги', 'деревня', 'деревянный', 'десять', 'добро', 'добрый', 'довольно', 'доживать', 'долго', 'должный', 'дом', 'домой', 'дочь', 'древний', 'друг', 'дуб', 'думать', 'душа', 'едва', 'ехать', 'жалобный', 'желание', 'желать', 'жениться', 'жених', 'женщина', 'жестокий', 'живой', 'жизнь', 'жить', 'забава', 'заблуждение', 'забывать', 'завтра', 'задумчивость', 'закраснеться', 'закричать', 'заря', 'здешний', 'здравствовать', 'зеленый', 'земля', 'златой', 'знать', 'ибо', 'играть', 'идти', 'имя', 'искать', 'исполняться', 'испугаться', 'история', 'исчезать', 'кабинет', 'казаться', 'капля', 'карета', 'карман', 'картина', 'катиться', 'клятва', 'колено', 'копейка', 'который', 'красота', 'крест', 'крестьянин', 'крестьянка', 'кровь', 'кроме', 'купить', 'ландыш', 'ласка', 'ласковый', 'левый', 'лес', 'лететь', 'летний', 'лето', 'лиза', 'лизин', 'лизина', 'лицо', 'лишний', 'лодка', 'ложиться', 'луг', 'луч', 'любезный', 'любить', 'любовь', 'лютый', 'матушка', 'мать', 'место', 'месяц', 'мечта', 'милый', 'мимо', 'минута', 'многочисленный', 'молить', 'молиться', 'молния', 'молодой', 'молодость', 'молчать', 'монастырь', 'море', 'москва', 'москварека', 'мочь', 'мрак', 'мрачный', 'муж', 'мысль', 'наглядеться', 'надеяться', 'надлежать', 'надобно', 'называть', 'наступать', 'натура', 'находить', 'наш', 'небесный', 'небо', 'невинность', 'невинный', 'нежели', 'нежный', 'незнакомец', 'немой', 'непорочность', 'неприятель', 'несколько', 'никакой', 'никто', 'ничто', 'новый', 'ночь', 'обижать', 'облако', 'обманывать', 'обморок', 'образ', 'обращаться', 'обстоятельство', 'объятие', 'огонь', 'однако', 'окно', 'окрестности', 'оно', 'опираться', 'описывать', 'опустеть', 'освещать', 'оставаться', 'оставлять', 'останавливать', 'останавливаться', 'отвечать', 'отдавать', 'отец', 'отечество', 'отменно', 'отрада', 'очень', 'падать', 'память', 'пастух', 'первый', 'перемениться', 'переставать', 'песня', 'петь', 'печальный', 'писать', 'питать', 'плакать', 'побежать', 'побледнеть', 'погибать', 'подавать', 'подгорюниваться', 'подле', 'подозревать', 'подымать', 'поехать', 'пойти', 'показываться', 'поклониться', 'покойный', 'покрывать', 'покрываться', 'покупать', 'полагать', 'поле', 'помнить', 'поселянин', 'последний', 'постой', 'потуплять', 'поцеловать', 'поцелуй', 'правый', 'представляться', 'прежде', 'преклонять', 'прекрасный', 'прелестный', 'приводить', 'прижимать', 'принадлежать', 'принуждать', 'природа', 'приходить', 'приятный', 'провожать', 'продавать', 'проливать', 'простой', 'просыпаться', 'проходить', 'проч', 'прощать', 'прощаться', 'пруд', 'птичка', 'пылать', 'пять', 'работа', 'работать', 'радость', 'рассказывать', 'расставаться', 'рвать', 'ребенок', 'река', 'решаться', 'робкий', 'роза', 'розовый', 'роман', 'российский', 'роща', 'рубль', 'рука', 'сажень', 'самый', 'свет', 'светиться', 'светлый', 'свидание', 'свирель', 'свободно', 'свой', 'свойство', 'сделать', 'сделаться', 'сей', 'сердечный', 'сердце', 'си', 'сидеть', 'сие', 'сиять', 'сказать', 'сказывать', 'сквозь', 'скорбь', 'скоро', 'скрываться', 'слабый', 'слеза', 'слезать', 'слово', 'случаться', 'слушать', 'слышать', 'смерть', 'сметь', 'смотреть', 'собственный', 'соглашаться', 'солнце', 'спасать', 'спать', 'спокойно', 'спокойствие', 'спрашивать', 'стадо', 'становиться', 'стараться', 'старуха', 'старушка', 'старый', 'статься', 'стена', 'сто', 'столь', 'стон', 'стонать', 'сторона', 'стоять', 'страшно', 'страшный', 'судьба', 'схватывать', 'счастие', 'счастливый', 'сын', 'таить', 'твой', 'темный', 'тения', 'тихий', 'тихонько', 'томный', 'трава', 'трепетать', 'трогать', 'убивать', 'уверять', 'увидеть', 'увидеться', 'удерживать', 'удивляться', 'удовольствие', 'узнавать', 'улица', 'улыбка', 'уметь', 'умирать', 'унылый', 'упасть', 'услышать', 'утешение', 'утро', 'хижина', 'хлеб', 'ходить', 'холм', 'хороший', 'хотеть', 'хотеться', 'хотя', 'худо', 'худой', 'царь', 'цветок', 'целовать', 'час', 'часто', 'человек', 'чистый', 'читатель', 'чувствительный', 'чувство', 'чувствовать', 'чудно', 'чулок', 'шестой', 'шум', 'шуметь', 'щадить', 'щека', 'эраст', 'эрастов', 'это']
    


```python
print(model_liza.wv.most_similar(positive=["смерть", "любовь"], negative=["печальный"], topn=1))

print(model_liza.wv.most_similar("любовь", topn=3))

print(model_liza.wv.similarity("лиза", "эраст"))

print(model_liza.wv.doesnt_match("скорбь грусть слеза улыбка".split()))

print(model_liza.wv.words_closer_than("лиза", "эраст"))
```

    [('сказать', 0.2698521018028259)]
    [('сказать', 0.317937970161438), ('любезный', 0.3166605830192566), ('лиза', 0.2862601578235626)]
    0.4018941
    грусть
    ['свой', 'сказать', 'сердце', 'день', 'любезный']
    


```python
urllib.request.urlretrieve("http://rusvectores.org/static/models/rusvectores2/ruscorpora_mystem_cbow_300_2_2015.bin.gz", "ruscorpora_mystem_cbow_300_2_2015.bin.gz")
```




    ('ruscorpora_mystem_cbow_300_2_2015.bin.gz',
     <http.client.HTTPMessage at 0xc228668>)




```python
m = 'ruscorpora_mystem_cbow_300_2_2015.bin.gz'

if m.endswith('.vec.gz'):
    model = gensim.models.KeyedVectors.load_word2vec_format(m, binary=False)
elif m.endswith('.bin.gz'):
    model = gensim.models.KeyedVectors.load_word2vec_format(m, binary=True)
else:
    model = gensim.models.KeyedVectors.load(m)
```

    2019-04-20 07:21:40,459 : INFO : loading projection weights from ruscorpora_mystem_cbow_300_2_2015.bin.gz
    2019-04-20 07:21:40,470 : WARNING : this function is deprecated, use smart_open.open instead
    2019-04-20 07:23:12,964 : INFO : loaded (281776, 300) matrix from ruscorpora_mystem_cbow_300_2_2015.bin.gz
    


```python
words = ['день_S', 'ночь_S', 'человек_S', 'семантика_S', 'студент_S', 'биткоин_S']
```


```python
for word in words:
    # есть ли слово в модели? 
    if word in model:
        print(word)
        # смотрим на вектор слова (его размерность 300, смотрим на первые 10 чисел)
        print(model[word][:10])
        # выдаем 10 ближайших соседей слова:
        for i in model.most_similar(positive=[word], topn=10):
            # слово + коэффициент косинусной близости
            print(i[0], i[1])
        print('\n')
    else:
        # Увы!
        print('Увы, слова "%s" нет в модели!' % word)
```

    день_S
    [-0.02580778  0.00970898  0.01941961 -0.02332282  0.02017624  0.07275085
     -0.01444375  0.03316632  0.01242602  0.02833412]
    

    2019-04-20 07:29:04,932 : INFO : precomputing L2-norms of word weight vectors
    

    неделя_S 0.7165195941925049
    месяц_S 0.6310489177703857
    вечер_S 0.5828739404678345
    утро_S 0.5676207542419434
    час_S 0.5605547428131104
    минута_S 0.5297020077705383
    гекатомбеон_S 0.4897990822792053
    денек_S 0.48224717378616333
    полчаса_S 0.48217129707336426
    ночь_S 0.478074848651886
    
    
    ночь_S
    [-0.00688948  0.00408364  0.06975466 -0.00959525  0.0194835   0.04057068
     -0.00994112  0.06064967 -0.00522624  0.00520327]
    вечер_S 0.6946249008178711
    утро_S 0.5730193257331848
    ноченька_S 0.5582467317581177
    рассвет_S 0.5553584098815918
    ночка_S 0.5351512432098389
    полдень_S 0.5334426164627075
    полночь_S 0.478694349527359
    день_S 0.478074848651886
    сумерки_S 0.4390218257904053
    фундерфун_S 0.4340824782848358
    
    
    человек_S
    [ 0.02013756 -0.02670703 -0.02039861 -0.05477146  0.00086402 -0.01636335
      0.04240306 -0.00025525 -0.14045681  0.04785006]
    женщина_S 0.5979775190353394
    парень_S 0.4991787374019623
    мужчина_S 0.4767409861087799
    мужик_S 0.47383999824523926
    россиянин_S 0.47190430760383606
    народ_S 0.46547412872314453
    согражданин_S 0.45378512144088745
    горожанин_S 0.44368088245391846
    девушка_S 0.4431447982788086
    иностранец_S 0.43849870562553406
    
    
    семантика_S
    [-0.03066749  0.0053851   0.1110732   0.0152335   0.00440643  0.00384104
      0.00096944 -0.03538784 -0.00079585  0.03220548]
    семантический_A 0.5334584712982178
    понятие_S 0.5030269026756287
    сочетаемость_S 0.4817051589488983
    актант_S 0.47596412897109985
    хронотоп_S 0.46330294013023376
    метафора_S 0.46158888936042786
    мышление_S 0.46101194620132446
    парадигма_S 0.45796656608581543
    лексема_S 0.45688071846961975
    смысловой_A 0.4543077349662781
    
    
    студент_S
    [ 0.02558023  0.0529849  -0.07036145  0.00279281 -0.09874777 -0.01620521
     -0.03918766  0.0326411   0.09191283  0.03495219]
    преподаватель_S 0.6958175301551819
    аспирант_S 0.6589953899383545
    выпускник_S 0.6523088216781616
    студентка_S 0.6321653723716736
    профессор_S 0.6080018281936646
    курсистка_S 0.5818493962287903
    юрфак_S 0.580669105052948
    первокурсник_S 0.5805511474609375
    семинарист_S 0.5773230791091919
    гимназист_S 0.5747809410095215
    
    
    Увы, слова "биткоин_S" нет в модели!
    


```python
print(model.similarity('человек_S', 'обезьяна_S'))
```

    0.23895612
    


```python
print(model.most_similar(positive=['пицца_S', 'сибирь_S'], negative=['италия_S'])[0][0])
```

    пельмень_S
    


```python
print(model.doesnt_match('мир_S жизнь_S бытие_S ничто_S'.split()))
```

    2019-04-20 07:29:21,663 : WARNING : vectors for words {'ничто_S'} are not present in the model, ignoring these words
    

    мир_S
    


```python
print(model.most_similar(positive=['жизнь_S'], negative=['любовь_S']))
      
```

    [('быт_S', 0.3733804225921631), ('жисть_S', 0.3260636329650879), ('существование_S', 0.29437491297721863), ('обстановка_S', 0.2809719443321228), ('махаль_S', 0.26148635149002075), ('жизень_S', 0.2572200894355774), ('уклад_S', 0.25639140605926514), ('скарб_S', 0.2556115388870239), ('житие_S', 0.251535028219223), ('передряга_S', 0.2505052387714386)]
    


```python
res = model.accuracy('ru_analogy_tagged.txt')
```

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: DeprecationWarning: Call to deprecated `accuracy` (Method will be removed in 4.0.0, use self.evaluate_word_analogies() instead).
      """Entry point for launching an IPython kernel.
    2019-04-20 07:53:00,410 : WARNING : this function is deprecated, use smart_open.open instead
    


```python
print(res[4]['incorrect'][:10])
```

    []
    
