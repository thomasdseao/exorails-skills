---
name: "MySQL guide"
description: "Using the Exorails MySQL and MariaDB connector well: schema first, parameters as placeholders, row limits, explain before heavy joins, and the read-only rule."
---

# MySQL

Tools: `schema`, `query`, `explain`, and `execute` when writes are allowed. Prefixed with the server name when the environment has several servers.

## Schema first

`schema` without a table lists the tables of the database; with `table` it describes columns, keys and indexes. Read it before writing SQL against a table you have not seen.

## Query

- One statement per `query`. Use `?` placeholders with `params`; never build SQL by string concatenation with user input.
- Rows are capped (200 by default). Prefer `WHERE`, `GROUP BY` and `LIMIT` over paging.
- On a read-only server the session is read-only: writes fail at the database. Report it, do not retry.
- Long queries are cancelled at the statement timeout. Check the plan with `explain` before joining large tables.

## Writes

`execute` appears only when the server allows writes. It runs in a transaction and returns the affected row count. For an `UPDATE` or `DELETE` whose `WHERE` is broad, show the statement and wait for the user's confirmation first.

## Details that bite

- `utf8mb4` is what modern schemas use; a `utf8` column truncates emoji.
- `DATETIME` has no time zone; `TIMESTAMP` is converted to the session's. Say which one a column is when the user asks about times.
