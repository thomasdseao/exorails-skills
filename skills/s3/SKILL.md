---
name: "S3 guide"
description: "Using the Exorails S3 connector well: list before reading, prefixes instead of listing everything, bounded object reads, and confirmation before put_object or delete_object."
---

# S3

Tools: `list_buckets`, `list_objects`, `stat_object`, `get_object`, and `put_object`, `delete_object` when writes are allowed. Works with AWS S3 and any S3-compatible store (MinIO, R2, Backblaze).

## Finding objects

- `list_objects` takes a bucket and a prefix. Always give a prefix; listing a whole bucket is slow and the result is capped.
- Keys are paths by convention only; `photos/2026/` is a prefix, not a directory.
- `stat_object` returns size, content type and last modification without reading the object: use it before `get_object` on anything that might be large.

## Reading

`get_object` returns the content, bounded in size. Text and JSON come back readable; binary content is summarised. For a large file, tell the user what you can and cannot read rather than retrying.

## Writes

`put_object` and `delete_object` exist only when writes are allowed. Deleting is permanent unless the bucket has versioning: show the exact key and wait for a yes. Never overwrite an object you did not just read without telling the user.

## Do not

Do not guess bucket names. Do not reveal credentials or presigned URLs; the connector never returns them.
