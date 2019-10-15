#### Flask application example

Let's work through a simple flask application.

A .py file with a toy questionnaire about the language use. How many pages does the application have? Where do we save the results of the survey?

```
from flask import Flask, render_template, request
import csv
import json

app = Flask(__name__)
filename = 'results.csv'


@app.route('/')
def main_page():
    return render_template("index.html")


@app.route('/thanks', methods=['POST'])
def save_to_csv():
    if request.method == 'POST':
        ask = request.form['ask']
        work = request.form['work']
        home = request.form['home']
        name = request.form['name']
        surname = request.form['surname']
        fin_form = 'Спасибо за ваш ответ!'
        fieldnames = ['ask', 'work', 'home', 'name', 'surname']
        with open(filename, 'a+', encoding='utf-8') as csvfile:
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
            writer.writerow({'ask': ask, 'work': work, 'home': home,
                             'name': name, 'surname': surname})
        return render_template("thanks.html", fin_form=fin_form)


@app.route('/search')
def do_search():
    return render_template("search.html")


@app.route('/result', methods=['POST'])
def show_result():
    dict_csv = {}
    if request.method == 'POST':
        ask = request.form['ask_search']
        what = request.form['what_search']
        fieldnames = ['ask', 'work', 'home', 'name', 'surname']
        with open(filename, 'r', encoding='utf-8') as csvfile:
            reader = csv.DictReader(csvfile, fieldnames=fieldnames)
            for row in reader:
                if row['ask'] == ask:
                    if (row['work'] == what or row['home'] == what or
                            row['name'] == what or row['surname'] == what):
                        name = row['name'] + ' ' + row['surname']
                        dict_csv[name] = json.loads(json.dumps(row))
        return render_template("result.html", result=dict_csv)


if __name__ == '__main__':
    app.run(debug=True)

```

Into the folder `templates` we should put the following files:

* index.html

```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="utf-8">
    <title>Анкета владения языками хантов Казыма</title>
</head>
<body>
<form action="http://127.0.0.1:5000/thanks" method = 'POST'>
    <fieldset>
        <legend><b>Опрос про распределение языков общения у носителей хантыйского</b></legend>
    <p>Пожалуйста, заполняйте таблицу на английском языке для удобства последующих операций с ней</p>
    Владеете ли Вы хантыйским как родным?<br>
    <input type="radio" name="ask" value="yes" checked>Да<br>
    <input type="radio" name="ask" value="no">Нет<br>
    На каком языке Вы общаетесь на работе?<br>
        Например: Khanty, Komi-Zyrian, Mansi, Russian<br>
    <input type="text" name="work" required pattern='[a-zA-Z]+'><br>
    На каком языке Вы общаетесь дома?<br>
        Например: Khanty, Russian, Mansi, Komi-Zyrian<br>
    <input type="text" name="home" required pattern='[a-zA-Z]+'><br>
        Ваше имя?<br>
        Например: Dasha<br>
    <input type="text" name="name" required pattern='[a-zA-Z]+'><br>
        Ваша фамилия?<br>
        Например: Popova<br>
    <input type="text" name="surname" required pattern='[a-zA-Z]+'><br>
    <p><input type="submit" value="Отправить"></p>

    </fieldset>
</form>
<p><a href='/search'>Поиск по результатам</a></p>
</body>
</html>
```
* result.html

```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Результат поиска</title>
</head>
<body>
<p>{{result}}</p>
<p><a href='/'>Главная страница</a></p>
<p><a href='/search'>Поиск по результатам</a></p>
</body>
</html>
```
* search.html

```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Поиск по ответам</title>
</head>
<body>
<form action="http://127.0.0.1:5000/result" method = 'POST'>
    <fieldset>
        <legend><b>Поиск ответов по ключевым словам</b></legend>
        Человек ответил да или нет на вопрос, хант ли он?<br>
        <input type="radio" name="ask_search" value="yes" checked>Да<br>
        <input type="radio" name="ask_search" value="no">Нет<br>
        Введите поисковый запрос на английском языке.<br>
        <input type="text" name="what_search" required pattern='[a-zA-Z]+'><br>
        <p><input type="submit" value="Искать"></p>
<p><a href='/'>Главная страница</a></p>
</body>
</html>
```

* thanks.html
```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Благодарность</title>
</head>
<body>
<h1>{{fin_form}}</h1>
<p><a href='/'>Главная страница</a></p>
<p><a href='/search'>Поиск по результатам</a></p>
</body>
</html>
```

If you want to include the styling, create a folder `static` and put the .css files into that folder. Link your .css files to the corresponding .html files. 

For example, we want to make the thank you part yellow. We create a style.css file and put it into a static folder:

```
h1{
	color:yellow;

}
```

The style.css file: color all `<h1>` elements with yellow.

Then, in the thanks.html file, we include the link to the style.css file: `<link rel= "stylesheet" type= "text/css" href= "{{ url_for('static',filename='style.css') }}">`:

* thanks.html

```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <link rel= "stylesheet" type= "text/css" href= "{{ url_for('static',filename='style.css') }}">
    <title>Благодарность</title>
</head>
<body>
<h1>{{fin_form}}</h1>
<p><a href='/'>Главная страница</a></p>
<p><a href='/search'>Поиск по результатам</a></p>
</body>
</html>
```
