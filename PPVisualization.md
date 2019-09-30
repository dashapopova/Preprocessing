
## Visualization

**Our plan for today:**

+ pandas - to organize the data
+ Matplotlib
+ Seaborn
+ Wordcloud




```python
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import random
random.seed = 23
```

#### A simple plot

Точки по х и у соединены линиями. Нужен, если есть однозначное соответствие х и у и мы хотим показать как при изменении х меняется у. Например, по х может быть время, а по у - частотность слова (как на графиках в НКРЯ).



```python
X = list(range(2010, 2020))
Y = [random.randint(i*10, (i+1)*20) for i in range(len(X))]
print('X:', X)
print('Y:', Y)

plt.plot(X, Y) # рисуем график - последовательно соединяем точки с координатами из X и Y
plt.title('Frequency of some random word') # заголовок
plt.ylabel('Frequency') # подпись оси Х
plt.xlabel('Year') # подпись оси Y
plt.show()
```

    X: [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019]
    Y: [3, 14, 45, 59, 71, 58, 100, 129, 140, 96]
    


![png](output_3_1.png)


#### Scatter plot

Точки, как и раньше, задаются по х и у, но теперь не соединяются линиями. Такие гарфики используются для отображения наблюдений в эксперименте, когда есть два параметра, которые могут принимать различные значения и нужно показать, какие комюбинации есть и как они раположены.



```python
X = [1.2, 2, 3.1, 4, 5.1, 1.7, 1.5, 3.5, 4.5, 4, 2, 2, 2.7, 3.1, 4.1]
Y = [1, 1.4, 3, 4, 5, 1.5, 1.3, 3.5, 4.5, 4.5, 1.3, 2.1, 3.2, 5.1, 4.9]

plt.scatter(X, Y, color='purple', label='group 1') # меняем цвет, добавляем label
plt.scatter(Y, X, color='orange', label='group 2') # нарисуем еще какие-то значения на том же графике
plt.title('Combinations of two variables\n in our imaginary experiment') # перенос строки
plt.ylabel('Some other variable')
plt.xlabel('Some variable')
plt.legend(loc='best') # автоматический поиск места для легенды
plt.show()
```


![png](output_5_0.png)



```python
X = [1.2, 2, 3.1, 4, 5.1, 1.7, 1.5, 3.5, 4.5, 4, 2, 2, 2.7, 3.1, 4.1]
Y = [1, 1.4, 3, 4, 5, 1.5, 1.3, 3.5, 4.5, 4.5, 1.3, 2.1, 3.2, 5.1, 4.9]
labels = 'abcdefghijklmno'
plt.figure(figsize=(6, 6)) # размер графика
plt.scatter(X, Y, color='red', label='group 1', marker='^') # маркер - треугольник
for x, y, key in zip(X, Y, range(len(X))):
    plt.text(x+0.2, y, labels[key]) # подписи
plt.title('Combinations of two variables\n in our imaginary experiment')
plt.ylabel('Some other variable')
plt.xlabel('Some variable')
plt.xlim((0, 7)) # предел по х
plt.ylim((0, 7)) # предел по у
plt.show()
```


![png](output_6_0.png)


#### Bar plot

Столбчатая диграмма - для категориальных данных по х и чисел по у, например, если у нас есть дни недели и среднее количество ругательств, которое человек произносит в этот день.



```python
x = [1, 2, 3, 4, 5]
x2 = [6, 7] # сделаем выходные отдельно
y = [30, 15, 17, 15, 10]
y2 = [7, 3]
days = ['пн', 'вт', 'ср', 'чт', 'пт', 'сб', 'вс']
plt.bar(x, y, color='grey')
plt.bar(x2, y2, color='pink')
plt.xticks(x + x2, days, rotation='vertical')
plt.title('Среднее количество ругательств по дням недели')
plt.ylabel('Среднее кол-во ругательств')
plt.xlabel('День недели')
plt.show()
```


![png](output_8_0.png)


#### Heatmap

Хитмэп нужен, когда у нас есть 3 переменные.



```python
X = [
    [1, 2, 3, 4, 5],
    [2, 3, 4, 5, 6],
    [3, 4, 5, 6, 7],
    [4, 5, 6, 7, 8],
    [5, 6, 7, 8, 9],
]
sns.heatmap(
    X, # матрица значений
    annot=True, # значения из матрицы
    xticklabels=DAYS[:5],
    yticklabels=[f'степень {i}' for i in range(0, 5)]
)
plt.xlabel('День недели')
plt.ylabel('Степень выраженности ругания')
plt.ylabel('Количесто ругающихся людей\n по дню недели и степени выраженности ругания')
plt.show()
```


![png](output_10_0.png)


#### Pandas


```python
import pandas as pd
```


```python
tolstoy = pd.read_csv('tolstoy.csv', sep='\t').fillna('') # читаем файл и пустые значения заполняем пустыми строками
```

    C:\ProgramData\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py:2785: DtypeWarning: Columns (21,23,24) have mixed types. Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)
    


```python
tolstoy.head(10)
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
      <th>lex</th>
      <th>word</th>
      <th>POS</th>
      <th>time</th>
      <th>gender</th>
      <th>case</th>
      <th>number</th>
      <th>verbal</th>
      <th>adj_form</th>
      <th>comp</th>
      <th>...</th>
      <th>имя</th>
      <th>отч</th>
      <th>фам</th>
      <th>вводн</th>
      <th>гео</th>
      <th>сокр</th>
      <th>обсц</th>
      <th>разг</th>
      <th>редк</th>
      <th>устар</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>том</td>
      <td>том</td>
      <td>S</td>
      <td></td>
      <td>муж</td>
      <td>вин</td>
      <td>ед</td>
      <td></td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>первый</td>
      <td>первый</td>
      <td>ANUM</td>
      <td></td>
      <td>муж</td>
      <td>вин</td>
      <td>ед</td>
      <td></td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>часть</td>
      <td>часть</td>
      <td>S</td>
      <td></td>
      <td>жен</td>
      <td>вин</td>
      <td>ед</td>
      <td></td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>первый</td>
      <td>первая</td>
      <td>ANUM</td>
      <td></td>
      <td>жен</td>
      <td>им</td>
      <td>ед</td>
      <td></td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>ну</td>
      <td>ну</td>
      <td>PART</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td>здравствовать</td>
      <td>здравствуйте</td>
      <td>V</td>
      <td></td>
      <td></td>
      <td></td>
      <td>мн</td>
      <td>пов</td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>6</th>
      <td>здравствовать</td>
      <td>здравствуйте</td>
      <td>V</td>
      <td></td>
      <td></td>
      <td></td>
      <td>мн</td>
      <td>пов</td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>7</th>
      <td>садиться</td>
      <td>садитесь</td>
      <td>V</td>
      <td>непрош</td>
      <td></td>
      <td></td>
      <td>мн</td>
      <td>изъяв</td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>8</th>
      <td>и</td>
      <td>и</td>
      <td>CONJ</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>9</th>
      <td>рассказывать</td>
      <td>рассказывайте</td>
      <td>V</td>
      <td></td>
      <td></td>
      <td></td>
      <td>мн</td>
      <td>пов</td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>10 rows × 25 columns</p>
</div>




```python
tolstoy['gender'].value_counts()
```




            107839
    муж      56781
    жен      33320
    сред     18063
    мж          96
    Name: gender, dtype: int64




```python
tolstoy[tolstoy['gender'] == 'мж']['lex'].value_counts().head(10)
```




    лаврушка       17
    браунау        11
    душенька        6
    судья           5
    моро            5
    бедняжка        5
    староста        3
    плакса          3
    сорвиголова     2
    бондаренко      2
    Name: lex, dtype: int64




```python
tolstoy['gender'].value_counts().plot.bar(color='purple'); # барплот сразу из датафрейма
plt.title('Gender')
plt.xlabel('gender')
plt.ylabel('number of entries');
```


![png](output_17_0.png)



```python
df2 = tolstoy[
    (tolstoy['gender'] != '') & (tolstoy['gender'] != 'мж')
][
    ['POS', 'gender', 'number']
].groupby(['POS', 'gender'], as_index=False).count()

df2.columns = ['POS', 'gender', 'total']
sns.barplot(x="POS", y="total", hue='gender', data=df2)
plt.title('Gender by POS')
plt.xlabel('POS')
plt.ylabel('number of entries');
```


![png](output_18_0.png)


#### Pie chart

Таким образом удобно визуализировать доли, которые занимают категории внтури целого, однако нужно аккуратно использовать этот вид графиков, потому что они могут ввести в заблуждение при небольшом количестве данных или при сравнении групп.



```python
plt.figure(figsize=(6, 6))
tolstoy['gender'].value_counts().plot(kind='pie');
plt.title('Gender');
```


![png](output_20_0.png)



```python
df2 = tolstoy[['lex', 'POS', 'gender']].groupby(['lex', 'POS'], as_index=False).count()
df2.columns = ['lex', 'POS', 'total']
df2 = df2[df2['total'] > 10]
```

#### Box plot

Боксплоты нужны для того, чтобы показать различия в распределениях в разныз группах, например, если у нас есть части речи и разные частоты лемм этих частей речи.



```python
plt.figure(figsize=(10, 6))
sns.boxplot(x="POS", y="total", data=df2)
plt.ylim((0, 1500))
plt.title('N of lemma entries by POS')
plt.ylabel('n entries')
plt.xlabel('POS');
```


![png](output_23_0.png)


#### Гистограмма

Главное отличие гистограммы от барплота - на гистограмме у нас одна переменная и мы хотим изучить только ее: сколько объектов с тем или иным значением (в промежуке значений), а барплот - это значения по категориям.



```python
df2['length'] = df2['lex'].apply(len)
plt.figure(figsize=(10, 6))
sns.distplot(df2['length'], bins=17, color='green')
plt.title('Distribution of lemma length')
plt.ylabel('%')
plt.xlabel('Length of word');
```

    C:\ProgramData\Anaconda3\lib\site-packages\matplotlib\axes\_axes.py:6462: UserWarning: The 'normed' kwarg is deprecated, and has been replaced by the 'density' kwarg.
      warnings.warn("The 'normed' kwarg is deprecated, and has been "
    


![png](output_25_1.png)


#### Word cloud

Один из видов визуализации — облако тегов, оно же облако слов. В питоне его можно сделать с помощью библиотеки `wordcloud`. Вот ее [документация с примерами](https://amueller.github.io/word_cloud/auto_examples/index.html). NB! Эту библиотеку нужно сначала установить,`pip install wordcloud`.


```python
from PIL import Image
import numpy as np
import matplotlib.pyplot as plt
from wordcloud import WordCloud

text = """Alice took up the fan and gloves and she kept fanning herself all the
time she went on talking. "Dear, dear! How queer everything is to-day!
And yesterday things went on just as usual. _Was_ I the same when I got
up this morning? But if I'm not the same, the next question is, 'Who in
the world am I?' Ah, _that's_ the great puzzle!'"""

cloud = WordCloud(background_color="white", max_words=2000)

# генерируем
cloud.generate(text)

# визуализируем
plt.imshow(cloud, interpolation='bilinear')
plt.axis("off")
plt.show()


# store to file
cloud.to_file("post_cloud.png")
```


![png](output_27_0.png)





    <wordcloud.wordcloud.WordCloud at 0xc368b70>




```python
# реальный размер картинки
Image.open("post_cloud.png")
```




![png](output_28_0.png)


