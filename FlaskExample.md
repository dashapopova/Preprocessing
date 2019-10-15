#### Flask application example

Let's work through a simple flask application.

A .py file:

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
