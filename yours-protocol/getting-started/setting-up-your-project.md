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

## Project Structure

A typical Yours Protocol project follows the standard Chromia project structure:
```
your-dapp/
├── chromia.yml           # Project configuration
├── src/
│   ├── your_module_a/    # Your custom modules
│   ├── your_module_b/    # Your custom modules
│   ├── libs/             # Your dependencies
│   │   ├── ft4/          # FT4 library
│   │   ├── iccf/         # ICCF library
│   │   └── yours/        # Yours Protocol library
│   └── main.rell         # Main application file
```

## Dependencies

Yours Protocol has two dependencies in order to be interoperable with other blockchains and dapps:

* **FT4** - Account management system for handling token ownership and transfers
* **ICCF** - Inter-Chain Communication Facility for cross-chain bridging and token interoperability

Add the following libraries to your `config.yaml`:

```yaml
libs:
  ft4:
    registry: https://gitlab.com/chromaway/ft4-lib.git
    path: rell/src/lib/ft4
    tagOrBranch: v1.0.1r
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

After adding the dependencies to your config file, install them using the Chromia CLI:

```bash
chr install
```

## Next Steps

With your project set up and dependencies installed, you're ready to:
1. [Create your first module](creating-your-first-module.md)
2. [Create and mint tokens](creating-and-minting-tokens.md)
3. [Make your tokens interoperable](making-your-tokens-interoperable.md)
