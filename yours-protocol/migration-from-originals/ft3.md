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

# FT3

Yours Protocol is based on FT4. However, we do provide a slimmed down version of Yours Protocol without cross-chain support which makes it easier to migrate for projects still using FT3.

The version with FT3 can be found on the `ft3` branch: [https://github.com/megayours/yours-protocol/tree/ft3](https://github.com/megayours/yours-protocol/tree/ft3)

```mermaid
graph TD
  subgraph "Dapp Using FT3"
    YOURS_FT3[Add Yours Protocol using FT3] --> R_ORIGINALS_FT3[Replace Originals]
    R_ORIGINALS_FT3 --> U_FT4[Upgrade to FT4]
    U_FT4 --> U_YOURS[Switch to Yours Protocol using FT4]
  end

  subgraph "Dapp Using FT4"
    YOURS_FT4[Add Yours Protocol using FT4] --> R_ORIGINALS_FT4[Replace Originals]
  end
```
