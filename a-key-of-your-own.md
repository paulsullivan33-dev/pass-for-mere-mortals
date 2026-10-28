# A Key of Your Own

Everything in a pass store is encrypted
to a **key pair**: two matched halves of
one mathematical object. The public half
can encrypt and can be handed to anyone.
The secret half decrypts, is kept in
your GPG keyring, and is itself locked
with a **passphrase** — the one password
in this whole arrangement that you must
simply remember, because it guards all
the others. If you already have a GPG
key you trust, pass can use it and this
chapter is a refresher. If not, you make
one now.

## Making the key

Interactively, the command is

```bash
gpg --full-generate-key
```

and it interviews you: key type (the
default is fine), key size, how long the
key should live, your name and email
address, and finally the passphrase,
typed twice into pinentry's prompt. None
of those questions has a wrong answer
for pass's purposes except the last one:
choose a passphrase you will still know
in ten years, because there is no reset
(see chapter 15).

The lab needed the same key with no
human at the keyboard, so it used GPG's
batch form of the identical operation —
an invented name, an address at
`example.invalid`, a domain reserved for
exactly this kind of fiction, and a
passphrase printed here because it
protects nothing real (transcript
`pass-lab/transcripts/t01-keygen.txt`):

```text
$ gpg --batch --pinentry-mode loopback --passphrase 'lab-only-passphrase' \
    --quick-generate-key 'Lab Keeper <keeper@lab.example.invalid>' default default never
gpg: revocation certificate stored as '~/pass-lab/gnupg/openpgp-revocs.d/98E7C7561F88C7288694F63515837920394279EA.rev'
$ gpg --list-keys
~/pass-lab/gnupg/pubring.kbx
----------------------------------------------------------------
pub   ed25519 2026-10-28 [SC]
      98E7C7561F88C7288694F63515837920394279EA
uid           [ultimate] Lab Keeper <keeper@lab.example.invalid>
sub   cv25519 2026-10-28 [E]
```

Read the listing the way pass will. Each
key is really two: a primary key marked
`[SC]` (it signs and certifies) and a
subkey marked `[E]` — the half that
entries are actually encrypted to.
Beneath the primary sits its
**fingerprint**, forty hex digits. That
string is the key's public name: safe to
print, safe to email, and the identifier
`pass init` will ask for in the next
chapter. GPG also stored a **revocation
certificate** — a pre-written "this key
is dead" notice, for the day a key is
lost or exposed; keep yours wherever you
keep the key's backups.

Check that both halves exist:

```text
$ gpg --list-secret-keys
~/pass-lab/gnupg/pubring.kbx
----------------------------------------------------------------
sec   ed25519 2026-10-28 [SC]
      98E7C7561F88C7288694F63515837920394279EA
uid           [ultimate] Lab Keeper <keeper@lab.example.invalid>
ssb   cv25519 2026-10-28 [E]
```

`sec` and `ssb` are the secret shadows of
`pub` and `sub`. Same fingerprints, other
drawer.

## The passphrase, and the agent

Decrypting anything means persuading
GPG to unlock the secret key, which
means the passphrase. Typing it once per
password would be unusable, so the
**gpg-agent** (chapter 2) caches the
unlocked key in memory for a stretch of
time after each use. In practice: the
first `pass show` of a session pops up a
pinentry prompt; the next several are
silent. The lab ran with no screen for
pinentry to draw on, so it unlocked the
agent once per session by decrypting a
throwaway message with the passphrase
supplied on the command line
(`--pinentry-mode loopback`) — a lab
convenience for scripts, not something
to copy onto a real machine, where the
prompt *is* the security working as
intended.

Two consequences deserve a sentence each
before the store exists. The passphrase
is not stored anywhere recoverable: GPG
cannot tell it to you, and neither can
pass. And the secret key's *file* is
only half the lock — a copied keyring
plus a guessed passphrase is a copied
store, which is why chapter 15 treats
key backups as secrets in their own
right.
