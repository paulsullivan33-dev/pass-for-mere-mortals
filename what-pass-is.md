# What pass Is

A password manager has to keep secrets
in a form you can use but nobody else
can read. Most of them do it with a
database: one big encrypted blob that
only the manager itself can open, served
by an account, a program, sometimes a
company. **pass** does it with almost
nothing:

- **One file per password.** The password
  for your example mail account is a file
  called `email/example-mail.gpg` in a
  folder on your disk.
- **Each file is encrypted with GPG** —
  the long-established encryption tool
  that already lives on most Unix
  machines — using a key pair you own.
- **The folder is the database.** Its
  subfolders are categories, its file
  names are the names of your accounts,
  and listing the folder lists your
  passwords.
- **pass itself is a shell script** that
  ties the pieces together: it asks GPG
  to encrypt and decrypt, keeps the
  folder tidy, and — if you turn it on —
  asks Git to record every change.

That is the whole design, and its
plainness is the point. There is no
pass server, no pass account, and no
pass file format to reverse-engineer.
Chapter 5 of this guide decrypts a store
file with plain `gpg`, no pass anywhere
in sight, to prove the wrapper is only a
wrapper.

## What an entry is

An **entry** is one encrypted file. The
first line of the decrypted file is the
password. Everything after the first
line is notes you chose to keep with it:
a user name, a web address, a recovery
hint. Chapter 7 uses that space properly.
The entry's *name* — `email/example-mail`
— is just its path inside the store
folder, without the `.gpg` ending. You
will type that name constantly, so short
predictable names are worth more than
clever ones.

## The rule, stated before everything else

A password store concentrates your whole
digital life behind one key and one
passphrase. That concentration is what
makes it useful, and it demands one
habit, which this guide states now and
repeats in chapter 15:

> **Guard the key and its passphrase like
> the master copies they are — and
> practise on invented passwords until
> the commands are boring.**

Anyone holding your secret key *and* its
passphrase can read every password you
own; losing either one locks you out of
all of them at once. The lab behind this
guide obeyed the rule strictly. Its key
was made for the occasion, its passphrase
is printed in the transcripts because it
protects nothing real, and every password
in its store belongs to a fictional
service at an address that does not
exist. You can therefore watch
`pass show` print a password in chapter
5 without anyone's secret being spent —
and when you build your own store, the
same commands will be handling real ones.

## A first taste

After installing (chapter 3) and making
a key (chapter 4), three commands are a
working password manager:

```bash
pass init 98E7C7561F88C7288694F63515837920394279EA
pass insert email/example-mail
pass show email/example-mail
```

The long string of hex digits is the
key's **fingerprint** — its public name,
safe to share and print. The insert asks
for the password twice, silently, and
stores it encrypted. The show asks the
key's guardian program for the passphrase
if it needs it, decrypts, and prints:

```text
$ pass show email/example-mail
sunrise-42-harbor
```

(Transcript `pass-lab/transcripts/t03-insert-show.txt`.)
The password shown is invented, like
every password in this guide.

## Who it is for

The person who lives in a terminal and
wants their passwords to be *files*:
greppable, scriptable, copyable with the
same tools as everything else they own.
The person who distrusts handing every
secret they have to a service that might
be breached, bought, or shut down. The
person with two or three machines who is
happy letting Git carry the store between
them. The common thread is one person,
their own machines, and a preference for
parts they can inspect.

## What it is not

pass is not a synchronisation service:
it will happily keep a store on one
machine only, and chapters 10 and 13
show the Git arrangement *you* operate
to move it. It is not an autofill tool:
nothing here types passwords into
browsers for you — the clipboard
(chapter 12) is as close as pass comes,
and phone apps that read a pass store
are other people's programs, pointed at
in chapter 14. It is not a sharing
platform: chapter 11's team store is a
folder and two keys, not an organisation
with accounts and permissions. And it is
not private about *metadata*: the names
of your entries sit on disk in plain
text, visible to anyone who can list
the folder — chapter 15 is blunt about
what that leaks.
