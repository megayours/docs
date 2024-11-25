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

Not every dapp will need minting support. It's only if you want to create your own tokens instead of re-using another dapp's `token`.

If you do want to create a new asset, that can be done by defining a new `token`.

<pre class="language-rust"><code class="lang-rust"><strong>// equippables/operations.rell
</strong><strong>operation register(
</strong><strong>  collection: text, 
</strong><strong>  name, 
</strong><strong>  symbol: text, 
</strong><strong>  slots: list&#x3C;text>, 
</strong><strong>  image_url: text, 
</strong><strong>  animation_url: text
</strong><strong>) {
</strong><strong>  ft4.auth.authenticate();
</strong><strong>  
</strong><strong>  val project = yours.project_info("Test Project", chain_context.blockchain_rid);
</strong><strong>
</strong>  val spec = yours.token_info(
    project,
    collection,
    name,
    modules = [rell.meta(register).module_name]
  );

  val token = yours.create_token(spec);
  attach(token, slots);
}

// equippables/functions.rell
// recommended to provide an attach function that other modules can call
// to keep the attaching logic contained in the module that is being attached
function attach(token, slots) {
  val equippable = create equippable(token);
  for (slot in slots) {
    create occupying_slot(equippable, slot);
  }
}
</code></pre>

There are a couple of optional things you can do here in terms of generic metadata. In the example above, we attach an `animation_url`. You can also attach a `description` and `image` of your `token` if you want to.

Below that, we create a new `equippable` entity and `occupying_slot` entities for each slot that was passed in, which is specific for your dapp and how you choose to represent the `token`.

What we have done here is to define what this is. We have defined that our token, with a specific id (in this scenario auto-incremented) is a token which is an `equippable` item that occupies a list of `slots`.

The next step after that would be to mint balances of this `token`. Each balance minted of this `token` will be according to the `token` definition, in our scenario it is an `equippable`.

```kotlin
// equippables/operations.rell
operation mint(
  project: name,
  collection: text, 
  token_id: integer, 
  amount: integer, 
  to_account_id: byte_array
) {
  ft4.auth.authenticate();

  val token = yours.get_token(project, collection, token_id);
  val account = accounts.account @ { .id == to_account_id };
  yours.mint(token, yours.balance_info(account, amount));
}
```
