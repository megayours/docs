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

# 🧩 Modules

Modules are built by developers and they provide utility to tokens.

A token can implement multiple modules, and they can be linked both at mint time, or at a later time dynamically.

An example of this would be a token that can be placed on top of a grid on a piece of land in a game, in that scenario you might want to build a placeables module and attach that to the token.

```kotlin
// grids/model.rell
import lib.yours;

// grids/module.rell
entity grid {
  key yours.token;
  width: integer;
  height: integer;
}
```

```kotlin
// lands/model.rell
import lib.yours;
import grids;

// lands/module.rell
entity land {
  key yours.token;
  grids.grid;
}
```

```kotlin
// griddables/model.rell
import lib.yours;

// griddables/module.rell
entity griddable {
  key yours.token;
}

entity gridded {
  key griddable;
  key placed_on: yours.token, x: integer, y: integer;
}
```

In this example, we have defined three separate modules.

**Grids:** that defines a `grid` entity which is linked to a `token` with metadata on its `width` and `height`.

**Lands:** that defines a land entity which is linked to a `token` as well as a mandatory relationship to a `grid`. The reason for this is because we want a `land` to always have a `grid` on top of it where other tokens can be placed.

**Griddables:** that defines a `griddable` entity which is linked to a `token`. This entity declares that the `token` may be placed on a `grid`. The **griddables** module also has an entity for `griddable` tokens that is currently placed on a `grid`.&#x20;

