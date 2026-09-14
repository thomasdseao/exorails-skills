---
name: REST API through Exorails
description: Using the Exorails REST and OpenAPI connector well: read the operation list, send the right method and body, treat writes with care, and read status codes as answers rather than failures.
---

# REST API through Exorails

The connector wraps an HTTP API, described by an OpenAPI document when one was given. Tool: `request`, and one tool per operation when the document lists them. Authentication is injected by the gateway; do not add headers for it.

## Calling

- With an OpenAPI document, the operations appear as tools with their parameters and descriptions: prefer them to a raw `request`.
- With `request`: give the method, the path relative to the API's base, query parameters, and a JSON body when the method takes one. The base address is fixed by the environment; you cannot call another host.
- On a read-only connector, only GET, HEAD and OPTIONS are sent. Writes are refused before leaving the gateway.

## Reading answers

- A 4xx is an answer, not a transport failure: 400 means the request was wrong, 404 the resource does not exist, 409 a conflict, 422 a validation error whose body says which field.
- A 429 means the API is rate limiting: wait and retry once, then report.
- Bodies are bounded; ask for a page or a filter rather than the whole collection.

## Writes

POST, PUT, PATCH and DELETE exist only when writes are allowed. Before a DELETE or a PUT that replaces a resource, show the request and wait for a yes. Idempotency keys, when the API supports them, belong in the request.
