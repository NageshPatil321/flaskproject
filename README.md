Project Name : Flask Project
Created By : Mr. Nagesh Patil

==========================================================================================================================================================

1 ) Steps to set up and deploy the application.

a.	Create a simple Python Flask application that returns "Hello World!".

•	Create Virtual Environment:
    Install virtualenv using pip command
    $ pip3 install virtualenv
    $ virtualenv env
    $ source env/bin/activate
    •	Set up the Flask application to run with Gunicorn as the WSGI server.
    Install gunicorn & flask: 
    $ pip install gunicorn
    $ pip install flask
    •	Create flask application file app.py.
    app.py:
    from flask import Flask
    app = Flask(__name__)
    @app.route("/")
    def index():
        return "Hello World"
    if __name__ == "__main__":
        app.run(host='0.0.0.0', port=8080)

b.	Set up the Flask application to run with Gunicorn as the WSGI server.

•	Create WSGI configuration for gunicorn interact with application.
    wsgi.py:

    from app import app
    if __name__ == "__main__":
        app.run()

•	Once wsgi file is ready then we are serving our application using below command.
    $ gunicorn --bind 0.0.0.0:5000 wsgi:app

    ![image](https://github.com/NageshPatil321/flaskproject/assets/63147214/908ec81e-2ae0-4f65-b57c-0e7dc879de21)
