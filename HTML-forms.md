## HTML Forms

To create an HTML form you need to use the `<form>` tag. The HTML `<form>` element defines a form that is used to collect user input. The tag `<form>` has the following attributes: 

* action - specifies the program we send our request to
* method - the method we use - GET|POST

The tag `<input>` is used to specify the areas for the user input. The `<input>` tag has the following attributes:

* name - the name of the field,
* value - the value of the field,
* type - the type of input:
    * text - a small text field
    * textarea - a big text field
    * checkbox - checkbox buttons
    * radio - radio buttons (only one of those can be selected)
    * submit - the submit to the server button

#### Example 1
```
<form method="GET">
    Как вас зовут? <input type="text" name="username" value="">
    <input type="submit" value="Отправить">
</form>
```

#### Example 2
```
<html>
    <head>
       <meta charset="utf-8">
       <title>Кого вы больше любите?</title>
    </head>
 <body>

 <form action="">
    <p><b>Как вас зовут?</b> <input type="text" name="username" value=""></p>
    <p><b>Кого вы больше любите?</b></p>
    <input type="radio" name="answer" value="cat">Котиков<br>
    <input type="radio" name="answer" value="dog">Собачек<br>
    <input type="radio" name="answer" value="bird">Птичек<br>
    <input type="radio" name="answer" value="elephant">Слоников<br>
    <p><input type="submit"></p>
 </form>

 </body>
 </html>
```
#### Example 3
```
<html>
    <head>
        <meta charset="utf-8">
        <title>Чем вы любите заниматься</title>
    </head>
    <body>

    <form action="">
        <p><b>Как вас зовут?</b> <input type="text" name="username" value=""></p>
        <p><b>Сколько вам лет?</b> <input type="text" name="age" value=""></p>
        <p><b>Чем вы любите заниматься?</b></p>
        <input type="checkbox" name="hobby" value="cat">Спать<br>
        <input type="checkbox" name="hobby" value="book">Читать книжки<br>
        <input type="checkbox" name="hobby" value="prog">Программировать<br>
        <input type="checkbox" name="hobby" value="game">Гамать<br>
        <input type="checkbox" name="hobby" value="write">Писать научные статьи<br>
        <input type="checkbox" name="hobby" value="exper">Придумывать лингвистические эксперименты<Br>
        <input type="checkbox" name="hobby" value="food">Кушать вкусняшки<br>
        <p><input type="submit"></p>
    </form>
    </body>
</html>
```
#### Exercise

Let's write a page that contains a form that allows the user to input a word to search for it using google.
```
<html>
    <head>
       <meta charset="utf-8">
       <title>Что ищем?</title>
    </head>
    <body>
       <form action="https://www.google.com/search">
          <p><b>Введите поисковый запрос</b></p>
          <input type="text" name="q">
          <p><input type="submit"></p>
       </form>
     </body>
 </html>
```
#### Useful links

More about HTML Forms: https://www.w3schools.com/html/html_forms.asp

Percent encoding/decoding: https://www.url-encode-decode.com/
