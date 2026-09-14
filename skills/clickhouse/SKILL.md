---
name: "ClickHouse through Exorails"
description: "Using the Exorails ClickHouse connector well: analytical queries over large tables, why you aggregate instead of selecting rows, readonly mode, and the FORMAT and LIMIT rules."
---

# ClickHouse through Exorails

Tools: `schema`, `query`, `explain`, and `execute` when writes are allowed. ClickHouse is a column store built for aggregation over very large tables: shape your queries accordingly.

## Aggregate, do not scan

- Selecting raw rows from a billion-row table is the wrong tool. Use `count()`, `sum()`, `uniq()`, `quantile()` and `GROUP BY` with a time bucket (`toStartOfHour`, `toDate`).
- Filter on the table's primary key or partition key first; `schema` shows the engine, sort key and partition key of a table.
- Rows returned are capped (200 by default); the cap is on rows, not on the data scanned, so a bad query can still take the full statement timeout.

## Readonly

By default the session is `readonly=1`: only SELECT-like statements. `INSERT`, `ALTER`, `DROP` are refused by the server. `execute` appears only when the user turns read-only off.

## Useful specifics

- `explain` shows the query plan; `EXPLAIN ESTIMATE` in a `query` shows how many parts and rows would be read, which is the number to look at before a heavy query.
- Dates are UTC unless the column has a time zone.
- `FINAL` is expensive; use it only when the user needs the deduplicated view of a ReplacingMergeTree.
