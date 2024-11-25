---
icon: screwdriver-wrench
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

# Setting up Your Project

## Project

## Dependencies

Yours Protocol has two dependencies in order to be interoperable with other blockchains and dapps.

* FT4 - for account management
* ICCF - for crosschain bridging

The following libs will be needed to be added to your config file.

```yaml
libs:
  ft4:
    registry: https://gitlab.com/chromaway/ft4-lib.git
    path: rell/src/lib/ft4
    tagOrBranch: v1.0.0r
    rid: x"FA487D75E63B6B58381F8D71E0700E69BEDEAD3A57D1E6C1A9ABB149FAC9E65F"
    insecure: false
  iccf:
    registry: https://gitlab.com/chromaway/core/directory-chain
    path: src/iccf
    tagOrBranch: 1.32.2
    rid: x"1D567580C717B91D2F188A4D786DB1D41501086B155A68303661D25364314A4D"
    insecure: false
  yours:
    registry: https://github.com/megayours/yours-protocol.git
    path: src/lib/yours
    tagOrBranch: main
    rid: x"---"
    insecure: false
```

After that, use the CLI to install the dependencies.

```bash
chr install
```
