## Get stated with Python [Flask](http://flask.pocoo.org/) Frame work on IBM Bluemix

This work is based on [Getting started with Python on Bluemix Tutorial](https://github.com/IBM-Bluemix/get-started-python)

The tutorial will develop a Python web application based on Flask framwork and deploy the app on [IBM Bluemix](http://bluemix.net). The app shows up a basic form which users can type their names. When hit enter the name will be add to database and can be access at `http://base_url/api/visitors`  

### 1. Requirement

- [Git](https://git-scm.com/downloads)
- [Cf](https://console.ng.bluemix.net/docs/starters/install_cli.html)
- [Python](https://www.python.org/downloads/)

### 2. Clone example code and run locally
- Rename your app
  Open up `manifest.yml`, similar like:
  
  ```
    applications:
        - name: Lecheebot
        random-route: true
        memory: 128M

  ```
- Run your app locally
  Ensure that you have installed `pip` in your PC. If not, follow this post: [pip and setuptool on windows](http://flask.pocoo.org/docs/0.12/installation/#windows-easy-install)

  now, you can run your app locally by typing the command:

  `pip install -r requirements.txt`

  `python hello.py`

  Your app is now accessible at: [http://localhost:8080](http://locallhost:8080)

### 3. Deploy to IBM Bluemix using Cloud Foudry CLI

Ensure you have [IBM Bluemix](http://bluemix.net) accout and installed CF CLI on your machine. if not, follow the steps: [[https://github.com/cloudfoundry/cli#downloads](https://github.com/cloudfoundry/cli#downloads)

- Log in to CF CLI

Run CMD as administrator, then login to CF with the commands:

`cf api https://api.ng.bluemix.net`

`cf login`

Enter your email and password.

- Deploy your app

Open CMD at your app reposit that you cloned in the step 2.
Execute the command to deploy your app:

`cf push`

You can view your app status by the command:

`cf apps`

### 4. Add Database

* To connect to Cloudant noSQL DB, follow these steps:

- [Login Bluemix accout](https://console.ng.bluemix.net/)
- Navigate to your app. Click on "Connect new" button on Connections Group. Choose Cloudant NoSQL DB service on the categories. Finally click Create button.
- Ensure that you connect Cloudant NoSQL DB service to your application. If not, click on the "Connect existing" to connect with service.

You can test your app at your app url.

* To connect Database for working locally:

- Create a file with th name `vcap-local.json`
- Add the following detail:

```
{
 "services": {
   "cloudantNoSQLDB": [
     {
       "credentials": {
        "username": "<your_username>",
        "password": "<your_password>",
        "host": "your_host",
        "port": 443,
        "url": "yoururl"

       },
       "label": "cloudantNoSQLDB"
     }
   ]
 }
}
```

You can find username, password, host by click on Cloudant NoSQL DB service at your IBM Blumix console. Then click on "Service Credentials". Copy and pass information to your `vcap-local.json` file.

![](/img/cloudant_credential.png)

Now you can test your app locally with the database:

`python hello.py`
