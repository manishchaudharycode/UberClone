# User Registration Endpoint Documentation

## Endpoint

`POST /users/register`

## Description

Registers a new user in the system. This endpoint creates a user account with the provided full name, email, and password. The password is securely hashed before storage. If registration is successful, a JWT authentication token is returned along with the user data.

## Request Body

Send a JSON object with the following fields:

| Field    | Type   | Required | Description                       |
| -------- | ------ | -------- | --------------------------------- |
| fullname | string | Yes      | The full name of the user         |
| email    | string | Yes      | The user's email address          |
| password | string | Yes      | The user's password (min 6 chars) |

**Example:**

```json
{
  "fullname": "John Doe",
  "email": "john@example.com",
  "password": "yourpassword"
}
```

## Validation

- `email` must be a valid email address.
- `password` must be at least 6 characters long.
- `fullname` must not be empty.

## Responses

### Success

- **Status Code:** `201 Created`
- **Body:**

```json
{
  "user": {
    "_id": "...",
    "fullname": "John Doe",
    "email": "john@example.com",
    ...
  },
  "token": "<jwt_token>"
}
```

### Validation Error

- **Status Code:** `400 Bad Request`
- **Body:**

```json
{
  "errors": [
    { "msg": "Invalid email address", "param": "email", ... },
    ...
  ]
}
```

### Missing Fields

- **Status Code:** `400 Bad Request`
- **Body:**

```json
{
  "error": "All fields are required"
}
```

### User Already Exists

- **Status Code:** `400 Bad Request`
- **Body:**

```json
{
  "error": "User already exists"
}
```

## Notes

- The password is never returned in the response.
- The returned JWT token can be used for authenticated requests.


## Captain Registration

### Endpoint

`POST /captain/register`

### Description

Registers a new captain (driver) with vehicle details. Returns captain data and JWT token on success.

### Request Body

```jsonc
{
  "fullname": "Jane Smith",                // string, required, not empty
  "email": "jane@captain.com",             // string, required, valid email
  "password": "strongpassword",            // string, required, min 7 chars
  "vehicle": {
    "color": "Red",                        // string, required, min 3 chars
    "plate": "AB123CD",                    // string, required, min 3 chars
    "capacity": 4,                         // integer, required, min 1
    "vehicaleType": "car"                  // string, required, one of: "bike", "car", "auto"
  }
}
```
### Success Response

```jsonc
{
  "captain": {
    "_id": "...",
    "fullname": "Jane Smith",
    "email": "jane@captain.com",
    "vehicle": {
      "color": "Red",
      "plate": "AB123CD",
      "capacity": 4,
      "vehicaleType": "car"
    }
    // ...other fields
  },
  "token": "<jwt_token>"
}
```

### Error Responses

- **402 Captain already exists**:  
  ```jsonc
  { "message": "Captain already exists" }
  ```
- **400 Bad Request**:  
  - Validation errors (invalid email, password too short, missing vehicle fields, etc.)
  - Missing required fields

---
# Notes

- Passwords are never returned in responses.
- JWT token should be used for authenticated requests.
- All endpoints expect and return JSON.