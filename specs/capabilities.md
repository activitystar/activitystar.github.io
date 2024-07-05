# Capability format

<dl>
	<dt>Namespace</dt><dd>`capabilities`</dd>
	<dt>Version</dt><dd>1.0</dd>
</dl>

This is an application format for continuous sharing and minting of capability information

## Required /encryption document settings

The following assumes that you've written the following documents:

`/encryption/1.0/capabilities/1.0/private/path.yaml`:
```
rules:
- key: self
  recursive: false
  type: per-key
```

`/encryption/1.0/capabilities/1.0/payload.yaml`:
```
rules:
- key: self
  recursive: true
  type: static
```

## Subspace

### Communal Namespaces

Capabilities are issued by an entity, and are therefore published under the issuing entities subspace.

### Owned Namespaces

The use of an owned Namespace depends on the owner - so they should set the rules on which subspace to use.
In practice - if you only have the ability to write in one subspace, you'd presumably publish the capabilities there.

## Public capabilities

The knowledge that you've issued capabilities to the receiver will be public for all. The contents, however, will be encrypted, so the capabilities / subspaces / paths they've been granted access to will not be public.

`/capabilities/1.0/public/{receiver-pubkey}/{id}`

## Private Capabilities

You may want to publish these entries to all (e.g. You'd like help from everyone in the network to distribute, because you don't have a direct peer with the recipient),
while not allowing people to know who you're actually granting these to.

More formally, given the issuer, a path a capability is published under, and a proposed receiver, it should not be possible for anyone else to know if that is the correct receiver.

`/capabilities/1.0/private/{receiver-pubkey}/{id}`

Because of the encryption settings above, this path gets encrypted and only the receiver knows it belongs to them.

## Capability document IDs

From a receiving perspective, these IDs are arbitrary. You want to read all of the ones under your pubkey's path.

From a writing perspective, if you're on a single device, you can keep updating the same document.

If you write from multiple devices - and in particular if you're doing automatic updates of these capabilities to extend the expiry - you may have issues with multiple devices overwriting each others updates.

A suggested scheme to handle this is: Read from all current capabilities, combine them, extend, then write to your own (persistent, but random) device ID.

## Payload format

Encryption happens under the hood, per the encryption definition above.

This should be a list of `McCapability` (TODO whatever the standard encoding of this is)

## Use cases

### week-by-week caps

Imagine you want to grant someone access to your social media posts (e.g. a private Twitter account), but you want to be able to revoke this in future. Granting someone an infinite length of time in a capability prevents you from ever being able to revoke this access. But while giving someone a capability document once is feasible, being able to continually do this while you're both offline is a challenge.

The solution is to grant two capabilities, first:

```
CommunalCapability:
  access_mode: read
  namespace_key: gardening.xxx...
  user_key: UserPublicKey
  delegations:
  - times: forever
    path: /capabilities/1.0/private/{their-bit}...
    subspace: you
```

This will allow them forever access - but only to capabilities specific to them.

Then you can put the actual week by week capability in there. Each time you make an entry, or regularly on expiry (whichever makes sense for your app), overwrite the entry at that path with a new capability, extending the time. This prevents there being lots of documents, and the first thing they can do on sync is get the latest capability, and then use that to sync all entries in the past. This could likely be done by the same client you're using to make the entries - if you aren't making you entries, the existing caps are sufficient. If you make an entry outside of the current end time, you'll want to update them all. Any peer will get the updated caps in the same sync as the new entries, whether that's a direct peer or via some pub/relay.

To stop someone accessing future Entries, stop updating the capability (and optionally tombstone the old one).

### Granting public access

For this to work, we generally want to get our capabilities into the hands of our desired recipient, but how do we bootstrap this? People can't request an entity without proving a right to receive it first. For online things, your website could mint the desired capabilitiy entry for their key - but we have the same problem as above for week-by-week caps, needing to update it continually. This means you need to know all your followers. For a generally-public account, this may be undesirable.

Grant a capability as follows:

```
CommunalCapability:
  access_mode: read
  namespace_key: gardening.xxx...
  user_key: btnaix46fptu7nj4hwhkusutly6vhjgbigi6ewnapykza66ucf24a
  delegations:
  - times: forever
    path: /capabilities/1.0/public/btnaix46fptu7nj4hwhkusutly6vhjgbigi6ewnapykza66ucf24a/default
    subspace: you
```

Publish this on your website, in your bio, etc.

This public key matches the secret key `bhxh7emac7wtpbwqog4k24kfgmvjwoejexb5523g6mg7tzi7uyiwa`

This will allow anyone who knows the secret (i.e. everyone. This could well be encoded into client apps by default) to sync the public-access capability.

You can then put the week-by-week caps into `/capabilities/1.0/public/btnaix46fptu7nj4hwhkusutly6vhjgbigi6ewnapykza66ucf24a/default`, for all the content you want to make public. This will sync to everyone else who's opted-in to syncing the public capability Namespace.
