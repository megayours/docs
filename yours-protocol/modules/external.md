# 👾 External

Yours Protocol allows your dapp to provide on-chain utility to tokens that have their ownership on external EVM blockchains. This section will describe how you can set that up. For more higher-level information about how the tokens are synced, see the [External Tokens](https://app.gitbook.com/o/jiDvljerSsgiwCActvEn/s/lUK5DbEH82eZXWYqi1RL/~/changes/20/external-tokens) documentation.

### Configuration

````yaml
```yaml
definitions:
  - &receiver # Base configuration for a chain that receives messages
    gtx:
      modules:
        - "net.postchain.d1.icmf.IcmfReceiverGTXModule"
    sync_ext:
      - "net.postchain.d1.icmf.IcmfReceiverSynchronizationInfrastructureExtension"

blockchains:
  megayours_tg:
    module: main
    config:
      <<: *receiver
      icmf:
        receiver:
          local:
            - bc-rid: x"0C034CAB6586CD4F38EAF56721905BA388762906CA75C0C67650F9AB2E69C7BC"
              topic: 'L_yours_external_amoy_c5f7f51e9de3b92a5f2ad9fd41c9e58c0cd2f2a6'
            - bc-rid: x"0C034CAB6586CD4F38EAF56721905BA388762906CA75C0C67650F9AB2E69C7BC"
              topic: 'L_yours_external_amoy_8d3a7c58f5b90b669e076ae4b01247c1561559b5'
            - bc-rid: x"0C034CAB6586CD4F38EAF56721905BA388762906CA75C0C67650F9AB2E69C7BC"
              topic: 'L_yours_external_amoy_7ddf0ca2e6ef5dac06ed79864e2cb40659b38401'
            - bc-rid: x"0C034CAB6586CD4F38EAF56721905BA388762906CA75C0C67650F9AB2E69C7BC"
              topic: 'L_yours_external_amoy_6721b9f19a1667e77107581ef79b9f2f106e81e0'
            - bc-rid: x"0C034CAB6586CD4F38EAF56721905BA388762906CA75C0C67650F9AB2E69C7BC"
              topic: 'L_yours_external_amoy_687b124886f203197ce68dc0ff14827dd13769ed'

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
  icmf:
    registry: https://gitlab.com/chromaway/core/directory-chain
    path: src/messaging/icmf
    tagOrBranch: 1.32.2
    rid: x"19D6BC28D527E6D2239843608486A84F44EDCD244E253616F13D1C65893F35F6"
  yours:
    registry: git@github.com:megayours/yours-protocol.git
    path: src/lib/yours
    tagOrBranch: main
    rid: x"B85DF44B4C7A4A94B871547EF5759E1356FCD19269419AF500A7586156B4B6D6"
    insecure: false
```
````

It is very easy to consume external tokens, the additional steps involved are:

1. Include the ICMF modules
2. Installing the icmf library
3. Configuring which external tokens you want to listen to

#### Contracts

The Gamma chain, which in our case here we specified with bc-rid `x"0C034CAB6586CD4F38EAF56721905BA388762906CA75C0C67650F9AB2E69C7BC"` broadcasts events for a contract on a topic basis.

In the example above we are subscribing to 5 NFT contracts and all of them on Amoy Polygon testnet.
