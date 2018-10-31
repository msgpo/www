---
title: App architecture | Documentation
layout: documentation
permalink: 'docs/architecture/'
---

# App architecture
{% include toc.md %}

<ul class="uk-breadcrumb uk-padding-small uk-padding-remove-vertical uk-padding-remove-right">
<li><a href="/docs">Documentation</a></li>
<li>App architecture</li>
</ul>

Let's go over the basics of how Turtl works.

## Sharing

Sharing allows you to collaborate on a board with someone else. The general
idea is this: each note has a unique key that decrypts it. This key is
encrypted with the key of any space the note is in, and this encrypted key is
stored in the note's data. So if you have an encrypted note, and you have the
key of a space that note is in, you can decrypt the note. Giving each object
in Turtl a unique key allows sharing only specific data with any set of people
without compromising the master key of your account.

Sharing is done only on a space basis. You cannot share single notes. You can,
however, put a single note into a space and share that space.

## Syncing

Whenever you change data in your profile, the changed objects are encrypted,
saved to local storage, and the syncing system is notified of the
change. All changed objects are saved in a local syncing table, which the
sync system periodically reads. If it finds changes, it sends them up to the
Turtl server, in order, using a bulk send (all items are sent at once).

At the same time, the sync system polls the API for incoming changes to
your profile. This can happen when you use Turtl on another device, or if a
shared space you're a member of has changes on it. Then the reverse happens:
the changes are pulled down, saved (encrypted) to the local storage, and the
app is notified of the changes (at which point it loads the changes into
memory, decrypted, if it needs to).

If at any point Turtl gets disconnected, it holds outgoing sync data
indefinitely until it gets a connection. What this means is that after you log
in (which requires a connection), you can operate completely in offline mode
until you are connected again at which point all the changes you've made will
be synced.

Interested in how this all works in detail? [Read more about Turtl's syncing
system &raquo;](/docs/syncing)

