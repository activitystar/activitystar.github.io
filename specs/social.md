# DID format

<dl>
	<dt>Namespace</dt><dd>`social`</dd>
	<dt>Version</dt><dd>1.0</dd>
</dl>

This is an application format for describing how the communal social Namespace works.

## Encryption

Clients MUST respect the [encryption](encryption.md) format. Everything that follows is assumed to be after that's decrypted things as appropriate.

## Document prefixg

Most things are prefixed using `/social/`

## Contents

`feed/` - follows the [activities spec](activities.md). This is where you post your general social media... "stuff".
- Suggestion (only) - use `feed/public` and `feed/followers` with different capabilities set, so only followers can see things under that section.
- Suggestion (only) - use date-based paths for nice urls - e.g. `feed/public/2024/04/09/1711062719000.yaml`
- e.g. `/social/feed/public/2024/04/09/1711062719000.yaml`

`following/` - follows the [trusted-dids spec](trusted-dids.md).
- e.g. `/social/following/did:web:example.com.yaml`

## Additional extensions

At the top level, `/about/` should be present following the [about spec](about.md)
