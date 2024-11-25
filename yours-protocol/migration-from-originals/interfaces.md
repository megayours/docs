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

# Interfaces

Originals is interface centric. A token has a direct interface, the most direct interface that you minted the token from. Each interface has a set of attributes attached to it. Each interface can have a dependency to multiple interfaces.

Let's assume the following structure.

```
IPlot
  depends on |
    INonFungibleOriginal
    IGriddable
    IERC721
  attributes |
    plot_id
    island
    region
    water_quality
    soil_fertility
    soil_type
    bjorn_extracted
```

How could we represent that with **Yours Protocol**?

```kotlin
// griddables/model.rell
// intentionally kept outside yours context (no key yours.token) because
// metadata about positioning in a region in My Neighbor Alice
// is not data that should be transmitted crosschain.
entity gridded {
  key id: integer; // could be a region, plot id or another placeable
  x_left: integer;
  y_bottom: integer;
  width: integer;
  height: integer;
}

// plots/model.rell
entity plot {
  key yours.token;
  key gridded; // this would be the position on the region map
  island: text;
  region: text;
  water_quality: decimal;
  water_type: text;
  soil_fertility: decimal;
  soil_type: text;
  mutable bjorn_extracted: integer;
}

// erc721/model.rell
entity erc721 {
  key yours.token;
  contract_address: text;
  chain_id: text;

  index chain_id, contract_address, token;
}
```

With the above, we have two entities that have a direct relationship `plot` -> `gridded`.

But we also introduce an optional relationship from `plot` -> `erc721`.&#x20;

* This is achieved by creating a separate `erc721` entity whenever a `plot` has its ownership represented on `Binance Smart Chain`, e.g. sold plots.
* But the reason why this would be optional is because not all plots will be sold on `Binance Smart Chain`. E.g. My Neighbor Alice has lands which are not sold, and lands that are not `erc721`. In that scenario, they don't need to be associated with a `chain_id` and `contract_address`.
