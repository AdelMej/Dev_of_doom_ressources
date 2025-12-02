# Python DTO Cheat Sheet (Pydantic Edition)

## 🧙‍♂️ Why DTOs in Python?

DTOs (Data Transfer Objects) let you validate and structure
incoming/outgoing data with zero clownery.\
Using **Pydantic**, you get Spring Boot--level validation and clean
controllers.

------------------------------------------------------------------------

## 📥 Input DTO Example

``` python
from pydantic import BaseModel, EmailStr, Field

class UserIn(BaseModel):
    name: str = Field(min_length=1)
    age: int = Field(gt=0)
    email: EmailStr
```

------------------------------------------------------------------------

## 📤 Output DTO Example

``` python
class UserOut(BaseModel):
    id: int
    name: str
    age: int
```

------------------------------------------------------------------------

## 🎯 Using DTOs in Flask-RESTX

``` python
@api.route("/users")
class UserController(Resource):
    def post(self):
        dto = UserIn(**request.json)  # validates automatically
        user = service.create(dto)
        return UserOut(**user).dict(), 201
```

------------------------------------------------------------------------

## 🛡️ Benefits

-   Automatic validation
-   Automatic type checking
-   Automatic error messages
-   No 500-level clownery
-   Cleaner controllers
-   Predictable data structures

------------------------------------------------------------------------

## 🔮 Custom Validators

``` python
from pydantic import validator

class UserIn(BaseModel):
    name: str
    age: int

    @validator("name")
    def no_empty_name(cls, v):
        if v.strip() == "":
            raise ValueError("Name cannot be empty.")
        return v
```

------------------------------------------------------------------------

## 🚀 Nested DTOs

``` python
class Address(BaseModel):
    street: str
    city: str

class UserIn(BaseModel):
    name: str
    address: Address
```

------------------------------------------------------------------------

## ⚙️ Converting Entities → DTOs

``` python
def to_dto(user_entity):
    return UserOut(
        id=user_entity.id,
        name=user_entity.name,
        age=user_entity.age
    )
```

------------------------------------------------------------------------

## 🧩 Full DTO Pipeline

**IN DTO → Service → OUT DTO**

Just like Spring Boot.

------------------------------------------------------------------------

Happy coding, dark wizard 🧙‍♂️
