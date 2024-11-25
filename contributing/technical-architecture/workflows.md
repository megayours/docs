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

# Workflows

## Import ERC721 Token

```mermaid
sequenceDiagram
    participant PLATFORM as Mega Platform
    participant CHAIN as Mega Chain
    participant META_GATEWAY as Mega Gateway
    participant NFT_OWNER as NFT Owner

    NFT_OWNER ->> PLATFORM: List Owned EVM Tokens
    NFT_OWNER ->> PLATFORM: Import Token
    PLATFORM ->> CHAIN: Import Request
    META_GATEWAY --> META_GATEWAY: Fetch Metadata
    META_GATEWAY ->> BASE_CHAIN: Create & Mint Token
```

## Token Creation

It's important to note that all tokens in Yours can have metadata attached to them. This means they all fungible tokens are actually semi-fungible tokens.

The determining factor if a token is non-fungible or semi-fungible is just a matter of how many tokens have been minted of it. If just 1 token is minted from each token, then it is considered non-fungible.

```mermaid
sequenceDiagram
    participant PLATFORM as Mega Platform
    participant BASE_CHAIN as Mega Chain
    participant NFT_CREATOR as NFT Creator

    NFT_CREATOR ->> PLATFORM: Create Token with Metadata
    PLATFORM ->> CHAIN: Define Token
    NFT_CREATOR ->> PLATFORM: Mint Token/s to Address/es
    PLATFORM ->> CHAIN: Mint Token/s to Address/es
```

## On-chain Metadata for ERC721 Tokens

One use-case that we support is to mint tokens on any EVM chain but have the metadata on Yours.

These tokens can later easily be bridged to Yours so that the tokens can utilize on-chain metadata.

```mermaid
sequenceDiagram
    participant PLATFORM as Mega Platform
    participant BASE_CHAIN as Mega Chain
    participant META_GATEWAY as Mega Gateway
    participant NFT_CREATOR as NFT Creator

    NFT_CREATOR ->> PLATFORM: Create Token with Metadata
    PLATFORM ->> BASE_CHAIN: Define Token
    NFT_CREATOR ->> PLATFORM: Mint Token/s
    PLATFORM ->> BASE_CHAIN: Mint Token/s to Locked Account
    NFT_CREATOR ->> META_GATEWAY: Visit URL, e.g. /${collectionId}/${id}
    META_GATEWAY ->> BASE_CHAIN: Fetch Metadata
    BASE_CHAIN ->> NFT_CREATOR: Return Metadata
```
