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

# Creating Your First Module

A [module](https://docs.chromia.com/category/module-definitions) is a concept defined by the programming language Rell.

We use the terminology as well in Yours Protocol as a utility provider to a token. Let's assume that you have a token that represents a sweater. Now you want to allow this token to be equipped by an account. In that case, you could build an `equippables` module that integrates with Yours Protocol.

```kotlin
// equippables/module.rell
module;

import lib.yours;
```

Next, we will define the persistence layer for the utility that you want to provide. Here you have complete freedom as a developer to decide what metadata you want to associate with the token, but also how you persist that.

It is recommended to create an entity that references the underlying token tagged with the key attribute to ensure that it can only be created once for each token in your dapp. But this is also optional if you choose to go another direction.

```kotlin
// equippables/model.rell
entity equippable {
  key mega_yours.token;
}

entity occupying_slot {
  key equippable, name;
}
```

That's it. That's all you need to do to integrate with Yours Protocol. Now you can write logic in your dapp that utilizes these entities to provide use and provide dynamic metadata to the token.
