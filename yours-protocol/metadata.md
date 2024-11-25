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

# 📄 Metadata

Metadata and the format of it is important when you interopering with other dApps, as well as when you want your tokens to be understood by other clients.

Therefore, Yours Protocol enforces a specific schema on your metadata when you:

1. Transfer tokens to other dApps.
2. When a client wants to fetch information of tokens on your dApps.

In order for your module to include all its data into the metadata, all you have to do is to extend a function.

```kotlin
@extend(yours.populate_metadata)
function populate_metadata(yours.token, modules: set<name>) {
  if (not modules.contains("equippables")) return null; // ignore non-supported tokens
  val metadata = map<text, gtv>();

  val equippable = equippable @? { token }; // ignore not-known equippable tokens
  if (equippable == null) return null;

  // attach additional metadata
  val slots = occupying_slot @* { .equippable.token == token } ( .name );
  if (not slots.empty()) {
    metadata.put("slots", slots.to_gtv());
  }

  return metadata;
}
```
