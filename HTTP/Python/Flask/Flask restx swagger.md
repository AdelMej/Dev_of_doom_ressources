# 🧠 Swagger / OpenAPI Cheat Sheet for Flask-RESTX

---

## 🧱 Basic Setup

```python
from flask import Flask
from flask_restx import Api, Resource, fields

app = Flask(__name__)
api = Api(app,
          title="My Awesome API",
          version="1.0",
          description="Flask-RESTX Swagger Example",
          doc="/docs")  # Swagger UI path (default is /)
```
---

## 📦 Define Models (Shown in Swagger)

```python
user_model = api.model("User", {
    "id": fields.String(readonly=True, description="The user ID"),
    "name": fields.String(required=True, description="User name"),
    "email": fields.String(required=True, description="User email")
})
```

---

## 🚀 Endpoints with Swagger Annotations

```python
ns = api.namespace("users", description="User operations")

@ns.route("/")
class UserList(Resource):
    @ns.doc("list_users")
    @ns.marshal_list_with(user_model)
    def get(self):
        """List all users"""
        return [{"id": "1", "name": "Alice", "email": "a@mail.com"}]

    @ns.doc("create_user")
    @ns.expect(user_model)
    @ns.marshal_with(user_model, code=201)
    def post(self):
        """Create a new user"""
        return api.payload, 201


@ns.route("/<string:id>")
@ns.response(404, "User not found")
@ns.param("id", "The user identifier")
class User(Resource):
    @ns.doc("get_user")
    @ns.marshal_with(user_model)
    def get(self, id):
        """Fetch a single user"""
        return {"id": id, "name": "Alice", "email": "a@mail.com"}
```

---

## 🧰 `@ns.doc()` Parameters

|Option|Description|Example|
|---|---|---|
|`id`|Unique operation ID|`"get_user"`|
|`params`|Describe query/path params|`{"id": "User ID"}`|
|`description`|Custom description|`"Returns user details"`|
|`responses`|Document response codes|`{404: "Not found"}`|
|`security`|Define security schema|`{"jwt": []}`|

---

## 🔍 Query / Path Parameters

```python
parser = api.parser()
parser.add_argument("page", type=int, required=False, help="Page number")

@ns.route("/paged")
class Paged(Resource):
    @ns.expect(parser)
    @ns.doc(description="List users with pagination")
    def get(self):
        args = parser.parse_args()
        return {"page": args.get("page", 1)}
```

Swagger UI automatically documents `page` as a query param.

---

## ⚙️ Response Examples

```python
@ns.response(200, "Success", user_model)
@ns.response(404, "User not found")
@ns.marshal_with(user_model)
def get(self, id):
    """Fetch a user"""
    ...
```

---

## 🔒 Security (JWT Example)

```python
authorizations = {
    "jwt": {
        "type": "apiKey",
        "in": "header",
        "name": "Authorization"
    }
}

api = Api(app, authorizations=authorizations, security="jwt")

@ns.doc(security="jwt")
class SecureResource(Resource):
    def get(self):
        return {"msg": "You’re authorized!"}
```

Swagger UI will show a lock icon 🔒 and let you set the Bearer token.

---

## 🧭 Disable or Customize Swagger UI

|Task|Code|
|---|---|
|Disable Swagger|`Api(app, doc=False)`|
|Change path|`Api(app, doc="/swagger")`|
|Hide endpoints|`@ns.hide` decorator|
|Add custom metadata|`Api(..., contact="you@example.com")`|

---

## 📜 Example Folder Structure

```
project/
├─ app.py
├─ api/
│  ├─ __init__.py
│  ├─ models.py
│  ├─ users.py
│  └─ ...
```

---

## 🧾 Quick Reference Table

|Feature|Decorator|Swagger Effect|
|---|---|---|
|Describe endpoint|`@ns.doc()`|Adds description + params|
|Expected body|`@ns.expect(model)`|Shows input model|
|Return schema|`@ns.marshal_with(model)`|Shows output model|
|Query params|`@ns.expect(parser)`|Auto-docs query fields|
|Response codes|`@ns.response()`|Adds status code docs|
|Hide route|`@ns.hide`|Hides from Swagger UI|