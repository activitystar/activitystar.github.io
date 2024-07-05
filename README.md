# activitystar.github.io

ACTIVITY(pub|streams) + earthSTAR

...naming is hard, ok? (Naming suggestions welcome!)

## Goals

- offline/p2p first social media + chat
  - A person's data is primarily stored on their phone / laptop.
    - Data can be synced between their devices. Any device can publish.
    - They may optionally use some cloud-hosted relay/pub so others can get their data while they're offline.
  - A person's data can be shared selectively - it's not all or nothing, some things can be public, while others are only shared with friends.
    - You can help your friends content share - but only to people who're authorised to see it.
    - You don't need to keep *all* of your friend's data. You can throw some away, and resync it later if desired.
    - If you mark something as deleted, well-behaving clients will also delete it.
  - Any servers/relays/pubs that are used are not authoratitive. You can use more than one, switch between them. They don't have any power. Server operators can't inspect data
    - Though they may get metadata. e.g. if you publish something, and send it to a relay, they'll know when you did that etc.
- design specs suitable for multiple clients to interoperate

## Non-Goals

- Building the one true app

## Why activities as a base?

- I'm lazy, and I don't want to re-invent the wheel.
  - The low level syntax of activities etc seem fine, they cover all the data types we might want.
  - There's the potential for extension (create a new activity type, go!), so it doesn't feel like we're locked in.
- Possibility of easier bridging down the line

## Why different specs for different bits?

The aim here is to produce small independent specifications that build up to a greater whole. Some of these may be useful in other applications - and those things can be collaborated on, instead of everything needing to be built specifically for this application. It also allows for clients to swap in different bits if they only want to interopate with certain parts of the ecosystem - or to innovate with something better!

## Features, and the specs related to them

### Identity

One of the key pieces we need to solve is identity. Who is who, who do you follow/trust, etc?

tl;dr; Use an existing open specification for this - [DIDs](https://www.w3.org/TR/did-core/)

The details are covered in the [did format](specs/did.md).

### Privacy

We only want some people to see some of our content.

The [encryption format](specs/encryption.md) and [capabilities format](specs/capabilities.md) covers how your individual subspace can be set up to make things private, or not, as you desire.

Of note here - different clients / users may choose to do things in different ways. The main aim of the encryption spec is to be liberal in the types of things you can read (so you can interoperate with lots of clients), while being opinionated in what you produce.

### Discovery

Seeing only your own content does not a social media make.

Publishing your follows using the [trusted-dids format](specs/trusted-dids.md) lets you tell people who you follow. Or from the other perspective - when you're following someone, you now know who they follow, and can see their content too.

### Content

We haven't got anything about what content actually looks like yet - where are the activities?!

That's the [activity format](specs/activities.md). This is mostly a one to one mapping with activitystreams, adapted to match Earthstar semantics better.

### Pulling it all together - social media

So, how does all this work together?

That's the [social format](specs/social.md).

## So how is all this meant to work?

- You generate a DID (web based, key based, whatever you want)
  - This does at least need to map to an Earthstar key
- You publish content
- You follow someone (Need to get a copy of their public capability for this, to then get the other capabilities to read everything etc)
- Your client can now read their `following`, and get all that content too!
  - Repeat to N levels, default of say.. 2? 3? Friends of friends of friends.

### How does this compare to Mastodon?

A lot depends on how your client chooses to present things. One way could be:
- A "Home" timeline - posts from the people you follow, and optionally +N levels of their followings etc.
- An "Everything your client knows about" timeline - similar to the Federated timeline, all posts you know about regardless of level
- One or more "Community" timelines
  - The interesting approaches here are, like Mastodon, we anticipate multiple groups having their own "server". But like in SSB, this would be presented more as an identity that follows a bunch of people. So, it's moderated in that the owner of that community (identity) can unfollow people etc.
  - Unlike Mastodon, you can follow multiple of these, to get content from multiple communities in your feeds.
  - These could be presented as separate timelines, or your client could merge all of these into your Home feed - many options at the presentation layer.
