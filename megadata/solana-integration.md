# Solana Integration

### Programs

<table><thead><tr><th width="262.9921875">Network</th><th>Program ID</th></tr></thead><tbody><tr><td>devnet </td><td>yteDZqmquhCJwDKumf35uPqcknDn87xgcDLqdxjAigq</td></tr></tbody></table>

### SDK

A TypeScript SDK that integrates easily with Metaplex is available.

```bash
yarn add @megayours/megadata
```

The SDK is currently experimental and configured to use our program on Solana devnet. You can easily mint and update metadata by following the examples below.

#### Minting

```typescript
import { createNft } from '@metaplex-foundation/mpl-token-metadata';
import { percentAmount } from '@metaplex-foundation/umi'
import { createMegadata } from '@megayours/megadata/solana';

const megadataUri = await createMegadata({
  umi,
  mint,
  megadata: {
    "pfps": {
      "Hair": "Brown Textured Crop",
      "Length (CM)": "174"
    },
    "equippables": {
      "Hat": "Blue Cap",
      "Shirt": "Black T-Shirt",
      "Pants": "Shorts",
      "Shoes": "Brown Moccasins"
    }
  }
});

await createNft(umi, {
  mint,
  name: 'My NFT',
  uri: megadataUri,
  sellerFeeBasisPoints: percentAmount(5.5),
}).sendAndConfirm(umi);
```

#### Updating

```typescript
import { updateMegadata } from '@megayours/megadata/solana';

await updateMegadata({
  umi,
  mint,
  megadata: {
    "equippables": {
      "Hat": "Red Cap"
    }
  }
})
```

#### Token URI

The SDK will prepare and return a token URI which is unique for your token and serves as a gateway to the Megadata blockchain.&#x20;

```
https://router1.testnet.megayours.com/solana/4NjERDbp6GVPtoSjNmjjVNtEVscv67WGCPiVkFN9XW2i
```
