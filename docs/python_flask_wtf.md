## Web Form

### install Flask WTF

`pip install Flask-wtf`

### Cross-site request forgery (CSRF) protection

In `hello.py`:

```
app = Flask(__name__)
app.config['SECRET_KEY']='secretkey'
```
