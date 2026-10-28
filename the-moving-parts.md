# The Moving Parts

pass is a conductor, not an orchestra.
Six parts do the work, and knowing which
part holds what turns every later
confusion into a named problem.

## The store

A folder, by default `~/.password-store`.
Everything pass knows lives here: one
`.gpg` file per entry, arranged in
subfolders you name as you go
(`email/`, `banking/`, `wifi/`). There
is no index and no database file; the
directory listing *is* the catalogue.
Set the environment variable
`PASSWORD_STORE_DIR` to point pass at a
different folder and you have a second
store — chapter 13 runs one beside the
first.

## The .gpg-id file

A small plain-text file at the top of
the store, naming the key or keys every
entry is encrypted for — one fingerprint
per line. It is written by `pass init`
(chapter 5) and rewritten when the
recipient list changes (chapter 11). It
is not secret: it names *public* keys.
It is, however, load-bearing: entries
are encrypted to exactly the keys it
lists, which is why chapter 11 checks
the files themselves rather than
trusting this file's say-so. A subfolder
may carry its own `.gpg-id`, narrowing
the recipients for just that folder —
described from the manual here, not
demonstrated in the lab.

## Your GPG key pair

Two matched halves, made in chapter 4.
The **public key** encrypts; anyone may
have it, and pass writes its fingerprint
into `.gpg-id`. The **secret key**
decrypts; it lives in your GPG keyring
(by default `~/.gnupg`), never in the
store, and it is itself locked with a
**passphrase**. Store and keyring are
separate folders on purpose: the store
is safe to copy, back up and push to a
Git remote precisely because it contains
nothing that can decrypt it.

## The agent

Typing the key's passphrase for every
single `pass show` would end the habit
within a day, so GPG runs a background
helper, **gpg-agent**, which holds the
unlocked key in memory for a while after
you type the passphrase once. A small
prompt program, **pinentry**, collects
the passphrase when the agent needs it.
When a decrypt fails with a complaint
about terminals or devices, it is almost
always this pair failing to ask you the
question — chapter 16 names the shapes
that takes.

## tree, Git, and the clipboard tools

Three optional companions do visible
work:

- **tree** draws the store listing that
  `pass ls` prints. Without it, pass
  cannot show you the catalogue.
- **Git** is optional until you ask for
  it (`pass git init`, chapter 10); from
  then on every insert, edit and removal
  is committed automatically, and push
  and pull become your synchronisation.
- **xclip** (on X11) or **wl-clipboard**
  (on Wayland) carry chapter 12's
  clipboard trick. Without a display
  they cannot work at all — the lab
  proves the failure, since it has none.

## Extensions

pass can grow extra commands from shell
scripts in an extensions folder —
community add-ons exist for one-time
passwords, menus, and bulk operations.
The lab installed none, so extensions
are described in chapter 12 from the
manual only. The stock commands are the
whole of this guide's verified ground.

## Which part holds the secret

Worth stating once, plainly. The
*passwords* are in the store, encrypted.
The *ability to read them* is the secret
key in the keyring, gated by its
passphrase and cached briefly by the
agent. pass itself holds nothing: delete
the program and every entry still
decrypts with `gpg` alone (chapter 5
does exactly that). Whoever plans a
backup is therefore planning for two
folders and one memorised passphrase —
chapter 15 returns to this with the
consequences spelled out.
