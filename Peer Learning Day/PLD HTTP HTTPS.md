## What is a RESTful API and what does REST stand for?

RESTful API is the standard way a http server comunicate with an api a restul api exposes path for requesting data or operations REST stand for Representational State Transfer

## What is the main difference between HTTP and HTTPS?

HTTP is the standard web protocol it doesn't come with encryption HTTP stand for hypertext transfer protocol it uses port 80 by default
HTTPS uses HTTP + TLS for encryption also TLS stand for Transport Layer Security it uses standard encryption like SHA256 it uses port 443 by default

## What does the “S” in HTTPS represent and why is it important?

Secure it's import because if someone is connected to the same network as you they can intercept and read the content of the http package so every information gets exposed

## What are the most common HTTP methods and what are they used for?

GET method usually used for requesting data
POST method used for creating data generaly a new user
PUT used for updating data from an already existing data
PATCH used for patching data
DELETE used for deleting values

## What is an HTTP status code and how is it structured?

and http status code usually define how the request went

1xx mean continue
2xx mean success
4xx mean failure
3xx mean redirection
5xx internal error

## How to inspect HTTP requests and responses using your browser’s developer tools?

you can inspect http request in the network tab when you inspect a webpage in your browser

## What happens when a client sends an HTTP request to a server?

a webserver receives it and provides a webservice internal communication can ensure that a proper delivery happen so a webserver can communicate with an internal API

## What is the purpose of the curl command?

the curl command is used to make http request you can send and receive data using curl

## How to perform a GET request with curl?

```bash
curl -X GET http://omegalul
curl http://kek
```

## How to send data with a POST request using curl?

```bash
curl -X POST -H "header content" -d "data to send" http://goodluck.com/myservice
```

## What is the difference between curl -I and curl -X POST?

curl -I just fetch the header of the http request
curl -X POST on the other hand needs more info or you'll get a bad request also you use a POST reqquest used -H to provide a header -F to provide a File and -d to provide a string

## what Python library is commonly used to interact with REST APIs?

the request module is used to interact with REST APIs

## How to parse JSON data in Python using the requests library?

```python
requests.json()
```

## How to write fetched API data into a CSV file in Python?

```python
request.get(url)
data = 
```

## What is the purpose of Python’s http.server module?

the http.server module is meant to be  used to learn basic http server and how they work it shouldn't be used for anything serious we'd prefer apache or nginx

## How to start a simple HTTP server in Python?

```python
from http.server import BasicHttpRequesterHandler, HTTPServer

class serv(BasicHttpRequesterHandler):
	pass
	
my_server = HTTPServer(("localhost", 8000), serv)
my_server.server_forever()
```

## What is Flask and why is it useful for building APIs?

Flask is a python module used to create api it can handle routing automatically as well as authentification can deal with different request types

## How to define a route in Flask using the @app.route() decorator?

with the @app.route decorator
```python
@app.route("/")
def index():
	return "some random stuff", 200
```

## what is the role of the jsonify() function in Flask?

jsonify transform a python object into json very useful for returning datas

## How to handle POST requests and access data sent by the client in Flask?

```python
@app.route("/", methods=["POST"])
def index_post():
	username = request.form["username"]
	password = request.form["password"]
	return username
```
for json data
```python
request.get_json()
```

## What is the difference between authentication and authorization?

authentication is when you want to autheticate a user you need information
authoorization is who is allowed to reach a certain webpage or service they generaly go with authetication as an authenticated user can have access to some services that are locked behind authorization

## What is Basic Authentication and how does it work?

a basic authetication works with a username and password combo you give it to the api and it returns success or failure if the information about the user name are valid it generalize use hashing to store password and validate them

## What is a JSON Web Token (JWT) and what is it used for?

a json web token is used for sessions and authetication usage it avoid the repeated sending of password and username information as well as allow for sessioning as you can easily create a token on authentication with limited times, it's also more secure since stealing a token while can be dangerous generaly won't cause huge issues because it will be invalid after a certain amount of time generaly between 5 and 15 minutes you can also use cookies for persistent connection usually rikier tho because someone that has access to a client token can use them indefinitely

## What is OpenAPI and why is it important for documenting APIs?

it's a framework for doculenting and developping an API it's important because it's standard and anyone can understand it
https://www.openapis.org/what-is-openapi
## What is Swagger UI and how does it help developers interact with an API?

