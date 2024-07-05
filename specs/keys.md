# Encryption format

<dl>
	<dt>Namespace</dt><dd>`keys`</dd>
	<dt>Version</dt><dd>1.0</dd>
</dl>

This is an application format for distributed encryption keys to other parties.

It's intended that this be handled automatically by the Earthstar libraries, so applications don't need to thing about the details here.

These keys are intended to be used by the [encryption spec](encryption.md)

### Required /encryption document settings

The following assumes that you've written the following documents:

`/encryption/1.0/keys/1.0/path.yaml`:
```
rules:
- key: self
  recursive: false
  type: per-key
```

`/encryption/1.0/keys/1.0/payload.yaml`:
```
rules:
- key: self
  recursive: true
  type: static
```

### Keys document

The path format of a keys document is:

```
/keys/1.0/<ed25519 public key>/<random-key>
```

The contents are:

```
key: <encryption key>
type: AES-256
```

### Usage

Imagine Alice wants to give a key `swordfish` to bob with id `123`:

She writes the document payload:
```
id: 123
key: swordfish
type: AES-256
```

to the path `/keys/1.0/<bob-public-key>/123` in the `<alice-subspace>`

Under the hood Earthstar, from the above encryption spec, encrypts it using Bob's public key (The `self` refers to the person decrypting - TODO figure out a better magic parameter?)
And the path written to is actually `/keys/1.0/{hkdf(key=scalarmult(<alice-private-key>, <bob-public-key>), info=<namespace>#<alice-subspace>#<path>)}/123`

This has some useful properties:
- We've not disclosed who the key is for
- We've not disclosed what the key is for (The key IDs are generally made public in the /encryption spec)
- Only Bob can decrypt it
