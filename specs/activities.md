# Activities format

<dl>
	<dt>Namespace</dt><dd>`activites`</dd>
	<dt>Version</dt><dd>1.0</dd>
</dl>

This is an application format for activities - stuff that happens!

This is designed to be a sub-format - i.e. it should be used as part of something larger (e.g. nested inside the `did` spec, with a name like `outbox`)

## Path format

The path format of an activities tree is as follows

```
`{containing-path}/1.0/.../{TIMESTAMP}.yaml
`{containing-path}/1.0/.../{TIMESTAMP}.{other-stuff}.yaml
```

Nesting etc is left up to the user - any leaf is a valid document.

This might be useful to separate out several classes of activities (e.g. in a social application you want to have public posts, and follower-only posts). It's easier to give different capabilities to different subtrees, than each individual post.

On the reading side - collect all activites you can understand and order by time, optionally including the context about where they came from.

### Document names

Should start with a timestamp, in milliseconds since the unix epoch.

Can optionally have more things after it. (E.g. a slug of the post).

Using just the timestamp discloses little extra - Willow already needs entry timestamps to function. Slugs might give away things if you don't use path encryption, but might be useful if you want nicer URLs and are publishing entries directly through HTTP too. Up to you!

### Document contents

YAML files following the [activity spec](https://www.w3.org/TR/activitystreams-core/#activities).

TODO: More formal definitions

e.g. for a [note](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-note):

```yaml
# @context: Is assumed to be "https://www.w3.org/ns/activitystreams"
type: Note
name: A Word of Warning
content: Looks like it is going to rain today. Bring an umbrella!
```

### Links

Linking to resources should be done using links based off DIDs.

e.g. If I wanted to link to post hosted at `/did/1.0/did:key:z6MkhaXgBZDvotDkL5257faiztiGiC2QtKLGpbnnEGta2doK/social/1.0/feed/public/2024/04/09/1711062719000.yaml`, I'd use the link `did:key:z6MkhaXgBZDvotDkL5257faiztiGiC2QtKLGpbnnEGta2doK/social/1.0/feed/public/2024/04/09/1711062719000.yaml`
