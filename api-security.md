# API SECURITY

### Basic usename, password based authentication for API
In general, in basic auth clients call API keeping username:password in the Authorization header for the APIs. 
By standard basic auth annotation, the username:password will be Base 64 encoded string.
```
HTTP
GET /notes/{id} HTTP/1.1
Host: secret-note.com
Content-Type: application/json
Authorization: Basic MzMzOjQ0NA==
```
