# Finding Things

A store earns its keep at a hundred
entries, when memory stops working.
pass searches two different things, and
keeping them apart is most of this
chapter: the **names** of entries, which
are plain text on disk and cost nothing
to search — and the **contents**, which
are encrypted, so searching them means
decrypting everything.

## Searching names: find

`pass find` takes a fragment and prints
the part of the tree whose names contain
it (transcript
`pass-lab/transcripts/t08-find-grep.txt`):

```text
$ pass find example-b
Search Terms: example-b
`-- banking
    `-- example-bank
```

That is the tree program at work, pruned
to matches: folder, then entry. Naming
entries consistently — the advice from
chapter 1 — pays off here: a store where
every bank is under `banking/` and every
address reads `example-<something>` is a
store `find` can navigate. `pass ls` with
a folder name shows just that branch, and
plain `pass ls` (or just `pass`) shows
the whole tree — the catalogue from
chapter 2, drawn for real.

## Searching contents: grep

`pass grep` answers the other question:
which entry *mentions* this? It decrypts
every entry in the store — the agent
will want the passphrase if it has gone
cold — and runs your pattern over the
plain text, printing the matching lines
under their entry names:

```text
$ pass grep linen
wifi/home-router:
note: spare ethernet cable lives in the linen cupboard
```

One word from chapter 7's note, one
entry found. This is the audit tool as
much as a search tool: grep for an old
user name after an address change, or
for a reused hint you meant to retire,
and the store tells you everywhere it
lives. The cost is real, if modest:
every entry is decrypted, so on a big
store `grep` takes a moment, and the
plain text passes through memory on
your own machine — the same bargain as
editing in chapter 7.

## Reading just the password

When a script — or a login form over
your shoulder — wants the password and
not the notes, take line one only:

```text
$ pass show wifi/home-router | head -n 1
basement-router-88
```

That is the first-line rule from chapter
7 doing its job, and it is the idiom
chapter 13's recipes build on. Between
`find` for names, `grep` for contents,
and `head -n 1` for the password itself,
a store of several hundred entries stays
a few keystrokes deep.
