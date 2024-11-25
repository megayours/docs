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

# Prototype

Prototypes are not a concept in Yours Protocol. Each token is responsible for holding all of its metadata.

However, this doesn't stop you as a dApp developer from having a prototype that you can mint stuff from.

```kotlin
// animals/model.rell
entity prototype {
  key species: name;
  habitat: string;
  diet: string;
  avg_lifespan: integer;
}

entity animal {
  key yours.token;
  prototype;
}

// animals/extensions.rell
@extend(yours.populate_metadata)
function populate_metadata(yours.token, modules: set<name>) {
  if (not modules.contains("animals")) return null; // ignore non-supported tokens
  val metadata = map<text, gtv>();

  val animal = animal @? { token }; // ignore not-known animal tokens
  if (animal == null) return null;

  // attach additional metadata
  metadata.put("species", animal.prototype.species.to_gtv());
  metadata.put("habitat", animal.prototype.habitat.to_gtv());
  metadata.put("diet", animal.prototype.diet.to_gtv());
  metadata.put("avg_lifespan", animal.prototype.diet.to_gtv());

  return metadata;
}
```

As you can see, the persistence layer is completely up to a dapp to decide how that looks like. When you bridge this token into another dApp then can decide to persist it in a completely different entity structure.
