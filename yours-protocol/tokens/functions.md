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

# Functions

## Getters

```kotlin
// Gets a token, no matter if there exists a balance for it or not.
function get_token(project: name, collection: name, token_id: big_integer): token?
function get_token_by_uid(uid: text): token?

// Gets a token, but only if it exists at least a balance of 1 for it.
// External tokens may have metadata on-chain but the balance isn't here (yet).
function get_active_token(project: name, collection: name, token_id: big_integer): token?
function get_active_token_by_uid(uid: text): token?

// Returns the balance an account has of a token.
function get_balance(token, account: ft4.accounts.account): big_integer

// Returns the metadata of tokens in gtv format { name, properties, yours }.
function get_metdata(token): token_metadata_outgoing
```

## Creation

```kotlin
// Creates a new token with a pre-determined ID.
function create_token_with_id(spec: token_info, token_id: big_integer): token
// Creates a new token with an incremented ID based on the highest previous ID.
function create_token(spec: token_info): token
```

## Mint

```kotlin
function mint(token, spec: balance_specification)
```

## Burn

```kotlin
function burn(token, spec: balance_specification)
```

## Transfer

```kotlin
function transfer(token, spec: transfer_specification)
```

## Metadata

```kotlin
// Attaches (or removes if send null) an animation from a token.
function attach_animation(token, animation_url: text?): token_animation?
// Attaches (or removes if send null) a description from a token.
function attach_description(token, description: text?): token_description?
// Attaches (or removes if send null) an image from a token.
function attach_image(token, image_url: text?): token_image?
```
