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

## Pros and Cons

| Aspect | Pros | Cons |
| ------ | ---- | ---- |
| Simplicity | Easy to understand and debug; plain-text request/response model | Can feel verbose compared to binary protocols |
| Statelessness | Scales well and is easy to cache | State must be managed separately with cookies, sessions, or tokens |
| Compatibility | Works across browsers, servers, and APIs | Not ideal for low-latency or specialized systems |
| Security | Can be secured with HTTPS | Plain HTTP is insecure and exposes data in transit |
| Flexibility | Supports many content types such as HTML, JSON, and files | Many behaviors depend on headers and server implementation |

HTTP is simple, universal, and easy to use, which makes it well suited for the web. Its main limitations are that it is stateless and insecure without HTTPS, so state and security must be handled separately.
