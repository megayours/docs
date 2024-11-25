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

After you have built your modules, it is crucial that you instruct Yours Protocol that you want a token to implement a module. There are two ways to do this, at token creation time, but also later when the token already has existed.

## Token Creation

```kotlin
val spec = yours.token_specification(
  project = "Example Dapp",
  collection,
  name,
  modules = ["grids", "lands"]
);

val token = yours.create_token(spec);
val grid = create grids.grid(token, width, height);
val land = create lands.land(token, grid);
```

## Existing Token

```kotlin
yours.attach_module(token, "placeables");
```

## Best Practices

In your module, provide a attach module that the caller can call which does all the logic which is needed in order to fully attach.

```kotlin
// lands/functions.rell
function attach(yours.token, width: integer, height: integer) {
  val grid = create grid(token, width, height);
  val land = create land(token, grid);
}
```
