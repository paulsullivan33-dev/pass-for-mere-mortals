# Recipes

Short jobs, each built only from moves
earlier chapters verified. Passwords
shown are the lab's invented ones; yours
will differ.

## A second store, for work

One key can open several stores. Point
`PASSWORD_STORE_DIR` at a new folder and
initialise it with the same fingerprint
(transcript
`pass-lab/transcripts/t13-second-store.txt`):

```text
$ export PASSWORD_STORE_DIR=~/pass-lab/work-store
$ pass init 98E7C7561F88C7288694F63515837920394279EA
mkdir: created directory '~/pass-lab/work-store/'
Password store initialized for 98E7C7561F88C7288694F63515837920394279EA
$ printf 'tunnel-63-ember\ntunnel-63-ember\n' | pass insert vpn/example-vpn
$ pass ls
Password Store
`-- vpn
    `-- example-vpn
$ unset PASSWORD_STORE_DIR
$ pass ls email
email
`-- example-mail
```

Unsetting the variable returns you to
the default store. Separate stores suit
separate *trusts* — a work store can get
its own remote, its own `.gpg-id` list,
its own chapter-11 teammates — while
sharing one key keeps the daily use
simple. (Your real folders will not be
called `~/pass-lab`; the lab's
transcripts keep its scratch address.)

## Moving to a new machine

Everything needed is three parcels,
moved separately: the **store** (clone
it, if chapter 10's Git remote exists —
`git clone`, then use it directly or via
`PASSWORD_STORE_DIR` as chapter 10 did);
the **secret key** (`gpg --export-secret-keys`,
carried on something you control, then
`gpg --import` at the other end — the
export and import themselves are plain
GPG, described here rather than run,
because the lab never lets its key leave
its scratch keyring even in fiction); and
the **passphrase**, carried in your
head. When all three arrive,
`pass show` answers — chapter 10's
clone-and-show transcript is the second
half of this recipe, verified.

## A password inside a script

Line one only, from chapter 8:

```bash
DB_PASS=$(pass show banking/example-bank | head -n 1)
```

The agent makes this painless while its
cache is warm; a cold cache means a
pinentry prompt in the middle of your
script, which is GPG being correct.
Scripts that must run unattended belong
on keys and stores created for them —
never your main key with its passphrase
written beside the script, which would
defeat the entire arrangement in one
line.

## The reuse audit

`pass grep` (chapter 8) decrypts
everything, so it can answer hygiene
questions. Grep for a password you
suspect you reused — generated passwords
make this unlikely, typed ones make it
worth checking:

```bash
pass grep 'sunrise-42-harbor'
```

One match — the entry the password
belongs to — is the answer you want.
Two matches means the same password is
serving two services, and one of them
gets a chapter-6 generation today. Grep
for an old address, a retired user
name, a recovery hint — each search is
a small audit the database-style
managers cannot offer without exporting
first.

## Retiring a password properly

Change it at the service first. Then
either `pass generate --in-place`
(chapter 6) to rewrite line one and keep
the notes, or `pass cp` the entry to an
`-old` name first (chapter 9) when the
service is one you cannot be locked out
of, and delete the old entry after the
new password is proven. Remember
chapter 9's warning: with Git on, the
old password remains in history,
encrypted, for as long as the history
is kept. Retired means *no longer
accepted by the service* — that part is
the service's doing, and it is the part
that protects you.
