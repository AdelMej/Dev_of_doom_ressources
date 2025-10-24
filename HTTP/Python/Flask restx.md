## 🧱 Basic Setup

```python
from flask import Flask
from flask_restx import Api, Resource, fields

app = Flask(__name__)
api = Api(app, title="My API", version="1.0", description="Example RESTX app")
```
---

## 🧩 Namespaces (for route grouping)

```python
ns = api.namespace("users", description="User operations")
api.add_namespace(ns)     # or just declare @ns.route()
```

---

## 📦 Models (for request/response schemas)

```python
user_model = api.model("User", {
    "id": fields.String(readonly=True),
    "name": fields.String(required=True, description="User name"),
    "email": fields.String(required=True)
})
```

---

## 🚀 Endpoints

```python
@ns.route("/")
class UserList(Resource):
    @ns.marshal_list_with(user_model)
    def get(self):
        """List all users"""
        return [{"id": "1", "name": "Alice", "email": "a@mail.com"}]

    @ns.expect(user_model)
    @ns.marshal_with(user_model, code=201)
    def post(self):
        """Create a new user"""
        data = api.payload
        return data, 201


@ns.route("/<string:id>")
@ns.response(404, "User not found")
class User(Resource):
    @ns.marshal_with(user_model)
    def get(self, id):
        """Get a user by ID"""
        return {"id": id, "name": "Alice", "email": "a@mail.com"}

    def delete(self, id):
        """Delete a user"""
        return "", 204
```

---

## 🧠 Request Parsing (alternative to `api.expect`)

```python
parser = api.parser()
parser.add_argument("page", type=int, required=False, help="Page number")

@ns.route("/paginated")
class Paginated(Resource):
    @ns.expect(parser)
    def get(self):
        args = parser.parse_args()
        return {"page": args.get("page", 1)}
```

---

## 🧰 Marshaling vs Expecting

|Decorator|Purpose|
|---|---|
|`@api.expect(model)`|Validates request body|
|`@api.marshal_with(model)`|Formats single object response|
|`@api.marshal_list_with(model)`|Formats list responses|

---

## ⚙️ Error Handling

```python
@api.errorhandler(ValueError)
def handle_value_error(error):
    return {"message": str(error)}, 400
```

---

## 📜 Disable Swagger UI (if it annoys you 😂)

```python
api = Api(app, doc=False)  # hides /swagger or /docs
```

---

## 🧩 Folder Structure Example

```
project/
├─ app.py
├─ api/
│  ├─ __init__.py
│  ├─ models.py
│  ├─ routes/
│  │   ├─ user_routes.py
│  │   └─ ...
```

---

## 🧠 Quick Reference

|Action|Code|
|---|---|
|Add Namespace|`api.add_namespace(ns)`|
|Define Model|`api.model("Name", {...})`|
|Parse Args|`parser.parse_args()`|
|Access Body|`api.payload`|
|Access Query|`request.args.get("param")`|
|Return JSON|`return data, status_code`|
|Disable Docs|`Api(..., doc=False)`|