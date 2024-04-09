# DID format

<dl>
	<dt>Namespace</dt><dd>`social`</dd>
	<dt>Version</dt><dd>1.0</dd>
</dl>

This is an application format for describing how the communal social Namespace works.

## Encryption

Clients MUST respect the [encryption](encryption.md) format. Everything that follows is assumed to be after that's decrypted things as appropriate.

## Document prefix

Everything is prefixed using the [did format](did.md), followed by `social/1.0/`

e.g. `/did/1.0/did:key:z6MkhaXgBZDvotDkL5257faiztiGiC2QtKLGpbnnEGta2doK/social/1.0/...`

## DID level contents

`about.yaml` - follows the [about spec](about.md)

`feed/` - follows the [activities spec](activities.md). This is where you post your general social media... "stuff".
- Suggestion (only) - use `feed/public` and `feed/followers` with different capabilities set, so only followers can see things under that section.
- Suggestion (only) - use date-based paths for nice urls - e.g. `feed/public/2024/04/09/1711062719000.yaml`
- e.g. `/did/1.0/did:key:z6MkhaXgBZDvotDkL5257faiztiGiC2QtKLGpbnnEGta2doK/social/1.0/feed/public/2024/04/09/1711062719000.yaml`

`followers/` - follows the [trusted-dids spec](trusted-dids.md).
- e.g. `/did/1.0/did:key:z6MkhaXgBZDvotDkL5257faiztiGiC2QtKLGpbnnEGta2doK/social/1.0/followers/1.0/did:web:example.com.yaml`
