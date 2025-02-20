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

A [module](https://docs.chromia.com/rell/modules) is a concept defined by the programming language Rell.

We use modules in Yours Protocol as utility providers for tokens. Let's create a simple example: imagine you have a token that represents a sweater, and you want to allow this token to be equipped by an account. For this, we'll build an `equippables` module.

## Module Definition

First, create your module file - assuming you followed the instruction of the [Setting up Your Project](setting-up-your-project.md) - it should be under `rell/src/`.

```kotlin
// modules/equippables/module.rell
module;

import lib.yours;
```

## Data Model

Next, define the persistence layer for your utility. Here you have complete freedom as a developer to decide what metadata you want to associate with the token and how you want to persist it.

In this step we will create an equippable entity and specify that an equippable entity is linked to one Yours Token.&#x20;

The occupying slot will specify what slot (e.g. hat, torso, bottom) the equippable will occupy.

<pre class="language-kotlin"><code class="lang-kotlin">// modules/equippables/model.rell
<strong>entity equippable {
</strong>  key yours.token;  // Reference to the underlying Yours token
}

entity occupying_slot {
  key equippable, name: text;  // Specifies which slots this equippable occupies
}
</code></pre>

{% hint style="info" %}
It's recommended to create an entity that references the underlying token with the `key` attribute. This ensures it can only be created once for each token in your dapp. However, this is optional based on your specific needs.
{% endhint %}

Certainly `equippable`and `occupying_slot`entities could be normalized in one unique entity, for the simplicity of the tutorial, we prefer to keep them separate for now.

## Basic Functions

Add some basic functionality by attaching your module to the token:

```kotlin
// modules/equippables/functions.rell
function attach(token: yours.token, slots: list<text>) {
  yours.attach_module(token, rell.meta(equippable).module_name);
  val equippable = create equippable(token);
  for (slot in slots) {
    create occupying_slot(equippable, slot);
  }
}

function get_slots(token: yours.token): list<text> {
  return occupying_slot @* { .equippable.token == token } ( .name );
}

function get_equippable(token: yours.token): equippable? {
  return equippable @? { .token == token };
}
```

That's all you need to provide utility to a token with Yours Protocol! You can now build additional logic in your dapp that utilizes these entities to provide functionality and dynamic metadata to your tokens.
