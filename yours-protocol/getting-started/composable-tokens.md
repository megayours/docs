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

# Composable Tokens

Due to the fact that it up to you as a dapp developer to decide how you persist data for a token, you can actually achieve composability between different modules very easily.

Let's extend our application with a second module, an `avatars` module.

```kotlin
// avatars/module.rell
module;

import equippables; // import our first module
import lib.yours;
```

```kotlin
// avatars/model.rell
entity avatar {
  key yours.token; // the avatar itself is a token as well
}

entity equipment {
  key avatar, yours.token;
}
```

```kotlin
// avatars/functions.rell
function attach(token: yours.token) {
  yours.attach_module(token, rell.meta(avatar).module_name);
  create avatar(token);
}

function equip(avatar: avatar, item: yours.token) {
  require(equippables.get_equippable(item) != null, "Item must be equippable");
  create equipment(avatar, item);
}

function get_avatar(token: yours.token): avatar? {
  return avatar @? { token };
}
```

This logic above allows an avatar to equip 1 token of each equippable. But not the same equippable twice.

Now we can mint this token. I want an avatar to be a NFT, which means we will create a new token each time we create an avatar.

```kotlin
// avatars/operations.rell
operation mint(name, to_account_id: byte_array) {
  val account = ft4.auth.authenticate();

  val spec = yours.token_info(
    project = yours.project_info("Test Project", account.id),
    collection = "Avatars",
    name,
    modules = [rell.meta(mint).module_name]
  );

  val token = yours.create_token(spec);
  avatars.attach(token);
  
  val recipient = ft4.accounts.account @ { .id == to_account_id };
  yours.mint(token, yours.balance_info(recipient, 1));
}
```

## Equipping Items

Now that we have both modules set up, we can create operations to equip items:

```kotlin
// avatars/operations.rell
operation equip(
  avatar_uid: byte_array,
  equippable_uid: byte_array
) {
  val account = ft4.auth.authenticate();

  // Get the tokens
  val avatar_token = yours.get_active_token_by_uid(avatar_uid);
  val equippable_token = yours.get_active_token_by_uid(equippable_uid);
  
  // Verify ownership
  require(yours.get_balance(avatar_token, account) > 0, "Must own avatar");
  require(yours.get_balance(equippable_token, account) > 0, "Must own item");

  // Get the avatar and item
  val avatar = require(avatars.get_avatar(avatar_token));

  // Create equipment relationship
  avatars.equip(avatar, equippable_token);
}
```

## Next Steps

Now that you understand token composability:
- Learn about [making your tokens interoperable](making-your-tokens-interoperable.md)
- Explore more [module relationships](../modules/relationships.md)
- Add [metadata extensions](../metadata.md) to your modules
