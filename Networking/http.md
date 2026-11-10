# HTTP (HyperText Transfer Protocol)

HTTP is an application layer protocol in the **Internet protocol suite** used
betwwen **clients** and **servers** over the web. It uses the request-response
protocol.

In the most simplist scenario, a web browser acts as the client and a web server
acts as the server.

An **application layer** is an **abstraction layer** that specifies the shared
communication protocols and interface methods used by hosts in a communications
network.

```text
+--------+                          +--------+
| Client |  --------------------->  | Server |
|        |     HTTP request         |        |
|        |                          |        |
|        |  <---------------------  |        |
|        |     HTTP response        |        |
+--------+                          +--------+
```

## Data Exchange

HTTP is a **stateless** application-level protocol and it requires a reliable
network transport connection to exchange data between client and server. In
HTTP implementations, **TCP/IP** are used using well known ports.

## HTTP overview

HTTP is a simple, stateless, text-based protocol used for communication between
clients and servers on the web. A client sends a request, and a server sends back
a response.

## Request and Response Layout

HTTP requests and responses have a similar structure: a start line, headers, and an optional body. The main difference is that a request tells the server what to do, while a response tells the client the result.

| Part | In a request | In a response | Purpose |
| ---- | ------------ | ------------- | ------- |
| Start line | `METHOD /path?query=123 HTTP/1.1` | `HTTP/1.1 200 OK` | Describes the action or result and the HTTP version. |
| Method / status | GET, POST, PUT, DELETE, etc. | Status code such as `200`, `404`, `500` | Tells the server what action to perform or tells the client the outcome. |
| Target / resource | URL path and query string, such as `/users?id=42` | The response body and metadata | Identifies the resource involved. |
| Headers | `Host`, `Accept`, `Content-Type`, `Authorization`, etc. | `Content-Type`, `Content-Length`, `Set-Cookie`, etc. | Carries metadata about the request or response. |
| Body | Data sent to the server, such as JSON or form data | Data returned by the server, such as HTML, JSON, or files | Contains the payload. |
| Query string | `?name=alice&active=true` | Not usually used | Extra parameters attached to the URL. |
| Response code | Not present | `200 OK`, `404 Not Found`, `500 Internal Server Error` | Indicates whether the request succeeded or failed. |

## Methods

| HTTP Method | Description |
| ----------- | ----------- |
| GET | Requests a representation of a resource. It is intended for retrieving data and should be safe and idempotent. |
| HEAD | Same as GET, but only returns headers and no response body. |
| POST | Submits data to the server for processing, often creating or updating resources and changing server state. |
| PUT | Replaces the target resource with the request payload, or creates it if it does not exist. |
| PATCH | Applies partial modifications to a resource without replacing the whole resource. |
| DELETE | Deletes the identified resource. |
| OPTIONS | Describes the communication options or supported methods for the target resource. |
| TRACE | Echoes the request back to the client for debugging and diagnostics. |
| CONNECT | Establishes a tunnel to the server identified by the target resource, commonly used for HTTPS proxies. |

## Parameters

| Parameter type | Example | Where it appears | Use case |
| -------------- | ------- | ---------------- | -------- |
| Path parameter | `/users/42` | In the URL path | Identifies a specific resource, such as a user ID or item ID. |
| Query parameter | `/users?id=42&sort=name` | In the URL after `?` | Filters, searches, sorts, or paginates data. |
| Body parameter | `{"name":"Alice"}` | In the request body | Sends data to create or update a resource. |
| Header parameter | Authorization: Bearer token | In headers | Sends metadata such as auth tokens, content type, or API keys. |

## Example

Example request:

```http
GET /api/users?id=42&active=true HTTP/1.1
Host: example.com
Accept: application/json
```

Example response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 32

{"id":42,"name":"Alice"}
```

## Default Ports

| Protocol | Default port | Notes |
| -------- | ------------ | ----- |
| HTTP | 80 | Plain text, unencrypted |
| HTTPS | 443 | HTTP over TLS, encrypted |
| Alternate HTTP | 8080 | Common for development or proxies, not a default |

## HTTPS

**HTTPS** is HTTP sent over a **TLS** (Transport Layer Security) connection.
TLS encrypts the data so it can't be read or altered in transit, and it
authenticates the server using a certificate. The HTTP messages themselves
are unchanged; only the connection underneath is secured.

## Status Code Classes

| Range | Class | Meaning | Examples |
| ----- | ----- | ------- | -------- |
| 1xx | Informational | Request received, continuing process | `100 Continue`, `101 Switching Protocols` |
| 2xx | Success | Request was received and processed successfully | `200 OK`, `201 Created`, `204 No Content` |
| 3xx | Redirection | Further action is needed, usually going to another URL | `301 Moved Permanently`, `302 Found`, `304 Not Modified` |
| 4xx | Client error | The request is invalid or can't be fulfilled | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found` |
| 5xx | Server error | The server failed to handle a valid request | `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable` |

### 401 vs 403

| Code | Meaning | Example |
| ---- | ------- | ------- |
| `401 Unauthorized` | The client is **not authenticated**: credentials are missing or invalid. | Requesting a profile page without logging in. |
| `403 Forbidden` | The client **is authenticated** but doesn't have permission. | A logged-in regular user requesting an admin-only page. |

## Safe and Idempotent Methods

- **Safe**: the method only reads data and doesn't change server state.
- **Idempotent**: sending the same request multiple times has the same effect
  on the server as sending it once.

| Method | Safe | Idempotent |
| ------ | ---- | ---------- |
| GET | Yes | Yes |
| HEAD | Yes | Yes |
| OPTIONS | Yes | Yes |
| TRACE | Yes | Yes |
| PUT | No | Yes |
| DELETE | No | Yes |
| POST | No | No |
| PATCH | No | Not guaranteed |
| CONNECT | No | No |

Example: submitting the same `POST /orders` twice can create two orders,
while sending the same `PUT /users/42` twice leaves user 42 in the same state.

## Pros and Cons

| Aspect | Pros | Cons |
| ------ | ---- | ---- |
| Simplicity | Easy to understand and debug; plain-text request/response model | Can feel verbose compared to binary protocols |
| Statelessness | Scales well and is easy to cache | State must be managed separately with cookies, sessions, or tokens |
| Compatibility | Works across browsers, servers, and APIs | Not ideal for low-latency or specialized systems |
| Security | Can be secured with HTTPS | Plain HTTP is insecure and exposes data in transit |
| Flexibility | Supports many content types such as HTML, JSON, and files | Many behaviors depend on headers and server implementation |

HTTP is simple, universal, and easy to use, which makes it well suited for the web. Its main limitations are that it is stateless and insecure without HTTPS, so state and security must be handled separately.
