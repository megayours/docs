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

# 🔌 Attaching Modules

After creating your modules, you need to tell Yours Protocol which modules a token should implement. This can be done either at token creation time or later when the token already exists.

## Token Creation

When creating a new token, specify which modules it should support:

```kotlin
// Create a character that can equip items
operation create_character(name: text) {
  val account = ft4.auth.authenticate();
  
  // Create token with character module
  val spec = yours.token_specification(
    project = yours.project_info("Game Example", chain_context.blockchain_rid),
    collection = "Characters",
    name,
    modules = ["characters"]  // Specify modules at creation
  );

  val token = yours.create_token(spec);
  characters.attach(token, name);  // Initialize character data
}

// Create an equippable item
operation create_item(name: text, power: integer) {
  val account = ft4.auth.authenticate();
  
  // Create token with item module
  val spec = yours.token_specification(
    project = yours.project_info("Game Example", chain_context.blockchain_rid),
    collection = "Items",
    name,
    modules = ["items"]  // Specify modules at creation
  );

  val token = yours.create_token(spec);
  items.attach(token, power);  // Initialize item data
}
```

## Adding Modules Later

You can also add module support to existing tokens:

```kotlin
// Make an existing token equippable
operation make_item(token: yours.token, power: integer) {
  val account = ft4.auth.authenticate();
  
  // Add items module to token
  items.attach(token, power);
}
```

## Best Practices

### Attachment

Provide helper functions in your modules to handle the attachment process:

```kotlin
// modules/items/functions.rell
function attach(token: yours.token, power: integer) {
  // Initialize item data
  yours.attach_module(token, "items");
  create item(token, power);
}

// modules/characters/functions.rell
function attach(token: yours.token, name: text) {
  yours.attach_module(token, "characters");
  // Initialize character data
  create character(token, name, experience_points = 0);
}
```

These helper functions ensure that:

1. All required entities are created
2. Data is initialized correctly
3. Module attachment is consistent

### Declaring Module



