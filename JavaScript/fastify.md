# Fastify - Lightning Fast Web Framework for Node.js

Fastify is a performance-oriented backend web framework for Node.js.

## Installation

Install with npm:

```shell
npm i fastify # i is shorthand for install
```

## First server

Here is the starting file from official Fastify documents:
[Fastify Guides: Getting Started](https://fastify.dev/docs/latest/Guides/Getting-Started/)

```js
// Require the framework and instantiate it

// ESM
import Fastify from 'fastify'

const fastify = Fastify({
  logger: true
})
// CommonJs
const fastify = require('fastify')({
  logger: true
})

// Declare a route
fastify.get('/', function (request, reply) {
  reply.send({ hello: 'world' })
})

// Run the server!
fastify.listen({ port: 3000 }, function (err, address) {
  if (err) {
    fastify.log.error(err)
    process.exit(1)
  }
  // Server is now listening on ${address}
})
```

### Fastify Server Instance

Create a fastify server instance. This object is the control point for defining
routes, handling requests, sending replies, starting the server, validation
& serialization, lifecycle hooks, etc.

Use the factory function like this:

`const fastify = Fastify();`

There you can use it in many ways.

### Declaring Routes

Fastify offers shorthand methods for HTTP Request methods.

See here for more details on HTTP methods:
[MDN HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)

#### Quick Reference

| HTTP Method | Fastify method | Description |
| ----------- | -------------- | ----------- |
| GET | `.get(url, handler)` | Requests a representaion of data; only retrieving |
| QUERY | `.query(url, handler)` | Server-side query, safe calls |
| HEAD | `.head(url, handler)` | Identical to `.get()`, but no response body |
| POST | `.post(url, handler)` | Submits enitity to resource, often changing state on server |
| PUT | `.put(url, handler)` | Replaces all current representations of target resource |
| DELETE | `.delete(url, handler)` | Delete a specified resource |
| OPTIONS | `.options(url, handler)` | Describes communication options for target resource |
| PATCH | `.patch(url, handler)` | Applies partial modifications to a resource |

Fastify also allows users to register a route with `.route()`

```js
const fastify = Fastify();
fastify.route({ method, url, handler });
```
