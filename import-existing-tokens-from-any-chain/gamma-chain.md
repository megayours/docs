# 🛰️ Gamma Chain

### Register Contracts

Contracts can be added into the Gamma Chain by paying a fee in the Gamma Chain. By doing so, any dapps within the same cluster will be able to receive events regarding that NFT contract.

```bash
chr tx \
    --api-url <node_url> \
    --blockchain-rid <brid> \
    --ft-auth
    tokens.register_contract \
    <chain> \
    <contract> \
    <project_name> \
    <collection_name> \
    <block_height> \
    <type>
```

The transaction above registers a new token in the Gamma Chain with the arguments:

1. `chain`: which blockchain the contract is on, available options are:
   1. `ethereum`
   2. `polygon`
   3. `amoy`
2. `contract`: the address of the contract
3. `project_name`: the name of the project, e.g. `My Neighbor Alice`
4. `collection_name`: the name of the collection, e.g. `ELLE Mot's d'Amore`
5. `block_height`: the block height the event was created
6. `type`: the type of the contract, available options are:
   1. `erc721`
