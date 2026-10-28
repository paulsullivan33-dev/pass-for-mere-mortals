# pass for Mere Mortals

Every account you own wants a password,
and the passwords are supposed to be
different, long, and impossible to
remember. **pass** — its full name is
*the standard Unix password manager* —
answers with an idea old enough to be
boring: **one file per password**, each
file encrypted with a key only you hold,
all of them in an ordinary folder you
can list, copy, back up and inspect.
There is no account to sign up for, no
server that must be running, and no
database format you need a special tool
to read. The program itself is a shell
script that drives three tools you may
already have: **GPG** does the encrypting,
the **filesystem** does the storing, and
**Git** — if you ask it to — remembers
every change.

This guide explains how that works, how
to make the GPG key everything depends
on, how to start a store and put your
first passwords in it, how pass generates
passwords you could never invent, how an
entry can carry notes beside the password,
how to find one password among hundreds,
and how a store can be tracked in Git and
shared, one folder at a time, with a
second person. The limits are stated
plainly, including the two that surprise
people: the *names* of your passwords are
not encrypted, and the one key opens
everything, so losing it loses it all.

One rule governs the lab behind this
guide, and chapter 1 states it before
anything depends on it: **every password
in these pages was invented for the lab**,
for services that do not exist, under a
key that was created for the occasion and
never used for anything else. That is
what makes the transcripts safe to print
in full — `pass show` really printing a
password — and it is the habit to copy:
practice on invented secrets, never on
real ones.

The lab ran the real thing: **pass
1.7.4** (the current Ubuntu package),
with **GnuPG 2.4.4**, **Git 2.43.0** and
**tree 2.1.1**, on a small Linux machine,
inside a throwaway home directory built
for the purpose. Against it the lab made
a key, initialised a store, typed and
generated passwords, read the encrypted
files on disk without decrypting them —
and with `grep`, proved the passwords are
not in there in any readable form — then
decrypted one file with plain `gpg` to
show that pass is a wrapper, not a vault.
It edited entries, moved and copied and
deleted them, searched names and
contents, turned on Git tracking (and
captured the first attempt failing,
because Git had not been told whose name
to put on the commits, while `pass git
init` reported success anyway), pushed
the store to a local Git remote, and
cloned it back somewhere else. It shared
the store with a second key — capturing
the first attempt failing *silently*, the
files still readable by only one key
while the paperwork claimed two — fixed
the cause, and proved with packet-level
evidence and the second key itself that
the sharing then worked. It also recorded
the failure modes on purpose: a wrong
passphrase, a missing secret key, a
corrupted file, and a clipboard with no
display to copy to.

What the lab could not do, it did not
fake. The machine has no graphical
display, so the clipboard chapter's
success story — copy a password, paste
it, watch it clear itself — is taught
from the manual and labelled as such;
only its failure on a display-less
machine was run. Signing up for hosted
Git remotes, synchronising phones, and
the extension ecosystem are likewise
described, not demonstrated.
`criteria.md` keeps the full ledger of
what was run and what was only read.

## What this guide covers

- [**What pass is**](what-pass-is.md) —
  the mental model: a folder of encrypted
  files, one key, and why the boring
  design is the point
- [**The moving parts**](the-moving-parts.md) —
  the store, the `.gpg-id` file, your key
  and the agent that holds its passphrase,
  and which part actually keeps the secret
- [**Installing pass**](installing-pass.md) —
  one package and its three companions,
  and how to check the install worked
- [**A key of your own**](a-key-of-your-own.md) —
  making the GPG key pair everything
  depends on, its fingerprint, and the
  passphrase that guards it
- [**Your first store**](your-first-store.md) —
  `pass init`, your first typed password,
  showing it again, and the error that
  eats a mismatched pair
- [**Generating passwords**](generating-passwords.md) —
  passwords invented by the machine:
  length, symbols or none, and the one
  time pass shows you the result
- [**Entries with more inside**](entries-with-more-inside.md) —
  the first line is the password; the
  rest of the file is yours — user names,
  addresses, notes — and editing them
- [**Finding things**](finding-things.md) —
  searching entry *names* with `find`,
  searching their *contents* with `grep`,
  and reading the store as a tree
- [**Moving things around**](moving-things-around.md) —
  rename, copy and delete, and the
  confirmation that stands in front of
  the bin
- [**The store in Git**](the-store-in-git.md) —
  a commit for every change, the identity
  failure that reports success, pushing to
  a remote, and cloning your passwords
  onto a second machine
- [**Sharing a store**](sharing-a-store.md) —
  a second key on the store: the attempt
  that silently did nothing, why GPG
  refused, and the two-recipient proof
- [**The clipboard**](the-clipboard.md) —
  copying without showing, the 45-second
  self-clearing trick, and what happens
  with no display — plus extensions
- [**Recipes**](recipes.md) — a second
  store for work, moving to a new machine,
  passwords inside scripts, and an audit
  with `grep`
- [**The neighbours**](the-neighbours.md) —
  KeePassXC, Bitwarden and Vaultwarden,
  gopass, and the browser's own manager:
  what each does that pass doesn't, and
  vice versa
- [**Limits**](limits.md) — the names are
  not encrypted, the key is a single point
  of total failure, Git never forgets an
  old password, and other edges, stated
  plainly
- [**Troubleshooting**](troubleshooting.md) —
  "No secret key", "Bad passphrase", the
  checksum error, the git failure that
  exits 0, and the traps this guide's lab
  fell into so you don't have to
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Recovering a store whose key or
passphrase is lost — there is no such
chapter because there is no such
procedure; chapter 15 explains why that
is a property, not an oversight. Cracking
or attacking other people's stores,
stealing clipboard contents, and key
management for whole companies are out.
Mobile apps and browser autofill are
pointed at, not taught: pass is a
command-line tool, and this guide stays
where it does its honest work.

## How to use this guide

Read chapter 1 for the model and the
rule, and chapter 2 before installing
anything — knowing which part holds what
turns later confusion into a named
problem. Chapter 4 (the key) and chapter
5 (the store) belong in one sitting: by
the end of them you have a working
password manager, and every later chapter
is refinement. Do not skip chapter 11 if
you ever plan to share a store: its
failed first attempt is the single most
important transcript in the guide, because
the failure is silent and the fix is one
command. The lab's transcripts live in
`pass-lab/transcripts/`; every chapter
names the transcript it quotes, so you
can check the guide against the evidence.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [age for Mere Mortals](https://paulsullivan33-dev.github.io/age-for-mere-mortals/)
- [KeePassXC for Mere Mortals](https://paulsullivan33-dev.github.io/keepassxc-for-mere-mortals/)
- [Git for Mere Mortals](https://paulsullivan33-dev.github.io/git-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/pass-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/pass-for-mere-mortals/blob/main/LICENSE-CODE).
