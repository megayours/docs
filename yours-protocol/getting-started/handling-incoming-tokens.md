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

# Handling Incoming Tokens

It is optional if you want to utilize the metadata that was attached to the token when you receive a token from another dapp. No matter if you choose to use the metadata or not, the data is still persisted by Yours Protocol so that it is not lost when sending the token to another dapp blockchain.

However, if you want to utilize the metadata that was attached to the token, you can do so by implementing the `after_apply_transfer` function in your receiving dapp.

```kotlin
@extend(yours.after_apply_transfer)
function after_apply_transfer(yours.token, modules: set<name>, attributes: map<text, gtv>) {
  if (not modules.contains(rell.meta(equippable).module_name)) return;

  val equippable = equippable @? { token } ?: create equippable(token);
  val slots = list<name>.from_gtv(attributes.get("slots"));
  for (slot in slots) {
    val occupying_slot = occupying_slot @? { equippable, slot } ?: create occupying_slot(equippable, slot);
  }
}
```

Three parameters are passed into this function: the underlying `token`, the `modules` that this `token` has declared it supports, and the `attributes` that were attached to the `token`.

Here it is up to you to decide which attributes you want to map to entities in your own dapp in order to utilize.
