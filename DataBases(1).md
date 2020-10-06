
## Databases

**A relational database** organizes data into one or more tables (or "relations") of columns and rows, with a unique key identifying each row. A **row** of the table contains data about an object, e.g., a student; the **columns** of the table describe different properties of the corresponding object; they contain attributes, e.g., name, year, specialization. Each column describes a single property of the object and has a fixed datatype. All the rows have the same fields with different values for different objects. Here is a toy example:

|Name (string)| Specialization (string)| Year (integer)|
| ------------- |-------------:| -----:|
|Петя Иванов| филология| 1|
|Вася Петров| физика| 2|
|Маша Сидорова| биология| 3|

In a relational database each table has to have a **primary key** — a field or a combination of fields that uniquely identify each row of the table.

*Relational tables can be linked with each other*, which means that the data can be extracted from multiple tables. The tables are linked with each other to minimize the size of a database.

There are three types of relationships:
* one-to-one
* one-to-many
* many-to-many

The **one-to-one** correspondence presupposes that each attribute of the first table corresponds to a single attribute of the second table and vice versa. The **one-to-many** correspondence presupposes that one attribute of the first table corresponds to several attributes of the second table. The **many-to-many** correspondence presupposes that one attribute of the first table corresponds to several attributes of the second table and vice versa.

To work with a database you will need to use a special program, a **Database Management System**, or **DBMS**. Below I list some DBMSs:

* SQLite
* MySQL
* PostgreSQL
* MongoDB
* ...

## SQL

Some of the database management systems have SQL in their names. What does SQL stand for?

**SQL** *(Structured Query Language)* is a standard language for accessing and manipulating databases.  SQL can execute queries against a database, can retrieve data from a database, can insert records in a database, can update records in a database, can delete records from a database, can create new databases etc.

SQL is a very simple language. We will need a small number of commands for the operations with the data (CREATE, DELETE, DROP, SELECT, INSERT, UPDATE) and commands-restrictors for the formulation of more specific queries (WHERE, IN, AND, OR, NOT, BETWEEN, LIKE, LIMIT, OFFSET). Note that in a query, the word order is fixed: first comes "what", then "where", then "how".

You have already worked through the interactive tutorial SQLBolt.

SELECT exercises

* https://sqlbolt.com/lesson/select_queries_introduction - основы (уроки 1-5, 8)
* https://sqlbolt.com/lesson/select_queries_with_joins - выбор данных из нескольких таблиц (уроки 6-7)
* https://sqlbolt.com/lesson/select_queries_with_expressions - работа с математическими выражениями


DML exercises

* https://sqlbolt.com/lesson/inserting_rows
* https://sqlbolt.com/lesson/updating_rows
* https://sqlbolt.com/lesson/deleting_rows

Creating and deleting tables

* https://sqlbolt.com/lesson/creating_tables (уроки 16-18)


## Programs to use when working with databases

Database is not a text format, you cannot open it with a text editor and examine its contents. To do so, you need to use a special program designed to work with databases. Below I list some of the options:

* [MySQL](https://www.mysql.com/)
* [PostgreSQL](https://www.postgresql.org/)
* [MongoDB](https://www.mongodb.com/)
* [Firebird](https://firebirdsql.org/)

[Here](https://blog.capterra.com/free-database-software/) you can find a discussion of the pros and cons of the above options.

**DBrowser**: https://sqlitebrowser.org/dl/


## Databases and Python

To work with databases in python we will be using the [SQLite](https://docs.python.org/3.5/library/sqlite3.html) library. The database files have the `.db` extension.


```python
import sqlite3

# connecting to the database
conn = sqlite3.connect('example.db')

# creating a cursor object we will be sending our queries to
c = conn.cursor()

# creating a table
c.execute("CREATE TABLE IF NOT EXISTS students(name text, major text, year integer)")

# inserting a line
c.execute("INSERT INTO students VALUES ('Петя Иванов','филология',1), ('Маша Петрова','история',4)")

# commiting the changes
conn.commit()

# closing the database
#conn.close()
```

**Important**: don't forget to create a cursor object, without it you won't be able to send any queries to the database!

**Important**: if you changed anything, don't forget to commit the changes, they won't be saved otherwise!

**Important**: in your queries don't use string concatenation or formatting that you use in python. This will make your database vulnerable to SQL-injections, code injection techniques, used to attack data-driven applications, in which malicious SQL statements are inserted into an entry field for execution (e.g. to dump the database contents to the attacker). You can read more about it [here](https://habrahabr.ru/post/148151/) or [here](https://en.wikipedia.org/wiki/SQL_injection).

![](https://imgs.xkcd.com/comics/exploits_of_a_mom.png)


```python
# Don't do this!
name = 'Петя'
c.execute("SELECT * FROM students WHERE name = '%s'" % name)

# This is the right way
x = ('Петя Иванов',)
c.execute('SELECT * FROM students WHERE name=?', x)
print(c.fetchone())
```

    ('Петя Иванов', 'филология', 1)
    


```python
# If the expected result of the query is a number of lines, you can iterate through them

for row in c.execute('SELECT * FROM students ORDER BY year'):
    print(row)
```

    ('Петя Иванов', 'филология', 1)
    ('Маша Петрова', 'история', 4)
    


```python
# to insert several variables into the sql-query

x = 'Вася Пупкин'
y = 'математика'
z = 3

c.execute('INSERT INTO students VALUES (?, ?, ?)', (x, y, z))
conn.commit()
```


```python
for row in c.execute('SELECT * FROM students ORDER BY year'):
    print(row)
```

    ('Петя Иванов', 'филология', 1)
    ('Вася Пупкин', 'математика', 3)
    ('Маша Петрова', 'история', 4)
    

#### Formatting

If you want to insert variables as your rows, use string formatting.


```python
params = ['vowel', 'f1', 'f2']
c.execute('CREATE TABLE vowels({}, {}, {})'.format(params[0], params[1], params[2]))
```




    <sqlite3.Cursor at 0x4dd4730>




```python
# как написать длинный запрос посимпатичнее
c.execute('''
INSERT INTO vowels 
VALUES 
('a', 1234.56, 4567.8), 
('u', 1111.1, 3333.3)'''
)
```




    <sqlite3.Cursor at 0x4dd4730>




```python
for row in c.execute('SELECT * FROM vowels'):
    print(row)
```

    ('a', 1234.56, 4567.8)
    ('u', 1111.1, 3333.3)
    

#### Cursor functions

* **fetchone()** -- fetches the next row of a query result set, returning a single sequence, or None when no more data is available
* **fetchall()** -- fetches all (remaining) rows of a query result, returning a list. An empty list is returned when no rows are available
* **fetchmany()** -- fetches the next set of rows of a query result, returning a list. An empty list is returned when no more rows are available. The number of rows to fetch per call is specified by the size parameter. If it is not given, the cursor’s arraysize determines the number of rows to be fetched. The method should try to fetch as many rows as indicated by the size parameter. If this is not possible due to the specified number of rows not being available, fewer rows may be returned.


```python
# fetching rows one by one
# note that in each query fetchone returns the next row
c.execute('SELECT * FROM students ORDER BY year')
print(c.fetchone())
print(c.fetchone())
print(c.fetchone())
```

    ('Петя Иванов', 'филология', 1)
    ('Вася Пупкин', 'математика', 3)
    ('Маша Петрова', 'история', 4)
    


```python
# fetching two rows
c.execute('SELECT * FROM students ORDER BY year')
print(c.fetchmany(2))
```

    [('Петя Иванов', 'филология', 1), ('Вася Пупкин', 'математика', 3)]
    


```python
# fetching all the rows
c.execute('SELECT * FROM students ORDER BY year')
print(c.fetchall())
```

    [('Петя Иванов', 'филология', 1), ('Вася Пупкин', 'математика', 3), ('Маша Петрова', 'история', 4)]
    

### Exercise 1

The [nanai-vowels.csv](https://github.com/dashapopova/Preprocessing2019/blob/master/nanai-vowels.csv) contains observations about nanai vowels. 7 speakers from two villages were recorded pronouncing different words that had the i, ɪ (encoded as I in the file) and ə (encoded as e in the file) vowels. The file contains information about the first and the second formants. Let's create a database out of the file using `sqlite3` and print the observations where village='Dzhuen', sex='f', sound='i'.

A solution -- https://github.com/dashapopova/Preprocessing2019/blob/master/dbexample(1).ipynb

### Exercise 2

Let's work with `rutul_vowels.csv`


```python
import pandas as pd
import csv
import sqlite3
```


```python
df = pd.read_csv('rutul_vowels.csv', sep=',', encoding='utf-8')
```


```python
df.head()
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
      <th>id</th>
      <th>word</th>
      <th>translation</th>
      <th>vowel</th>
      <th>stress</th>
      <th>syllable</th>
      <th>left_context</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>χed</td>
      <td>дикая алыча</td>
      <td>e</td>
      <td>yes</td>
      <td>cvc</td>
      <td>χ</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>eked</td>
      <td>старший</td>
      <td>e</td>
      <td>no</td>
      <td>cv</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>ɢina</td>
      <td>в Кинe</td>
      <td>i</td>
      <td>yes</td>
      <td>cv</td>
      <td>ɢ</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>χuda</td>
      <td>в кулаке</td>
      <td>u</td>
      <td>no</td>
      <td>cv</td>
      <td>χ</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>ʁuli</td>
      <td>в окне</td>
      <td>u</td>
      <td>no</td>
      <td>cv</td>
      <td>ʁ</td>
    </tr>
  </tbody>
</table>
</div>




```python
con = sqlite3.connect("rutul.db")
cur = con.cursor()
```

A more efficient way to do the same: 


```python
df.to_sql(name='rutul', con=con, if_exists='replace')
```

Let's check that what we have done worked:


```python
for row in cur.execute('SELECT * FROM rutul'):
    print(row)
```

    (0, 1, 'χed', 'дикая алыча', 'e', 'yes', 'cvc', 'χ')
    (1, 2, 'eked', 'старший', 'e', 'no', 'cv', 'no')
    (2, 3, 'ɢina', 'в Кинe', 'i', 'yes', 'cv', 'ɢ')
    (3, 4, 'χuda', 'в кулаке', 'u', 'no', 'cv', 'χ')
    (4, 5, 'ʁuli', 'в окне', 'u', 'no', 'cv', 'ʁ')
    (5, 6, 'ɢuje', 'в яме', 'u', 'yes', 'cv', 'ɢ')
    (6, 7, 'qaka', 'отдай назад', 'a', 'yes', 'cv', 'q')
    (7, 8, 'qiq’a', 'возвращайся', 'i', 'yes', 'cv', 'q')
    (8, 9, 'χɨd', 'липа', 'ɨ', 'yes', 'cvc', 'χ')
    (9, 10, 'ʁɨˁbar', 'лягушки', 'ɨˁ', 'no', 'cv', 'ʁ')
    (10, 11, 'χaˁrad', 'масло', 'aˁ', 'no', 'cv', 'χ')
    (11, 12, 'itɨd', 'медовый', 'i', 'yes', 'cv', 'no')
    (12, 13, 'uˁbra', 'мерка для муки', 'uˁ', 'no', 'cv', 'no')
    


```python
import sqlite3
import csv
con = sqlite3.connect("rutul3.db")
cur = con.cursor()
cur.execute("DROP TABLE IF EXISTS rutul3")
cur.execute("CREATE TABLE rutul3 (vowel, translation, syllable);")  # all the columns names

with open('rutul_vowels.csv','r', encoding='utf-8') as fin:
    dr = csv.DictReader(fin)  # a comma is the default separator
    to_db = [(i['vowel'], i['translation'], i['syllable']) for i in dr]  # put the columns' names again

cur.executemany("INSERT INTO rutul3 (vowel,translation, syllable) VALUES (?, ?, ?);", to_db)  # and again, the names of the columns

for i in cur.execute("SELECT * FROM rutul3"):
    print(i)
    
con.commit()
con.close()
```

    ('e', 'дикая алыча', 'cvc')
    ('e', 'старший', 'cv')
    ('i', 'в Кинe', 'cv')
    ('u', 'в кулаке', 'cv')
    ('u', 'в окне', 'cv')
    ('u', 'в яме', 'cv')
    ('a', 'отдай назад', 'cv')
    ('i', 'возвращайся', 'cv')
    ('ɨ', 'липа', 'cvc')
    ('ɨˁ', 'лягушки', 'cv')
    ('aˁ', 'масло', 'cv')
    ('i', 'медовый', 'cv')
    ('uˁ', 'мерка для муки', 'cv')
    


```python
import sqlite3

with open('rutul_vowels.csv', 'r', encoding='utf-8') as f:
    f = f.readlines()

# connecting to the database
conn = sqlite3.connect('rutul2.db')

# creating a cursor
c = conn.cursor()

# creating a table
c.execute("DROP TABLE IF EXISTS rutul")
c.execute("CREATE TABLE IF NOT EXISTS rutul(id,word,translation,vowel,stress,syllable)")

for row in f:
    row = row.split(',')
    c.execute("INSERT INTO rutul VALUES (?, ?, ?, ?, ?, ?)", (row[0], row[1], row[2], row[3], row[4], row[5]))

# saving
conn.commit()

for i in c.execute("SELECT * FROM rutul WHERE vowel='ɨˁ' AND stress='no' AND syllable='cv'"):
    print(i)

# closing
conn.close()
```

    ('10', 'ʁɨˁbar', 'лягушки', 'ɨˁ', 'no', 'cv')
    

1. Print all the data, where the vowel is unstressed.
2. Print all the data, where the vowel is unstressed and there is no left context.


```python
c.execute('SELECT * FROM rutul WHERE stress="no"')
print(c.fetchall())
```

    [('2', 'eked', 'старший', 'e', 'no', 'cv'), ('4', 'χuda', 'в кулаке', 'u', 'no', 'cv'), ('5', 'ʁuli', 'в окне', 'u', 'no', 'cv'), ('10', 'ʁɨˁbar', 'лягушки', 'ɨˁ', 'no', 'cv'), ('11', 'χaˁrad', 'масло', 'aˁ', 'no', 'cv'), ('13', 'uˁbra', 'мерка для муки', 'uˁ', 'no', 'cv')]
    


```python
import sqlite3

with open('rutul_vowels.csv', 'r', encoding='utf-8') as f:
    f = f.readlines()

# connecting to the database
conn = sqlite3.connect('rutul2.db')

# creating a cursor
c = conn.cursor()

# creating a table
c.execute("DROP TABLE IF EXISTS rutul")
c.execute("CREATE TABLE IF NOT EXISTS rutul(id,word,translation,vowel,stress,syllable, left_context)")

for row in f:
    row = row.split(',')
    c.execute("INSERT INTO rutul VALUES (?, ?, ?, ?, ?, ?, ?)", (row[0], row[1], row[2], row[3], row[4], row[5], row[6]))

# saving
conn.commit()

c.execute('SELECT * FROM rutul WHERE stress="no" AND left_context="no"')
print(c.fetchall())

# closing
conn.close()
```

    [('13', 'uˁbra', 'мерка для муки', 'uˁ', 'no', 'cv', 'no')]
    


```python
# Don't forget!
conn.close()
```
