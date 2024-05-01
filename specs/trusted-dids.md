# Following format

<dl>
	<dt>Namespace</dt><dd>`trusted-dids`</dd>
	<dt>Version</dt><dd>1.0</dd>
</dl>

This is an application format for describing who you "trust". The exact semantics of this really depend on the application using this.

This is designed to be a sub-format - i.e. it should be used as part of something larger (e.g. nested inside the `did` spec, with a name like `following`)

## Path format

The path format of a trusted-dids tree is as follows

```
`{containing-path}/1.0/.../{DID}.yaml
```

Nesting etc is left up to the user - any leaf is a valid document.

This might be useful to separate out several classes of trusted-did (e.g. in a social application you wanted some public followings, and some ones you only show to your own followers.)

### Document fields

The `capability` property should be a serialised capability that will allow you to get more capabilities, following the `/capability/` spec.

(e.g. this is likely the `public` trusted capability - so no-one should have any qualms about you copying it from them and putting it here)

```
following:
  depth: 3 # 1 is just this identity, 2 is the ones they follow too, etc. (But recursive ones are the minimum of their depth and our depth. If `A-3->B-1->C-1->D`, then A can see B and C, but not D)
	from: UTC-TIMESTAMP # Time we're following this (and recursive identities) from. Optional
	from: UTC-TIMESTAMP # Time we're following this (and recursive identities) to. We'd expect to update this periodically. Allows for automatic pruning? Optional
```
