---
name: "PostgreSQL guide"
description: "Using the Exorails PostgreSQL connector well: inspect the schema before querying, keep result sets small, explain before heavy queries, and how read-only and write modes behave."
---

# PostgreSQL

The connector speaks the PostgreSQL wire protocol from the gateway. Tools: `schema`, `query`, `explain`, and `execute` when the server is not read-only. With several servers they are prefixed, for example `prod-db__query`.

## Start with the schema

Call `schema` with no argument to list tables, then `schema` with `table` to see columns, keys and indexes. Do not guess column names: a wrong guess costs a round trip and a confusing error.

## Query

- `query` runs one statement. Pass values through `params` as `$1, $2…`; never interpolate user input into the SQL.
- Rows are capped by `limit` (default 200, at most 2000). Aggregate or filter rather than paging through a large table.
- Every query runs in a READ ONLY transaction on a read-only server: an INSERT, UPDATE or DELETE is refused by the database, not by you. Say so instead of retrying.
- Queries longer than the statement timeout (15 s by default) are cancelled. Narrow the query, add a WHERE clause on an indexed column, or run `explain` first.

## Explain before heavy queries

`explain` returns the plan. Use it before a join across large tables or a query the user will run repeatedly. With `analyze` true the statement is executed to measure it, which is safe on a read-only server.

## Writes

`execute` exists only when the server allows writes. It runs inside a transaction and returns the affected row count or the RETURNING rows. Before an UPDATE or DELETE without a narrow WHERE clause, show the statement to the user and wait for a yes. DDL is irreversible: same rule.

## Reading results

Results come back as a table. Dates are in the server's time zone unless the column is `timestamptz`. NULL is shown as empty; ask with `IS NULL` when it matters.
