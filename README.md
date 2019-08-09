# RESTful Authentication with Flask


## Table of contents

- [About](#about)
- [How to run the application?](#how-to-run-the-application)
- [Using the API](#using-the-api)

## About

RESTful Authentication with Flask

This code uses the SQLite3 (https://www.sqlite.org/index.html) database to store users.

The user table has the following fields:

- **id:** unique identifier for users. Numeric type.
- **username:** username. String type.
- **password_hash:** password in hash format. String type.

The tasks resource use HTTP methods as follows:

| HTTP method |	URI | Example | Action |
| ----------- | --- | ------- | ------ |
| **POST** | http://[hostname]/api/users | ```curl -i -X POST -H "Content-Type: application/json" -d '{"username":"user1","password":"password"}' http://127.0.0.1:5000/api/users``` | User registration request |
| **GET** | http://[hostname]/api/resource | ```curl -u user1:password -i -X GET http://127.0.0.1:5000/api/resource``` | Get the protected resource for the user registered |
| **GET** | http://[hostname]/api/token | ```curl -u user1:password -i -X GET http://127.0.0.1:5000/api/token``` | Get an authentication token |
| **GET** | http://[hostname]/api/resource | ```curl -u eyJhbGciOiJIUzUxMiIsImlhdCI6MTU2NTM4NzE5MiwiZXhwIjoxNTY1Mzg3NzkyfQ.eyJpZCI6MX0.A6UWMpqmDQcplA1i9GDQCFD9slOyCIlFJC-6wn6GybTwyB2nJ9kgAamUr8d53rJEr0rR27oyxroo66AQufT2jA:unused -i -X GET http://127.0.0.1:5000/api/resource``` | Get resource with the token |


## How to run the application

1. Install [virtualenv](https://virtualenv.pypa.io/en/latest/) and [Flask](https://palletsprojects.com/p/flask/)
	* To activate the virtualenv on Linux or MacOS: ```source venv/bin/activate```
	* To activate the virtualenv on Windows: ```\venv\Script\activate.bat```

2. Run the application

Step-by-step:

```sh
cd restful_authentication_with_flask/

virtualenv venv

source venv/bin/activate

pip install -r requirements.txt

python app.py
```

**Note**: To desactivate the virtual environment

```sh
deactivate
```


## Using the API

### User registration

Here is an example user registration request sent from curl:

```
curl -i -X POST -H "Content-Type: application/json" -d '{"username":"user1","password":"password"}' http://127.0.0.1:5000/api/users
```

Output:

```
HTTP/1.0 201 CREATED
Content-Type: application/json
Content-Length: 26
Location: http://127.0.0.1:5000/api/users/1
Server: Werkzeug/0.15.5 Python/3.7.4
Date: Fri, 09 Aug 2019 21:45:04 GMT

{
  "username": "user1"
}
```

### Get the protected resource for the user registered

Here is an example curl request that gets the protected resource for the user registered above:

```
curl -u user1:password -i -X GET http://127.0.0.1:5000/api/resource
```

Output:

```
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 30
Server: Werkzeug/0.15.5 Python/3.7.4
Date: Fri, 09 Aug 2019 21:45:33 GMT

{
  "data": "Hello, user1!"
}
```

If an incorrect login is used, then this is what happens:

```
curl -u user1:wrong -i -X GET http://127.0.0.1:5000/api/resource
```

Output:

```
HTTP/1.0 401 UNAUTHORIZED
Content-Type: text/html; charset=utf-8
Content-Length: 19
WWW-Authenticate: Basic realm="Authentication Required"
Server: Werkzeug/0.15.5 Python/3.7.4
Date: Fri, 09 Aug 2019 21:46:03 GMT
```

### Get an authentication token

```
curl -u user1:password -i -X GET http://127.0.0.1:5000/api/token
```

Output:

```
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 203
Server: Werkzeug/0.15.5 Python/3.7.4
Date: Fri, 09 Aug 2019 21:46:32 GMT

{
  "duration": 600, 
  "token": "eyJhbGciOiJIUzUxMiIsImlhdCI6MTU2NTM4NzE5MiwiZXhwIjoxNTY1Mzg3NzkyfQ.eyJpZCI6MX0.A6UWMpqmDQcplA1i9GDQCFD9slOyCIlFJC-6wn6GybTwyB2nJ9kgAamUr8d53rJEr0rR27oyxroo66AQufT2jA"
}
```

### Get resource with the token

```
curl -u eyJhbGciOiJIUzUxMiIsImlhdCI6MTU2NTM4NzE5MiwiZXhwIjoxNTY1Mzg3NzkyfQ.eyJpZCI6MX0.A6UWMpqmDQcplA1i9GDQCFD9slOyCIlFJC-6wn6GybTwyB2nJ9kgAamUr8d53rJEr0rR27oyxroo66AQufT2jA:unused -i -X GET http://127.0.0.1:5000/api/resource
```

Output:

```
GET http://127.0.0.1:5000/api/resource
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 30
Server: Werkzeug/0.15.5 Python/3.7.4
Date: Fri, 09 Aug 2019 21:47:29 GMT

{
  "data": "Hello, user1!"
}
```

**Note that in this last request the password is written as the word unused. The password in this request can be anything, since it isn't used.**

