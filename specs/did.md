# DIDs

This is a document pattern for associating [Decentralized Identifiers (DID)](https://www.w3.org/TR/did-core/) information with a particular identity (e.g. `@suzy.baaa...`).

This is an extension of the [about pattern](https://github.com/earthstar-project/document-patterns/blob/main/earthstar-community/about.md)


## Why DIDs?

There are many reasons why people might need to rotate cryptographic identifiers (compromise, loss, privacy).

So, providing options that support this is important.

The existence of `did:cinn25519` means that if you just want to use your subspace ID, you can.

But other users might use [did:web](https://w3c-ccg.github.io/did-method-web/) or other choices.

This also allows for use of newer / better / different forms of ID, without changing the underlying protocol.

## did:cinn25519

`did:key` doesn't quite work - because `Cinn25519` needs the shortname too, so you can't just resolve an ed25519 key to the right subspace.

So - `did:cinn25519:suzy.bu45zjhb73b4mt3ftrvu5maroabjadmy6vk6i74sqfwwqg4l2eqfq` does what you might intuitively expect, and gets translated to an Earthstar ID

## Canonical DID

### Document path

`about / did`

### Document payload

The did the identity desires to be considered the canonical DID for their identity, as UTF-8 encoded bytes.

e.g.

- `did:web:example.com`
- `did:cinn25519:suzy.bu45zjhb73b4mt3ftrvu5maroabjadmy6vk6i74sqfwwqg4l2eqfq`

This _MUST NOT_ be trusted without resolving the DID, as described below.

If this resolution cannot be completed and verified, then any data in this subspace _SHOULD_ be ignored.

## cinn25519 DID Document

### Document path

`about / did.json`

### Document payload

For a `did:cinn25519` method, this _SHOULD_ contain the [standard JSON representation](https://www.w3.org/TR/did-core/#json) of the DID Document.

i.e. the `id` field should match the subspace identity

If this doesn't exist, then one can be generated using similar methods to `did:key`

## Resolution / Verification

Information found in a subspace _MUST NOT_ be trusted to belong to that did, without first
- [resolving](https://w3c-ccg.github.io/did-resolution/#resolving) the did (using the standard protocol for that did method) into a DID Document.
- verifying that the DID Document contains an `alsoKnownAs` with the form `did:cinn25519:<subspace>` that matches the subspace.

### Communal vs Owned Namespaces

A did:cinn25519:<subspace> that matches the subspace it's in is always valid in a communal namespace

For an owned Namespace, this is more interesting. Applications may wish to verify that the identity used to create a document matches the subspace ID. This area needs future thought - but the primary use-case right now is for communal namespaces.

Other implementations: atproto uses `alsoKnownAs` with an entry of the form `at://`
