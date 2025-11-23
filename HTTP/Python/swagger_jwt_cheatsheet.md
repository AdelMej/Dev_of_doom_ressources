# 🛡️ Flask-RestX + Swagger JWT Auth Cheat Sheet

## 🔧 1. Define Swagger Authorizations

``` python
authorizations = {
    'BearerAuth': {
        'type': 'apiKey',
        'in': 'header',
        'name': 'Authorization',
        'description': "Use: Bearer <your_token>"
    }
}
```

## 🚀 2. Attach Authorizations to the API

``` python
api = Api(
    app,
    version='1.0',
    title='HBnB API',
    description='HBnB Application API',
    doc='/api/v1/',
    authorizations=authorizations,
    security='BearerAuth'
)
```

## 🔒 3. Secure a Specific Endpoint

``` python
@api.doc(security='BearerAuth')
@api.route('/places')
class PlaceList(Resource):
    def get(self):
        return []
```

## 📌 4. Swagger Will Automatically Send

    Authorization: Bearer <your_token_here>

## 🔍 5. Extract the JWT in Flask

``` python
from flask import request
import jwt

def get_jwt_identity():
    auth = request.headers.get("Authorization", None)
    if not auth:
        raise Exception("Missing token")

    parts = auth.split()

    if parts[0].lower() != "bearer" or len(parts) != 2:
        raise Exception("Invalid Authorization header")

    token = parts[1]

    return jwt.decode(token, "YOUR_SECRET", algorithms=["HS256"])
```

## 🧪 6. Using Swagger "Authorize" Button

1.  Click **Authorize**
2.  Select **BearerAuth**
3.  Enter:\
    `Bearer <your_token>`
4.  Hit **Authorize**
5.  Done --- token is added to all secured requests.

## ⚡ TL;DR Mini-Snippet

``` python
authorizations = {'BearerAuth': {'type':'apiKey','in':'header','name':'Authorization'}}
api = Api(app, authorizations=authorizations, security='BearerAuth')
@api.doc(security='BearerAuth')
```
