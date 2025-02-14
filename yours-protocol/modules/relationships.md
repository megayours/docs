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

# 🌈 Relationships

When building modules in Yours Protocol, there are different ways to create relationships between them. Let's explore these patterns using a game example where characters can equip items.

## Direct Relationships

A direct relationship is when one module directly references another module's entities:

```kotlin
// modules/items/model.rell
entity item {
  key yours.token;
  mutable power: integer;
}

// modules/characters/model.rell
import items;

entity character {
  key yours.token;
  mutable experience_points: integer;
}

entity equipped_item {
  key character;
  key item: items.item; // Reference the token through the items module
  mutable slot: text;
}
```

**Avoid doing this!** While this approach works, it creates tight coupling between modules. The character module cannot exist without the items module, making it less reusable.

## Indirect Relationships

A better approach is to have modules reference tokens independently:

```kotlin
// modules/items/model.rell
entity item {
  key yours.token;
  mutable experience_points: integer;
}

// modules/characters/model.rell
entity character {
  key yours.token;
  key name;
  mutable level: integer;
}

entity equipped_item {
  key character;
  key item: yours.token; // Reference the underlying token and therefore create an indirect relationship
  mutable slot: text;
}
```

Here both modules are loosely coupled through the token. The benefits are:

1. **Loose Coupling:** The character module doesn't need to know about equippable entities
2. **Modularity:** Each module can be used independently
3. **Reusability:** The modules can be shared and used in different contexts

For example, the same item token could be:

* Equipped by a character
* Displayed in a marketplace
* Used in crafting
* Stored in a vault

Each of these features can be implemented as separate modules without direct dependencies.

## Best Practices

We highly recommend using the Indirect Relationship approach. The key features of Yours Protocol are flexibility, modularity, and reusability. It's important for modules to be shareable, which is much easier if each module is self-contained and doesn't directly depend on other modules.

A Direct Relationship may simplify querying slightly in certain scenarios, but we only recommend using it in modules that are specific to your dApp and ones that you do not intend to share or promote as reusable.
