### For locally:

`$ pip install flask-moment`

### For Bluemix: 
In `requirements.txt` add:

`Flask-moment==0.5.1`

In `hello.py`: Initialize Flask-Moment  

```
from flask_moment import Moment
moment= Moment(app)  
```

Add `moment.js` into base template base.html
In `templates/base.html`: Import `moment.js` library
```
{% block scripts %}
{{ super() }}
{{ moment.include_moment() }}
{% endblock %}  
```
To work with timestamps Flask-Moment makes a moment class available to templates. In hello.py passes a variable called current_time to the template for rendering. 
In `hello.py`: Add a datetime variable.
```
from datetime import datetime
@app.route('/')
def index():
return render_template('index.html', current_time=datetime.utcnow())
```
Render current_time in the template.
In `templates/index.html`: Timestamp rendering with Flask-Moment
```
<p>The local date and time is {{ moment(current_time).format('LLL') }}.</p>
<p>That was {{ moment(current_time).fromNow(refresh=True) }}</p>  
```
The problem is that the can not be refresh because you render the index page. the current_time variable is defined in server, not in the browser. Another way to load local date and time and refresh it every second, is to use script snippet:
In the "index.html":

```
<h3>Local time:<span id="timestamp"></span></h3>

<!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
<!-- Include all compiled plugins (below), or include individual files as needed -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<!-- the script for refresh current time -->
<script>
var update = function () {
// using jQuery (really not a must):
$("#timestamp").html(moment().format('MMMM Do YYYY, h:mm:ss a'));
// or without jquery
document.getElementById("timestamp").innerHTML = moment().format('MMMM Do YYYY, h:mm:ss a');
};

$(document).ready(function(){
setInterval(update, 100);
});
</script>

```