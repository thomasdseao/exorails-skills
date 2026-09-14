---
name: "Prometheus through Exorails"
description: "Using the Exorails Prometheus connector well: PromQL basics, instant versus range queries, how to find metric and label names, and how to read alerts and targets."
---

# Prometheus through Exorails

Tools: `query` (instant), `query_range`, `series`, `labels`, `targets`, `alerts`.

## Finding what exists

- `labels` with no name lists label names; with a name it lists that label's values (`job`, `instance`, `namespace`).
- `series` with a selector such as `{job="api"}` lists the series that match: the way to discover metric names for a job.

## Querying

- `query` answers "what is the value now": `sum(rate(http_requests_total[5m]))`.
- `query_range` answers "how did it evolve": same expression with a start, an end and a step. Keep the range and step proportionate; a day at a 15 s step is too many points.
- Counters must be wrapped in `rate()` or `increase()` over a window; a raw counter value is meaningless.
- Percentiles come from histograms: `histogram_quantile(0.95, sum(rate(x_bucket[5m])) by (le))`.

## Health

- `targets` shows which scrape targets are up. A target `down` explains missing metrics better than any query.
- `alerts` lists firing and pending alerts with their labels. Read them before diagnosing from raw metrics.

Results are bounded: aggregate with `sum by (…)` rather than returning thousands of series.
