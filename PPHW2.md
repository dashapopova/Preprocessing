## Linguistic Questionnaire/Experiment

Create your own flask application with a linguistic questionnaire or experiment. You determine the theme of the questionnaire or experiment.
It could be a lexical typological questionnaire, a sociolinguistic questionnaire, a questionnaire for a fieldwork project (for a dictionary, for collecting judgements) etc.

## Points Calculator

|Points|Task|
|----|--------|
|1|The app is working|
|2|The main page (127.0.0.1) with a linguistic questionnaire/experiment is created|
|1|The answers are saved to a .csv file|
|2|There is a statistics page (127.0.0.1/stats) that displays the distribution of the answers (as a table, as a summary of some statistical analysis, as a plot -- it is up to you), the page is linked to the search page, the stats page and the main page|
|2|There is a search page (127.0.0.1/search) that allows the user to search for the data they need, the page is linked to the search page, the stats page and the main page|
|2|There is a results page (127.0.0.1/results) that displays the results of the search, the page is linked to the search page, the stats page and the main page|
|**2**|**Bonus points:** the answers are saved in a database (not in a .csv file)|
|**2**|**Bonus points:** styling with [bootstrap](https://www.w3schools.com/booTsTrap/default.asp)|
|**2**|**Bonus points:** the experiment/questionnaire includes pictures or audiofiles or videofiles|
|**2**|**Bonus points:** the app is uploaded to pythonanywhere or heroku (send me the link if you do this)|

**NB!** Send me 1) your app in a `.py` or a `.ipynb` file; 2) a folder `templates` with your html-files; 3) optionally: a folder `static` with your .css files, pictures. 

### How do I insert a picture?

Pictures should be put into the folder `static`. In the html-template, include this line:

   `<img src="{{url_for('static', filename='image_name.jpg')}}" />`
   
To insert a picture into html-code (without flask), write this:
   `<img src="image_name.jpg" />`
