---
name: "MongoDB guide"
description: "Using the Exorails MongoDB connector well: sample the schema first, filter and project, prefer aggregate for anything non-trivial, and confirm before update_many or delete_many."
---

# MongoDB

Tools: `schema`, `find`, `aggregate`, `count`, `distinct`, and `insert_one`, `update_many`, `delete_many` when writes are allowed.

## Schema first

Collections have no declared schema. `schema` samples documents of a collection and reports the fields and types seen. Read it before writing a filter: a field you assume exists may be nested or named differently.

## Reading

- `find` takes a filter, an optional projection and a limit (documents are capped). Always project the fields you need; documents can be large.
- `count` and `distinct` answer their question without returning documents; prefer them to a `find` you would count yourself.
- `aggregate` is the tool for grouping, joining (`$lookup`), or reshaping. Put `$match` first so the pipeline reads as little as possible.

## Operators the connector refuses

`$where` and JavaScript evaluation are not allowed: they run code on the server. Express the condition with query operators instead.

## Writes

`update_many` and `delete_many` act on every document matching the filter. Before running one, show the filter and run `count` with it so the user sees how many documents are affected, and wait for a yes. An empty filter matches the whole collection.
