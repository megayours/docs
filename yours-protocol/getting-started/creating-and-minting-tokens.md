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

# Creating & Minting Tokens

Not every dapp needs minting support - you might want to only interact with existing tokens. However, if you want to create new assets, you'll need to understand token creation and minting in Yours Protocol.

## Token Creation

To create a new token, you'll need to define its specifications and attach any modules you want it to support.

```kotlin
// modules/equippables/operations.rell
operation create_equippable(
  collection: text,
  name: text,
  slots: list<text>
) {
  // Authenticate the caller
  val account = ft4.auth.authenticate();
  
  // Create token specification
  val spec = yours.token_specification(
    project = yours.project_info("Example Dapp", chain_context.blockchain_rid)
    collection,
    name,
    modules = [rell.meta(equippable).module_name]
  );

  // Create the token
  val token = yours.create_token(spec);
  
  // Attach our equippable module
  equippables.attach(token, slots);
}
```

## Minting Tokens

After creating a token definition, you can mint instances of it to specific accounts:

```kotlin
operation mint_equippable(
  token: yours.token,
  to_account_id: byte_array,
  amount: integer = 1
) {
  // Authenticate the minting request
  val account = ft4.auth.authenticate();
  
  // Get the recipient account
  val recipient = ft4.accounts.account @ { .id == to_account_id };
  
  // Mint the token
  yours.mint(token, yours.balance_info(recipient, amount));
}
```

## Understanding Token Types

In Yours Protocol, the distinction between fungible and non-fungible tokens is determined by how you use them:

* **Non-Fungible**: Create unique tokens by minting only one instance
* **Semi-Fungible**: Create multiple instances of the same token

For example, a unique piece of art would be minted with amount=1, while a game item might have multiple copies.

## Best Practices

When creating tokens:

1. Always authenticate operations
2. Validate input parameters
3. Consider which modules to attach
4. Plan your token distribution strategy
