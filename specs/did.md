# DID format

<dl>
	<dt>Namespace</dt><dd>`did`</dd>
	<dt>Version</dt><dd>1.0</dd>
</dl>

This is an application format for describing how information relating to a particular [Decentralized Identifiers (DID)](https://www.w3.org/TR/did-core/) is stored in an Earthstar share.

## Document prefix

The path format prefix (after any path decryption is performed) is:
```
/did/1.0/{DID}/...
```

e.g. `/did/1.0/did:cinn:@suzy.bu45zjhb73b4mt3ftrvu5maroabjadmy6vk6i74sqfwwqg4l2eqfq/...` or `/did/1.0/did:web:example.com/...`

Note this is a prefix - it's expected you'd use another application format under this structure to actually define the keys.

## Document paths

The document `/did/1.0/{DID}/did.json` may contain the [standard JSON representation](https://www.w3.org/TR/did-core/#json) of the DID Document.

If it exists, and the DID is of the `did:cinn` form and matches the subspace, and the namespace is a communal namespace, then this is to be treated as the canonical DID Document for that ID

TODO: Is this needed? Need to consider the use-cases here more.

## Why DIDs?

There are many reasons why people might need to rotate cryptographic identifiers (compromise, loss, privacy).

So, providing options that support this is important.

The existence of `did:cinn` means that if you just want to use your subspace ID, you can.

But other users might use [did:web](https://w3c-ccg.github.io/did-method-web/) or other choices.

This also allows for use of newer / better / different forms of ID, without changing the underlying protocol.

## did:cinn

`did:key` doesn't quite work - because `Cinn25519` needs the shortname too, so you can't just resolve an ed25519 key to the right subspace.

So - `did:cinn:@suzy.bu45zjhb73b4mt3ftrvu5maroabjadmy6vk6i74sqfwwqg4l2eqfq` does what you might intuitively expect, and gets translated to an Earthstar ID

## Resolution / Verification

Information found under a did prefix MUST NOT be trusted to belong to that did, without first [resolving](https://w3c-ccg.github.io/did-resolution/#resolving) the did (using the standard protocol for that did method) into a DID Document, and that DID Document must contain a service of type `Earthstar`, with serviceEndpoint `earthstar:<namespace>:<subspace>`. (Naturally, a did:cinn:<subspace> that matches the subspace it's in is always valid in a communal namespace).

Other implementations: atproto uses `alsoKnownAs` with an entry of the form `at://`
