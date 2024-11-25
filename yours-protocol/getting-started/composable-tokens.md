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

Let's extend our application with a second module, an \`avatars\` module.

```kts
// avatars/module.rell
module;

import equippables; // import our first module

import lib.yours;
```

```kts
// avatars/model.rell
entity avatar {
  key yours.token; // the avatar itself is a token as well
}

entity equipment {
  key avatar, equipabbles.equippable;
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
  val account = accounts.account @ { .id == to_account_id };
  yours.mint(token, yours.balance_info(account, 1L));
}
```

```
```
