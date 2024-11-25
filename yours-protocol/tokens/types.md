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

# Types

Yours Protocol supports both non-fungible and semi-fungible tokens. The difference is simply how many tokens you create under the same project and collection.&#x20;

## Non-Fungible Tokens

Tokens that are non-fungible are unique per `token`, this means that under each `token`, there should maxiumum exist 1 `token_balance`.

```kotlin
function mint_non_fungible_token(name, to_account_id: byte_array) {
  val spec = yours.token_info(
    project = "Cat Dapp",
    collection = "Cats Collection",
    name,
    modules = [rell.meta(mint_non_fungible_token).module_name]
  );

  val token = yours.create_token(spec);
  val account = accounts.account @ { .id == to_account_id };
  yours.mint(token, yours.balance_info(account, 1L));
}
```

With the function above, each time we mint, it will create a new `token` under the same `collection`.

Assuming that we call this function three times.

```kotlin
mint_non_fungible_token("Bengal", x"DEADBEEF");
mint_non_fungible_token("Maine Coon", x"DEADBEEF");
mint_non_fungible_token("Ragdoll", x"DEADBEEF");
```

The tokens that would get created are the following.

<table><thead><tr><th>Project</th><th>Collection</th><th>Token ID</th><th>Token Name</th><th data-type="number">Token Balance</th></tr></thead><tbody><tr><td>Cat Dapp</td><td>Cats Collection</td><td>0</td><td>Bengal</td><td>1</td></tr><tr><td>Cat Dapp</td><td>Cats Collection</td><td>1</td><td>Maine Coon</td><td>1</td></tr><tr><td>Cat Dapp</td><td>Cats Collection</td><td>2</td><td>Ragdoll</td><td>1</td></tr></tbody></table>

## Semi-Fungible Tokens

Semi-Fungible Tokens can be shared among multiple users, and each user can own multiple of them. This means that we will end up with multiple `token_balance` under the same `token`.

```kotlin
function register_semi_fungible_token(name) {
  val spec = yours.token_info(
    project = "Cat Dapp",
    collection = "Cats Collection",
    name,
    modules = [rell.meta(mint_non_fungible_token).module_name]
  );

  val token = yours.create_token(spec);
}

function mint_semi_fungible_token(
  collection, 
  token_id, 
  to_account_id: byte_array, 
  amount: big_integer
) {
  val token = yours.get_token("Cat Dapp", collection, token_id);
  val account = accounts.account @ { .id == to_account_id };
  yours.mint(token, yours.balance_info(account, amount));
}
```

With the functions above, we will mint new balances of the provided token.

```kotlin
register_semi_fungible_token("Bengal");
register_semi_fungible_token("Maine Coon");
register_semi_fungible_token("Ragdoll");

mint_semi_fungible_token("Cats Collection", 0, x"DEADBEEF", 2L);
mint_semi_fungible_token("Cats Collection", 1, x"DEADBEEF", 1L);
mint_semi_fungible_token("Cats Collection", 2, x"DEADBEEF", 4L);
```

The tokens that would get created are the following.

<table><thead><tr><th>Project</th><th>Collection</th><th>Token ID</th><th>Token Name</th><th data-type="number">Token Balance</th></tr></thead><tbody><tr><td>Cat Dapp</td><td>Cats Collection</td><td>0</td><td>Bengal</td><td>2</td></tr><tr><td>Cat Dapp</td><td>Cats Collection</td><td>1</td><td>Maine Coon</td><td>1</td></tr><tr><td>Cat Dapp</td><td>Cats Collection</td><td>2</td><td>Ragdoll</td><td>4</td></tr></tbody></table>
