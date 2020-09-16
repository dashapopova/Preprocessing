
### Distributive hypothesis in semantics

+ Ludwig Wittgenstein:
Die Bedeutung eines Wortes liegt in seinem Gebrauch.


+ Firth (1935:37) on context dependence (cited by Stubbs):
the complete meaning of a word is always contextual, and no study of meaning apart from context can be taken seriously.


+ Firth (1957:11):
You shall know a word by the company it keeps . . .


+ Harris (1954:34):
All elements in a language can be grouped into classes whose relative occurrence can be stated exactly. However, for the occurrence of a particular member of one class relative to a particular member of another class, it would be necessary to speak in terms of probability, based on the frequency of that occurrence in a sample.


+ Harris (1954:34):
It is possible to state the occurrence of any element relative to any other element, to the degree of exactness indicated above, so that distributional statements can cover all of the material of a language without requiring support from other types of information.


+ Harris (1954:34) (anticipating deep learning?):
The restrictions on relative occurrence of each element are described most simply by a network of interrelated statements, certain of them being put in terms of the results of certain others, rather than by a simple measure of the total restriction on each element separately.


+ Harris (1954:36) on levels of analysis:


    - Some question has been raised as to the reality of this structure. Does it really exist, or is it just a mathematical creation of the investigator’s? Skirting the philosophical difficulties of this problem, we should, in any case, realize that there are two quite different questions here. 
    
    - One: Does the structure really exist in language? The answer is yes, as much as any scientific structure really obtains in the data which it describes — the scientific structure states a network of relations, and these relations really hold in the data investigated.
    
    - Two: Does the structure really exist in speakers? Here we are faced with a question of fact which is not directly or fully investigated in the process of determining the distributional structure. Clearly, certain behaviors of the speakers indicate perception along the lines of the distributional structure, for example, the fact that while people imitate nonlinguistic or foreign-language sounds, they repeat utterances of their own language.


+ Harris (1954:39) on meaning and context-dependence:
All this is not to say that there is not a great interconnection between language and meaning, in whatever sense it may be possible to use this work. But it is not a one-to-one relation between morphological structure and anything else. There is not even a one-to-one relation between vocabulary and any independent classification of meaning; we cannot say that each morpheme or word has a single central meaning or even that it has a continuous or coherent range of meanings...The correlation between language and meaning is much greater when we consider connected discourse.


+ Harris (1954:43):
The fact that, for example, not every adjective occurs with every noun can be used as a measure of meaning difference. For it is not merely that different members of the one class have different selections of members of the other class with which they are actually found. More than that: if we consider words or morphemes A and B to be more different than A and C, then we will often find that the distributions of A and B are more different than the distributions of A and C. In other words, difference in meaning correlates with difference in distribution.


+ Turney & Pantel (2010:153):


    - Statistical semantics hypothesis: Statistical patterns of human word usage can be used to figure out what people mean (Weaver, 1955; Furnas et al., 1983). – If units of text have similar vectors in a text frequency matrix, then they tend to have similar meanings. (We take this to be a general hypothesis that subsumes the four more specific hypotheses that follow.)

    - Bag of words hypothesis: The frequencies of words in a document tend to indicate the relevance of the document to a query (Salton et al., 1975). – If documents and pseudo-documents (queries) have similar column vectors in a term–document matrix, then they tend to have similar meanings.

    - Distributional hypothesis: Words that occur in similar contexts tend to have similar meanings (Harris, 1954; Firth, 1957; Deerwester et al., 1990). – If words have similar row vectors in a word–context matrix, then they tend to have similar meanings.
      
    - Extended distributional hypothesis: Patterns that co-occur with similar pairs tend to have similar meanings (Lin & Pantel, 2001). – If patterns have similar column vectors in a pair–pattern matrix, then they tend to express similar semantic relations.

    - Latent relation hypothesis: Pairs of words that co-occur in similar patterns tend to have similar semantic relations (Turney et al., 2003). – If word pairs have similar row vectors in a pair–pattern matrix, then they tend to have similar semantic relations.
    
    
+ What is the meaning of the word "bardiwac" (Stefan Evert's example)?

    - He handed her her glass of bardiwac.

    - Beef dishes are made to complement the bardiwacs.

    - Nigel staggered to his feet, face flushed from too much bardiwac.

    - Malbec, one of the lesser-known bardiwac grapes, responds well to Australia’s sunshine.

    - I dined off bread and cheese and this excellent bardiwac.

    - The drinks were delicious: blood-red bardiwac as well as light, sweet Rhenish.

#### Word2Vec

One of the most famous distributional models is word2vec. The model is based on a neural network that predicts the probability of occurrence of a given word in a given context. The two seminal papers are linked below:

+ [Efficient Estimation of Word Representations inVector Space](https://arxiv.org/pdf/1301.3781.pdf)
+ [Distributed Representations of Words and Phrases and their Compositionality](https://arxiv.org/abs/1310.4546)

The model produces word representations in a form of a vector, or, an embedding.

Word2Vec comprises two algorithms: Skip-Gram and Continuous Bag-Of-Words (CBOW). The CBOW architecture predicts the current word based on the context, and the Skip-gram predicts surrounding words given the current word.

#### How does word2vec work?

Word2Vec takes a corpus as an input and creates a vector for each word. Vectors (embeddings) are created based on the distributional hypothesis. Cosine similarity between embeddings reflects similarity in the semantics of the words.

We can use embeddings to create analogies:

+ king: man = queen: woman $\Rightarrow$
+ king - man + woman = queen

![w2v](https://cdn-images-1.medium.com/max/2600/1*sXNXYfAqfLUeiDXPCo130w.png)

More on the mechanics you can find [here](https://habr.com/ru/post/446530/)

#### Why do we need it?

+ to solve semantic problems
+ for which classes of words is the distributional hypothesis most useful?
+ some papers on its use in semantics:

* [Turney and Pantel 2010](https://jair.org/index.php/jair/article/view/10640)
* [Lenci 2018](https://www.annualreviews.org/doi/abs/10.1146/annurev-linguistics-030514-125254?journalCode=linguistics)
* [Smith 2019](https://arxiv.org/pdf/1902.06006.pdf)
* [Pennington et al. 2014](https://www.aclweb.org/anthology/D14-1162/)
* [Faruqui et al. 2015](https://www.aclweb.org/anthology/N15-1184/)

+ to create input for neural networks
+ word2vec is used in Siri, Google Assistant, Alexa, Google Translate...

#### Gensim

We will use the `gensim` library to get access to the word2vec model. Here you can find the library's [documentation](https://radimrehurek.com/gensim/models/word2vec.html).

First, let's install the library: `pip install gensim`. You can do it from jupyter: `!pip install gensim`.


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

import warnings
warnings.filterwarnings('ignore')
```

    C:\ProgramData\Anaconda3\lib\site-packages\smart_open\ssh.py:34: UserWarning: paramiko missing, opening SSH/SCP/SFTP paths will be disabled.  `pip install paramiko` to suppress
      warnings.warn('paramiko missing, opening SSH/SCP/SFTP paths will be disabled.  `pip install paramiko` to suppress')
    

#### How to train your own model

NB! The training does not involve preprocessing! It means that, if necessary for your task, you have to get rid of the punctuation, lower, lemmatize, do the pos tagging before the training.

To log the training:


```python
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)
```

Below is the preprocessing part for liza.txt. You don't need to execute it in class, since you can download liza_lem.txt.


```python
from tqdm import tqdm
from pymystem3 import Mystem

m = Mystem()
sw = stopwords.words('russian')

with open('liza.txt', 'r', encoding='utf8') as f:
    text = f.readlines()

new_lines = []

for line in tqdm(text):
    line = ' '.join([w for w in line.split() if w not in sw])
    newline = ''.join(m.lemmatize(line))
    new_lines.append(newline)
        
with open('liza_lem.txt', 'w', encoding='utf8') as f1:
    for line in new_lines:
        f1.write(line)
```

    100%|████████████████████████████████████████████| 3/3 [00:12<00:00,  4.17s/it]
    

The input for the model is a text file, where every sentence starts on a new line. The text is stripped of the punctuation, lowered and lemmatized.


```python
f = 'liza_lem.txt'
data = gensim.models.word2vec.LineSentence(f)
```

We will be training our model now. The main parameters:

+ the data should be iterable
+ size — dimensionality of the word vectors,
+ window — maximum distance between the current and predicted word within a sentence,
+ min_count — ignores all words with total frequency lower than this,
+ sg —  training algorithm: 1 for skip-gram; otherwise CBOW,
+ sample — the threshold for configuring which higher-frequency words are randomly downsampled,
+ iter — number of iterations (epochs) over the corpus,
+ max_vocab_size — limits the RAM during vocabulary building; if there are more unique words than this, then prune the infrequent ones. Every 10 million word types need about 1GB of RAM. Set to None for no limit.


```python
%time model_liza = gensim.models.Word2Vec(data, size=300, window=5, min_count=2)
```

    2020-09-16 15:11:14,313 : INFO : collecting all words and their counts
    2020-09-16 15:11:14,313 : WARNING : this function is deprecated, use smart_open.open instead
    2020-09-16 15:11:14,323 : INFO : PROGRESS: at sentence #0, processed 0 words, keeping 0 word types
    2020-09-16 15:11:14,323 : INFO : collected 1189 word types from a corpus of 3043 raw words and 1 sentences
    2020-09-16 15:11:14,343 : INFO : Loading a fresh vocabulary
    2020-09-16 15:11:14,353 : INFO : effective_min_count=2 retains 463 unique words (38% of original 1189, drops 726)
    2020-09-16 15:11:14,393 : INFO : effective_min_count=2 leaves 2317 word corpus (76% of original 3043, drops 726)
    2020-09-16 15:11:14,395 : INFO : deleting the raw counts dictionary of 1189 items
    2020-09-16 15:11:14,395 : INFO : sample=0.001 downsamples 82 most-common words
    2020-09-16 15:11:14,395 : INFO : downsampling leaves estimated 1746 word corpus (75.4% of prior 2317)
    2020-09-16 15:11:14,405 : INFO : estimated required memory for 463 words and 300 dimensions: 1342700 bytes
    2020-09-16 15:11:14,415 : INFO : resetting layer weights
    2020-09-16 15:11:14,435 : INFO : training model with 3 workers on 463 vocabulary and 300 features, using sg=0 hs=0 sample=0.001 negative=5 window=5
    2020-09-16 15:11:14,445 : WARNING : this function is deprecated, use smart_open.open instead
    2020-09-16 15:11:14,465 : INFO : worker thread finished; awaiting finish of 2 more threads
    2020-09-16 15:11:14,475 : INFO : worker thread finished; awaiting finish of 1 more threads
    2020-09-16 15:11:15,425 : INFO : worker thread finished; awaiting finish of 0 more threads
    2020-09-16 15:11:15,425 : INFO : EPOCH - 1 : training on 3043 raw words (1706 effective words) took 1.0s, 1764 effective words/s
    2020-09-16 15:11:15,435 : WARNING : this function is deprecated, use smart_open.open instead
    2020-09-16 15:11:15,485 : INFO : worker thread finished; awaiting finish of 2 more threads
    2020-09-16 15:11:15,485 : INFO : worker thread finished; awaiting finish of 1 more threads
    2020-09-16 15:11:16,575 : INFO : EPOCH 2 - PROGRESS: at 100.00% examples, 1569 words/s, in_qsize 0, out_qsize 1
    2020-09-16 15:11:16,575 : INFO : worker thread finished; awaiting finish of 0 more threads
    2020-09-16 15:11:16,585 : INFO : EPOCH - 2 : training on 3043 raw words (1731 effective words) took 1.1s, 1556 effective words/s
    2020-09-16 15:11:16,595 : WARNING : this function is deprecated, use smart_open.open instead
    2020-09-16 15:11:16,625 : INFO : worker thread finished; awaiting finish of 2 more threads
    2020-09-16 15:11:16,645 : INFO : worker thread finished; awaiting finish of 1 more threads
    2020-09-16 15:11:17,727 : INFO : EPOCH 3 - PROGRESS: at 100.00% examples, 1563 words/s, in_qsize 0, out_qsize 1
    2020-09-16 15:11:17,737 : INFO : worker thread finished; awaiting finish of 0 more threads
    2020-09-16 15:11:17,737 : INFO : EPOCH - 3 : training on 3043 raw words (1743 effective words) took 1.1s, 1551 effective words/s
    2020-09-16 15:11:17,747 : WARNING : this function is deprecated, use smart_open.open instead
    2020-09-16 15:11:17,787 : INFO : worker thread finished; awaiting finish of 2 more threads
    2020-09-16 15:11:17,787 : INFO : worker thread finished; awaiting finish of 1 more threads
    2020-09-16 15:11:18,727 : INFO : worker thread finished; awaiting finish of 0 more threads
    2020-09-16 15:11:18,737 : INFO : EPOCH - 4 : training on 3043 raw words (1748 effective words) took 1.0s, 1829 effective words/s
    2020-09-16 15:11:18,747 : WARNING : this function is deprecated, use smart_open.open instead
    2020-09-16 15:11:18,797 : INFO : worker thread finished; awaiting finish of 2 more threads
    2020-09-16 15:11:18,797 : INFO : worker thread finished; awaiting finish of 1 more threads
    2020-09-16 15:11:19,799 : INFO : EPOCH 5 - PROGRESS: at 100.00% examples, 1697 words/s, in_qsize 0, out_qsize 1
    2020-09-16 15:11:19,809 : INFO : worker thread finished; awaiting finish of 0 more threads
    2020-09-16 15:11:19,809 : INFO : EPOCH - 5 : training on 3043 raw words (1737 effective words) took 1.0s, 1682 effective words/s
    2020-09-16 15:11:19,809 : INFO : training on a 15215 raw words (8665 effective words) took 5.4s, 1613 effective words/s
    2020-09-16 15:11:19,819 : WARNING : under 10 jobs per worker: consider setting a smaller `batch_words' for smoother alpha decay
    

    Wall time: 5.52 s
    

We can normalize the vectors, then the model would take up less RAM. After this operation, however, you won't be able to retrain the model. L2-normalization is used: the sum of squares of all the vector elements will be brought to 1. 


```python
model_liza.init_sims(replace=True)
model_path = "liza.bin"

print("Saving model...")
model_liza.wv.save_word2vec_format(model_path, binary=True)
```

    2020-09-16 15:11:25,841 : INFO : precomputing L2-norms of word weight vectors
    

    Saving model...
    

    2020-09-16 15:11:25,920 : INFO : storing 463x300 projection weights into liza.bin
    2020-09-16 15:11:25,924 : WARNING : this function is deprecated, use smart_open.open instead
    

Let's count the number of words in the model:


```python
print(len(model_liza.wv.vocab))
```

    463
    


```python
print(sorted([w for w in model_liza.wv.vocab]))
```

    ['ангел', 'анюта', 'армия', 'ах', 'барин', 'бедный', 'белый', 'берег', 'березовый', 'беречь', 'бесчисленный', 'благодарить', 'бледный', 'блеснуть', 'блестящий', 'близ', 'бог', 'богатый', 'большой', 'бояться', 'брать', 'бросать', 'бросаться', 'бывать', 'важный', 'ввечеру', 'вдова', 'велеть', 'великий', 'великолепный', 'верить', 'верно', 'весело', 'веселый', 'весна', 'вести', 'весь', 'весьма', 'ветвь', 'ветер', 'вечер', 'взглядывать', 'вздох', 'вздыхать', 'взор', 'взять', 'вид', 'видеть', 'видеться', 'видный', 'вместе', 'вода', 'возвращаться', 'воздух', 'война', 'воображать', 'воображение', 'воспоминание', 'восторг', 'восхищаться', 'время', 'вслед', 'вставать', 'встречаться', 'всякий', 'высокий', 'выть', 'выходить', 'глаз', 'глубокий', 'гнать', 'говорить', 'год', 'голос', 'гора', 'горе', 'горестный', 'горлица', 'город', 'горький', 'господин', 'гром', 'грусть', 'давать', 'давно', 'далее', 'дверь', 'движение', 'двор', 'девушка', 'дело', 'день', 'деньги', 'деревня', 'деревянный', 'десять', 'добро', 'добрый', 'довольно', 'доживать', 'долго', 'должный', 'дом', 'домой', 'дочь', 'древний', 'друг', 'дуб', 'думать', 'душа', 'едва', 'ехать', 'жалобный', 'желание', 'желать', 'жениться', 'жених', 'женщина', 'жестокий', 'живой', 'жизнь', 'жить', 'забава', 'заблуждение', 'забывать', 'завтра', 'задумчивость', 'закраснеться', 'закричать', 'заря', 'здешний', 'здравствовать', 'зеленый', 'земля', 'златой', 'знать', 'ибо', 'играть', 'идти', 'имя', 'искать', 'исполняться', 'испугаться', 'история', 'исчезать', 'кабинет', 'казаться', 'капля', 'карета', 'карман', 'картина', 'катиться', 'клятва', 'колено', 'копейка', 'который', 'красота', 'крест', 'крестьянин', 'крестьянка', 'кровь', 'кроме', 'купить', 'ландыш', 'ласка', 'ласковый', 'левый', 'лес', 'лететь', 'летний', 'лето', 'лиза', 'лизин', 'лизина', 'лицо', 'лишний', 'лодка', 'ложиться', 'луг', 'луч', 'любезный', 'любить', 'любовь', 'лютый', 'матушка', 'мать', 'место', 'месяц', 'мечта', 'милый', 'мимо', 'минута', 'многочисленный', 'молить', 'молиться', 'молния', 'молодой', 'молодость', 'молчать', 'монастырь', 'море', 'москва', 'москварека', 'мочь', 'мрак', 'мрачный', 'муж', 'мысль', 'наглядеться', 'надеяться', 'надлежать', 'надобно', 'называть', 'наступать', 'натура', 'находить', 'наш', 'небесный', 'небо', 'невинность', 'невинный', 'нежели', 'нежный', 'незнакомец', 'немой', 'непорочность', 'неприятель', 'несколько', 'никакой', 'никто', 'ничто', 'новый', 'ночь', 'обижать', 'облако', 'обманывать', 'обморок', 'образ', 'обращаться', 'обстоятельство', 'объятие', 'огонь', 'однако', 'окно', 'окрестности', 'оно', 'опираться', 'описывать', 'опустеть', 'освещать', 'оставаться', 'оставлять', 'останавливать', 'останавливаться', 'отвечать', 'отдавать', 'отец', 'отечество', 'отменно', 'отрада', 'очень', 'падать', 'память', 'пастух', 'первый', 'перемениться', 'переставать', 'песня', 'петь', 'печальный', 'писать', 'питать', 'плакать', 'побежать', 'побледнеть', 'погибать', 'подавать', 'подгорюниваться', 'подле', 'подозревать', 'подымать', 'поехать', 'пойти', 'показываться', 'поклониться', 'покойный', 'покрывать', 'покрываться', 'покупать', 'полагать', 'поле', 'помнить', 'поселянин', 'последний', 'постой', 'потуплять', 'поцеловать', 'поцелуй', 'правый', 'представляться', 'прежде', 'преклонять', 'прекрасный', 'прелестный', 'приводить', 'прижимать', 'принадлежать', 'принуждать', 'природа', 'приходить', 'приятный', 'провожать', 'продавать', 'проливать', 'простой', 'просыпаться', 'проходить', 'проч', 'прощать', 'прощаться', 'пруд', 'птичка', 'пылать', 'пять', 'работа', 'работать', 'радость', 'рассказывать', 'расставаться', 'рвать', 'ребенок', 'река', 'решаться', 'робкий', 'роза', 'розовый', 'роман', 'российский', 'роща', 'рубль', 'рука', 'сажень', 'самый', 'свет', 'светиться', 'светлый', 'свидание', 'свирель', 'свободно', 'свой', 'свойство', 'сделать', 'сделаться', 'сей', 'сердечный', 'сердце', 'си', 'сидеть', 'сие', 'сиять', 'сказать', 'сказывать', 'сквозь', 'скорбь', 'скоро', 'скрываться', 'слабый', 'слеза', 'слезать', 'слово', 'случаться', 'слушать', 'слышать', 'смерть', 'сметь', 'смотреть', 'собственный', 'соглашаться', 'солнце', 'спасать', 'спать', 'спокойно', 'спокойствие', 'спрашивать', 'стадо', 'становиться', 'стараться', 'старуха', 'старушка', 'старый', 'статься', 'стена', 'сто', 'столь', 'стон', 'стонать', 'сторона', 'стоять', 'страшно', 'страшный', 'судьба', 'схватывать', 'счастие', 'счастливый', 'сын', 'таить', 'твой', 'темный', 'тения', 'тихий', 'тихонько', 'томный', 'трава', 'трепетать', 'трогать', 'убивать', 'уверять', 'увидеть', 'увидеться', 'удерживать', 'удивляться', 'удовольствие', 'узнавать', 'улица', 'улыбка', 'уметь', 'умирать', 'унылый', 'упасть', 'услышать', 'утешение', 'утро', 'хижина', 'хлеб', 'ходить', 'холм', 'хороший', 'хотеть', 'хотеться', 'хотя', 'худо', 'худой', 'царь', 'цветок', 'целовать', 'час', 'часто', 'человек', 'чистый', 'читатель', 'чувствительный', 'чувство', 'чувствовать', 'чудно', 'чулок', 'шестой', 'шум', 'шуметь', 'щадить', 'щека', 'эраст', 'эрастов', 'это']
    

Let's see what the model learned:


```python
model_liza.wv.most_similar(positive=["смерть", "любовь"], negative=["печальный"], topn=3)
```




    [('девушка', 0.22612610459327698),
     ('ах', 0.2246483862400055),
     ('лодка', 0.1944190263748169)]




```python
model_liza.wv.most_similar("любовь", topn=3)
```




    [('ах', 0.29700374603271484),
     ('чистый', 0.2393895536661148),
     ('свой', 0.23854082822799683)]




```python
model_liza.wv.similarity("лиза", "эраст")
```




    0.38255787




```python
model_liza.wv.similarity("лиза", "лиза")
```




    0.99999994




```python
model_liza.wv.doesnt_match("скорбь грусть слеза улыбка".split())
```




    'скорбь'




```python
model_liza.wv.words_closer_than("лиза", "эраст")
```




    ['хотеть']



#### Parameter variation

1) preprocessing -- do we lemmatize, tokenize, pos-tag or not

2) corpus size -- the greater, the better; but! for semantic tasks the quality is more important than quantity

3) vocabulary size

4) negative samples

5) the number of iterations

6) vector size -- 100-300 (it look like >300 does not make the results better)

7) window size -- for syntax -- around 4, for semantics -- 8, 10.

A paper that discusses different parameter settings: https://www.aclweb.org/anthology/D14-1162.pdf

#### How to use a pre-trained model

#### RusVectōrēs

RusVectōrēs (https://rusvectores.org/ru/) provides a number of pre-trained models for Russian.

For other languages, look at [fastText](https://fasttext.cc/docs/en/english-vectors.html) and [GloVe](https://nlp.stanford.edu/projects/glove/)

Let's also look at some vector novels https://nevmenandr.github.io/novel2vec/

#### Working with a model

Word2vec models can have two formats:

+ .vec.gz — an ordinary file
+ .bin.gz — a binary file

To load a word2vec model, use `KeyedVectors`, you can set the `binary` parameter of the function `load_word2vec_format`.

Note that if the embeddings were created not by word2vec, you need to use `load`. Use it if you load the `glove`, `fasttext`, `bpe` embeddings.

Let's load a RusVectōrēs model for Russian, trained on Russian National Corpus 2015.


```python
urllib.request.urlretrieve("http://rusvectores.org/static/models/rusvectores2/ruscorpora_mystem_cbow_300_2_2015.bin.gz", "ruscorpora_mystem_cbow_300_2_2015.bin.gz")
```




    ('ruscorpora_mystem_cbow_300_2_2015.bin.gz',
     <http.client.HTTPMessage at 0xba70c88>)




```python
m = 'ruscorpora_mystem_cbow_300_2_2015.bin.gz'

if m.endswith('.vec.gz'):
    model = gensim.models.KeyedVectors.load_word2vec_format(m, binary=False)
elif m.endswith('.bin.gz'):
    model = gensim.models.KeyedVectors.load_word2vec_format(m, binary=True)
else:
    model = gensim.models.KeyedVectors.load(m)
```

    2020-09-16 16:06:43,338 : INFO : loading projection weights from ruscorpora_mystem_cbow_300_2_2015.bin.gz
    2020-09-16 16:06:44,768 : WARNING : this function is deprecated, use smart_open.open instead
    2020-09-16 16:08:54,040 : INFO : loaded (281776, 300) matrix from ruscorpora_mystem_cbow_300_2_2015.bin.gz
    


```python
words = ['хороший_A', 'плохой_A', 'ужасный_A','жуткий_A', 'страшный_A', 'красный_A', 'синий_A']
```

We need the POS tags, because the model was trained on lemmatized and tagged words. The name of the model specifies the algorythm that was used to tag the words, mystem, in our case.

Let's look at the 10 closest members for each word that we are interested in and at the cosine similarity.



```python
for word in words:
    # is the word present in the model?
    if word in model:
        print(word)
        # looking at the first 10 numbers from the embedding 
        print(model[word][:10])
        # getting 10 neighbours
        for i in model.most_similar(positive=[word], topn=10):
            # word + cosine similarity
            print(i[0], i[1])
        print('\n')
    else:
        # Oops!
        print('Oops, the word "%s" is not in the model!' % word)
```

    хороший_A
    [ 0.00722357 -0.00361956  0.1272455   0.06584469  0.00709477 -0.02014845
     -0.02056034  0.01321563  0.13692418 -0.09624264]
    

    2020-09-16 16:12:49,717 : INFO : precomputing L2-norms of word weight vectors
    

    плохой_A 0.7463520765304565
    неплохой_A 0.6708558797836304
    отличный_A 0.6633436679840088
    превосходный_A 0.6079519987106323
    замечательный_A 0.586450457572937
    недурной_A 0.5322482585906982
    отменный_A 0.5168066024780273
    прекрасный_A 0.4982394576072693
    посредственный_A 0.49099433422088623
    приличный_A 0.48622459173202515
    
    
    плохой_A
    [-0.05218472  0.0307817   0.1459371   0.0151835   0.06219714  0.01153753
     -0.01169093  0.01818374  0.0955373  -0.10191503]
    хороший_A 0.7463520765304565
    дурной_A 0.6186875700950623
    скверный_A 0.6014161109924316
    отличный_A 0.5226833820343018
    посредственный_A 0.5061031579971313
    неважный_A 0.5021153092384338
    неплохой_A 0.49169063568115234
    никудышный_A 0.48035895824432373
    ухудшать_V 0.43680471181869507
    плохо_ADV 0.4314875304698944
    
    
    ужасный_A
    [-0.05553271 -0.03172469  0.01998607  0.00171507 -0.00935555 -0.0296017
      0.05394973  0.01597532 -0.03785459 -0.02099892]
    страшный_A 0.8007249236106873
    жуткий_A 0.6982528567314148
    отвратительный_A 0.6798903942108154
    ужасающий_A 0.6174499988555908
    чудовищный_A 0.6100855469703674
    постыдный_A 0.6009703874588013
    невероятный_A 0.5827823281288147
    ужасать_V 0.5815353393554688
    кошмарный_A 0.5675789713859558
    позорный_A 0.5351496338844299
    
    
    жуткий_A
    [-0.07627533 -0.06143281 -0.02622319 -0.03769541 -0.00350412 -0.01479934
      0.03325103  0.06712756 -0.0044996   0.0145266 ]
    ужасный_A 0.6982529163360596
    страшный_A 0.6917036771774292
    зловещий_A 0.6490103006362915
    странный_A 0.6009964942932129
    отвратительный_A 0.5856714248657227
    тоскливый_A 0.5783498287200928
    кошмарный_A 0.5670032501220703
    гнетущий_A 0.5607055425643921
    чудовищный_A 0.5550791621208191
    мрачный_A 0.5542315244674683
    
    
    страшный_A
    [-0.12759186 -0.0206753   0.00979353 -0.02963523  0.03109632  0.02121338
     -0.02869159  0.02574235 -0.02556899 -0.03742376]
    ужасный_A 0.800724983215332
    жуткий_A 0.6917036771774292
    чудовищный_A 0.5934231877326965
    кошмарный_A 0.5395629405975342
    отвратительный_A 0.5351147651672363
    невероятный_A 0.520784854888916
    ужасающий_A 0.5174920558929443
    зловещий_A 0.5163233280181885
    жестокий_A 0.5096044540405273
    ужасать_V 0.5071669816970825
    
    
    красный_A
    [ 0.01627072 -0.01136785 -0.00790482  0.02294072  0.05129128  0.10162549
      0.07488654 -0.06475785 -0.0203686   0.09159683]
    алый_A 0.642128586769104
    малиновый_A 0.6113020777702332
    красная_S 0.5526680946350098
    желтый_A 0.5431625247001648
    оранжевый_A 0.5371882319450378
    трехцветный_A 0.531793475151062
    пунцовый_A 0.5125025510787964
    синий_A 0.5102002024650574
    фиолетовый_A 0.5072877407073975
    лиловый_A 0.5004072785377502
    
    
    синий_A
    [-0.00614284  0.04970241 -0.00461786 -0.11465221  0.08177482  0.00020589
      0.04895581  0.02750725 -0.05211812  0.06006202]
    голубой_A 0.855513334274292
    темно-синий_A 0.7498061656951904
    оранжевый_A 0.7341035008430481
    лиловый_A 0.7314398288726807
    фиолетовый_A 0.7291390895843506
    желтый_A 0.7263568639755249
    ярко-синий_A 0.6990128755569458
    черный_A 0.6909208297729492
    зеленый_A 0.6799378395080566
    коричневый_A 0.6729934215545654
    
    
    

Cosine similarity for a pair of words:


```python
print(model.similarity('плохой_A', 'хороший_A'))
```

    0.7463521
    


```python
print(model.similarity('плохой_A', 'синий_A'))
```

    -0.12778336
    


```python
print(model.similarity('ужасный_A', 'жуткий_A'))
```

    0.6982529
    

Proportion:

+ positive — vectors that we add
+ negative — vectors that we subtract


```python
print(model.most_similar(positive=['плохой_A', 'ужасный_A'], negative=['хороший_A'])[0][0])
```

    страшный_A
    

Find the word that does not match the rest of the words:


```python
print(model.doesnt_match('плохой_A хороший_A ужасный_A страшный_A'.split()))
```

    хороший_A
    


```python
for word, score in model.most_similar(positive=['ужасно_ADV'], negative=['плохой_A']):
    print(f'{score:.4}\t{word}')
```

    0.5575	безумно_ADV
    0.4791	безмерно_ADV
    0.4536	жутко_ADV
    0.4472	невероятно_ADV
    0.4394	очень_ADV
    0.4364	чертовски_ADV
    0.4231	страшно_ADV
    0.4124	необычайно_ADV
    0.4119	нестерпимо_ADV
    0.4005	необыкновенно_ADV
    

#### Visualization of the data

Many ways to do it, let's try PCA (Principle Component Analysis, reduces the number of dimensions)


```python
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
words = ['хороший_A', 'плохой_A', 'ужасный_A','жуткий_A', 'страшный_A', 'красный_A', 'синий_A']
X = model[words]
```

Executed on the list on words:


```python
pca = PCA(n_components=2)
coords = pca.fit_transform(X)
```


```python
plt.scatter(coords[:, 0], coords[:, 1], color='red')
plt.title('Words')

for i, word in enumerate(words):
    plt.annotate(word, xy=(coords[i, 0], coords[i, 1]))
plt.show()
```


![png](output_42_0.png)


Executed on all the words in the model:


```python
pca = PCA(n_components=2)
pca.fit(model[list(model.vocab)])
coords = pca.transform(model[words])
```


```python
plt.scatter(coords[:, 0], coords[:, 1], color='red')
plt.title('Words')

for i, word in enumerate(words):
    plt.annotate(word, xy=(coords[i, 0], coords[i, 1]))
plt.show()
```


![png](output_45_0.png)


#### Model evaluation

+ word similarity, compare the results of the training with experimental results from human participants
+ analogies:

| слово 1    | слово 2    | отношение     | 
|------------|------------|---------------|
| Россия     | Москва     | страна-столица| 
| Норвегия   | Осло       | страна-столица|


```python
res = model.accuracy('ru_analogy_tagged.txt')
```


```python
for row in res[4]['incorrect'][:10]:
    print('\t'.join(row))
```
