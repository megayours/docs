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

When we transfer this token from our dapp into another dapp, we want this metadata to follow along and be attached to the token in the receiving chains as well. This is done by extending and implementing the `populate_metadata` function. Therefore, we create an `extensions.rell` file.

```kotlin
@extend(yours.populate_metadata)
function populate_metadata(yours.token, modules: set<name>): map<text, gtv>() {
  if (not modules.contains(rell.meta(equippable).module_name)) return null;
  val metadata = map<text, gtv>();

  val equippable = equippable @? { token };
  if (equippable == null) return null;

  val slots = occupying_slot @* { .equippable.token == token } ( .name );
  if (not slots.empty()) {
    metadata.put("slots", slots.to_gtv());
  }

  return metadata;
}
```

Two parameters are passed into this function: the underlying token and the modules that this token has declared it supports.

You are expected to return `null` if the token does not support your module; otherwise, you waste some compute time by looking for utility that does not exist for this specific token.

This function is expected to return a `map<text, gtv>`. Everything that you put into this map will be included in the token's metadata under the attributes property. This is consistent and interoperable with the ERC721 Metadata Standard.

This same function also serves another purpose in populating your metadata for tokens into the query `yours.metadata(token_id)`.
