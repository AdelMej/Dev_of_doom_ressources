# .http File Cheat Sheet

## Basic Request Structure

```http
### Request Name (comment)
METHOD URL
Header-Name: Header-Value
Header-Name2: Header-Value2

{
  "body": "data"
}
```

## HTTP Methods

```http
### GET Request
GET http://localhost:5000/api/users

### POST Request
POST http://localhost:5000/api/users
Content-Type: application/json

{
  "name": "John"
}

### PUT Request
PUT http://localhost:5000/api/users/123
Content-Type: application/json

{
  "name": "Jane"
}

### PATCH Request
PATCH http://localhost:5000/api/users/123
Content-Type: application/json

{
  "email": "new@email.com"
}

### DELETE Request
DELETE http://localhost:5000/api/users/123
```

## Headers

```http
### With Headers
POST http://localhost:5000/api/login
Content-Type: application/json
Accept: application/json
User-Agent: MyApp/1.0

{
  "email": "test@example.com"
}

### With Authorization
GET http://localhost:5000/api/protected
Authorization: Bearer YOUR_TOKEN_HERE

### Multiple Headers
POST http://localhost:5000/api/data
Content-Type: application/json
X-Custom-Header: CustomValue
Cache-Control: no-cache
```

## Variables

### Named Requests (Capture Response)
```http
### Login and save response
# @name login
POST http://localhost:5000/api/login
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "pass123"
}

### Use token from login response
GET http://localhost:5000/api/users
Authorization: Bearer {{login.response.body.access_token}}
```

### Environment Variables
```http
### Define variables at top of file
@baseUrl = http://localhost:5000
@apiVersion = v1
@token = your-static-token-here

### Use variables
GET {{baseUrl}}/api/{{apiVersion}}/users
Authorization: Bearer {{token}}
```

### Response Variables
```http
# @name createUser
POST http://localhost:5000/api/users
Content-Type: application/json

{
  "name": "John"
}

### Use response data
GET http://localhost:5000/api/users/{{createUser.response.body.id}}

### Access nested response data
POST http://localhost:5000/api/orders
Authorization: Bearer {{login.response.body.data.token}}
Content-Type: application/json

{
  "user_id": "{{createUser.response.body.user.id}}"
}
```

## Request Body Types

### JSON Body
```http
POST http://localhost:5000/api/users
Content-Type: application/json

{
  "name": "John",
  "email": "john@example.com",
  "age": 30
}
```

### Form Data
```http
POST http://localhost:5000/api/upload
Content-Type: application/x-www-form-urlencoded

name=John&email=john@example.com&age=30
```

### Multipart Form Data
```http
POST http://localhost:5000/api/upload
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary

------WebKitFormBoundary
Content-Disposition: form-data; name="file"; filename="test.txt"
Content-Type: text/plain

File content here
------WebKitFormBoundary--
```

### Raw/Text Body
```http
POST http://localhost:5000/api/data
Content-Type: text/plain

Just some plain text data
```

## Query Parameters

```http
### In URL
GET http://localhost:5000/api/users?page=1&limit=10&sort=name

### Multiple params
GET http://localhost:5000/api/search?q=test&category=books&price_min=10&price_max=50
```

## Comments and Separators

```http
### This is a request separator
# This is a comment
// This is also a comment

GET http://localhost:5000/api/users

###
# Another request below
###

POST http://localhost:5000/api/users
```

## Real-World Example: Complete API Test Flow

```http
### Variables
@baseUrl = http://localhost:5000/api/v1

### 1. Create User
# @name createUser
POST {{baseUrl}}/users
Content-Type: application/json

{
  "email": "test@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "password": "password123"
}

### 2. Login
# @name login
POST {{baseUrl}}/login
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "password123"
}

### 3. Get All Places (Protected)
GET {{baseUrl}}/places
Authorization: Bearer {{login.response.body.access_token}}

### 4. Create Place
# @name createPlace
POST {{baseUrl}}/places
Authorization: Bearer {{login.response.body.access_token}}
Content-Type: application/json

{
  "title": "Cozy Apartment",
  "description": "A beautiful place to stay",
  "price": 100.0,
  "latitude": 48.8566,
  "longitude": 2.3522,
  "owner_id": "{{login.response.body.user.id}}"
}

### 5. Get Specific Place
GET {{baseUrl}}/places/{{createPlace.response.body.id}}
Authorization: Bearer {{login.response.body.access_token}}

### 6. Create Review
# @name createReview
POST {{baseUrl}}/reviews
Authorization: Bearer {{login.response.body.access_token}}
Content-Type: application/json

{
  "place_id": "{{createPlace.response.body.id}}",
  "text": "Amazing place! Highly recommend!",
  "rating": 5
}

### 7. Get Reviews for Place
GET {{baseUrl}}/places/{{createPlace.response.body.id}}/reviews
Authorization: Bearer {{login.response.body.access_token}}

### 8. Update User
PUT {{baseUrl}}/users/{{login.response.body.user.id}}
Authorization: Bearer {{login.response.body.access_token}}
Content-Type: application/json

{
  "first_name": "Jane"
}

### 9. Delete Review
DELETE {{baseUrl}}/reviews/{{createReview.response.body.id}}
Authorization: Bearer {{login.response.body.access_token}}

### 10. Logout (if you have this endpoint)
POST {{baseUrl}}/logout
Authorization: Bearer {{login.response.body.access_token}}
```

## Tips

1. **Separate requests with `###`** - Makes it clear where each request starts
2. **Name important requests** with `# @name varName` to reuse their responses
3. **Use variables** for base URLs and tokens to make maintenance easier
4. **Comments start with `#` or `//`** - Use them to document your requests
5. **Run requests in order** - Named requests need to be executed before you can use their responses
6. **Response syntax**: `{{requestName.response.body.path.to.value}}`
7. **No trailing comma in JSON** - Last item in objects/arrays shouldn't have a comma

## Kulala.nvim Specific

Default keybindings (check `:h kulala` for full list):
- Run current request: Depends on your config
- Run all requests: Sequential execution
- Jump to next request: `]]` or similar
- Jump to previous request: `[[` or similar

Add custom keybindings in your config:
```lua
vim.keymap.set('n', '<leader>rr', require('kulala').run, { desc = 'Run request' })
vim.keymap.set('n', '<leader>ra', require('kulala').run_all, { desc = 'Run all' })
```