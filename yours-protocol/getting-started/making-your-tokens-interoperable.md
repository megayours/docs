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

# Making Your Tokens Interoperable

Since you are integrating a token standard, it's pretty safe to assume that you want to integrate with other dapps and blockchains as well. Let's have a look at how that works.

In our module above, we declared utility in that an equippable has `1..*` occupying slots. For example, a sweater might occupy the "torso" slot, and a cap might occupy the "hat" slot.

When we transfer this token from our dapp into another dapp, we want this metadata to follow along and be attached to the token in the receiving chains as well. This is done by extending and implementing the `populate_metadata` function:

```kotlin
@extend(yours.populate_metadata)
function populate_metadata(yours.token, modules: set<name>): map<text, gtv>() {
  val metadata = map<text, gtv>();
  if (not modules.contains(rell.meta(equippable).module_name)) return metadata;

  val equippable = equippable @? { token };
  if (equippable == null) return null;

  val slots = occupying_slot @* { .equippable.token == token } ( .name );
  if (not slots.empty()) {
    metadata.put("slots", slots.to_gtv());
  }

  return metadata;
}
```

Two parameters are passed into this function:
- The underlying token
- The modules that this token has declared it supports

You are expected to return an empty map if the token does not support your module; otherwise, you waste compute time looking for utility that does not exist for this specific token.

This function returns a `map<text, gtv>`. Everything you put into this map will be included in the token's metadata under the attributes property. This is consistent and interoperable with the ERC721 Metadata Standard.

## Handling Incoming Tokens

When receiving tokens from other dapps, you can choose to utilize their metadata by implementing the `after_apply_transfer` function:

```kotlin
@extend(yours.after_apply_transfer)
function after_apply_transfer(yours.token, modules: set<name>, attributes: map<text, gtv>) {
  if (not modules.contains(rell.meta(equippable).module_name)) return;

  val equippable = equippable @? { token } ?: create equippable(token);
  val slots = list<text>.from_gtv(attributes.get("slots"));
  for (slot in slots) {
    val _ = occupying_slot @? { equippable, slot } ?: create occupying_slot(equippable, slot);
  }
}
```

It's optional if you want to utilize the metadata that was attached to the token when you receive it from another dapp. No matter if you choose to use the metadata or not, the data is still persisted by Yours Protocol so that it is not lost when sending the token to another dapp blockchain.

## Next Steps

- Learn about [advanced module relationships](../modules/relationships.md)
- Explore [metadata extensions](../metadata.md)
- Understand [cross-chain functionality](../interoperability.md)