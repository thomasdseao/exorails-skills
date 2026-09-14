---
name: "Redis through Exorails"
description: "Using the Exorails Redis connector well: scan instead of KEYS, read-only by default, what command runs where, and how to avoid blocking the server."
---

# Redis through Exorails

Tools: `get`, `scan`, `info`, `command`, and `set`, `del` when writes are allowed.

## Finding keys

Use `scan` with a pattern: it iterates in pages and never blocks the server. Do not run `KEYS *` through `command`; on a large instance it stalls every other client.

## Reading

- `get` reads a string key. For hashes, lists, sets and sorted sets use `command` with the read command (`HGETALL`, `LRANGE key 0 99`, `SMEMBERS`, `ZRANGE key 0 99 WITHSCORES`).
- Results are bounded; ask for ranges (`LRANGE key 0 99`) rather than whole structures.
- `info` returns server sections: memory, clients, keyspace. Use it to answer "how big is this Redis" before scanning.

## Writes

On a read-only server, writing commands are refused before they reach Redis. When writes are allowed, `set` and `del` exist, and `command` accepts writing commands: confirm with the user before `FLUSHDB`, `FLUSHALL`, or a `DEL` of a pattern's worth of keys.

## Do not

- Do not run `MONITOR`, `DEBUG`, `SAVE` or `BGREWRITEAOF`: they are operational commands the connector refuses.
- Do not store secrets or personal data the user did not ask you to store.
