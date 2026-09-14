---
name: "Elasticsearch through Exorails"
description: "Using the Exorails Elasticsearch and OpenSearch connector well: check the mapping, write query DSL rather than free text, keep hits small, and use aggregations for numbers."
---

# Elasticsearch through Exorails

Tools: `indices`, `mapping`, `search`, `get`, `count`, `cluster_health`, and `index_document`, `delete_document` when writes are allowed.

## Before searching

- `indices` lists the indices with their document counts and size. Search the right one; wildcards across all indices are slow.
- `mapping` shows the fields and their types. A `text` field is analysed and matched by terms; a `keyword` field is matched exactly. Filtering on the wrong one returns nothing.

## Searching

- `search` takes query DSL. For an exact value use `term` on a keyword field; for words use `match`; for ranges use `range`. Combine with `bool` (`must`, `filter`, `should`).
- Hits are capped (50 by default, 500 at most). Sort, filter and use `_source` to select fields rather than asking for more hits.
- For "how many" or "sum of" questions, use `aggs` with `size: 0`: the answer comes without hits.
- Script queries and scripted fields are refused: they execute code on the cluster.

## Health

`cluster_health` answers green, yellow or red with the number of unassigned shards. Yellow on a single node is normal.

## Writes

`index_document` and `delete_document` exist only when writes are allowed. Deleting is by id; there is no delete-by-query through the connector, on purpose.
