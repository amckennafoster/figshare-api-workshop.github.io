---
layout: workshop-lesson
---

# Introduction to HTTP Requests and APIs

## What is an HTTP Request?
- Headers
    - Authorization
    - Content-Type
- Request parameters
- Request body

Example Request:
```
POST /v2/articles/search?offset=10&limit=1 HTTP/1.1
Host: api.figshare.com
Content-Type: application/json
Authorization: Bearer b2be49036a3158c5edd5a0553ae9

{"search_for": ":tags: test"}
```
## What is an API?
It’s an interface which allows other applications to use the functionality of a software system. You send in an HTTP request and receive a response.

### REST
- Abbreviation for REpresentational State Transfer
- A set of principles on how web APIs should be built
- Servers should always respond with the representation of a resource

### JSON
JSON is a format that provides information in a structured way.

- Abbreviation for JavaScript Object Notation
- Human-readable data interchange format
- Consists of key-value pairs, lists and other basic types (numbers, strings)
- For example: {“name”: “Jane Doe”, “someList”: [1, “two”, 3]}

*Learn more about Figshare's <a href="https://help.figshare.com/article/figshare-metadata-schema-overview" target="_blank">metadata schema</a>*
 
## HTTP status codes
HTTP requests generate status codes. These are a standard (and the easiest) way to understand what happened with your requests
Some common status codes:
- 200 OK
- 400 Bad request
- 403 Forbidden
- 404 Not Found
- 500 Internal Server Error
- <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status" target="_blank">Full list</a>


## HTTP request Body
The body is the actual data sent to the server
- It’s any sequence of bytes (JSON, XML, an image file) but we need to tell the server what we are sending
    - Content-Type: application/json
- The server response can also contain any sequence of bytes but…
- We can tell the server what we accept:
    - Accept: */*
- The request/response bodies are optional

## HTTPS
- Secure HTTP
- A secure channel is created between the client and the server
- Request details (URL, headers, body) are not visible on the open network
- Important if sensitive data (e.g., authentication credentials) are exchanged





