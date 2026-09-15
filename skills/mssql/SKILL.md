---
name: "SQL Server guide"
description: "Using the Exorails SQL Server connector well: T-SQL specifics, TOP instead of LIMIT, schema-qualified names, and the read-only guarantee the connector enforces."
---

# SQL Server

Tools: `schema`, `query`, and `execute` when writes are allowed.

## T-SQL, not MySQL

- Use `SELECT TOP (n) …` rather than `LIMIT`. Rows are also capped by the connector (200 by default).
- Names are schema-qualified: `dbo.Orders`. `schema` lists them with their schema.
- String literals use single quotes; `N'…'` for Unicode.
- Dates: `GETDATE()`, `DATEADD`, `DATEDIFF`. `CONVERT(varchar, x, 120)` gives ISO format.

## Read-only, guaranteed

On a read-only server every `query` runs inside a transaction that is always rolled back, and statements that write in disguise (`SELECT … INTO`, `MERGE`, `EXEC` of a writing procedure) are refused. If the user needs a write, they turn read-only off in the server's settings; then `execute` appears.

## Writes

`execute` runs one statement in a transaction and returns the affected row count. Broad `UPDATE` or `DELETE`, `TRUNCATE`, `DROP`: show the statement and wait for a yes.

## Performance

Prefer indexed columns in `WHERE`. `SET SHOWPLAN` is not available through the connector; ask the user for the plan if a query is slow rather than retrying it.
