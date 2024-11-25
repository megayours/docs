---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🆔 Unique Identifier

A token can be identified by its project, collection, and token ID, which serves as a composite key. But it can also be identified by its unique identifier `uid`, which is a deterministic hash of its project, collection, and token ID.

When a token is created, its `uid` is generated and stored on-chain. The `uid` makes it easier for client applications to look up and target tokens due to it being a single value rather than three.

A token may be fetched by its `uid` using the `get_token_by_uid` and `get_active_token_by_uid` functions.
```kotlin
function get_token_by_uid(uid: text): token?
function get_active_token_by_uid(uid: text): token?
```
