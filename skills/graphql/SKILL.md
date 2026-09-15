---
name: "GraphQL guide"
description: "Using the Exorails GraphQL connector well: explore the schema, ask for exactly the fields you need, use variables, read errors from the payload, and the mutation rule."
---

# GraphQL

Tools: `schema`, `query`.

## Explore first

`schema` returns the types, queries and mutations the endpoint exposes, with their arguments. Read the type you need before writing a query: GraphQL rejects a single unknown field with a whole-request error.

## Querying

- Ask for exactly the fields you need. Over-fetching is slow and the result is bounded.
- Pass values through `variables`, never by pasting them into the document.
- Paginate with the schema's convention (`first`/`after` cursors, or `limit`/`offset`). Do not ask for thousands of nodes.
- Errors arrive in the `errors` array of an otherwise 200 response. Read them; a partial `data` with `errors` is a common shape.

## Mutations

On a read-only connector, a document that declares a mutation is refused before it is sent, whatever its shape. When writes are allowed, show the mutation and its variables to the user before running one that creates, updates or deletes.
