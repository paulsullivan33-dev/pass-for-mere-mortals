# Troubleshooting

Every failure below was produced on
purpose in the lab and is quoted from
its transcripts
(`pass-lab/transcripts/t12-failures.txt`
unless another is named). Read the error
the way the lab learned to: it usually
names the layer that failed — pass,
GPG, Git, or the machine.

## "is not in the password store"

```text
$ pass show post-office/example
Error: post-office/example is not in the password store.
exit: 1
```

A pass-level error: the name does not
resolve to a file. Check the spelling
against `pass ls`, and remember names
are paths — `example` and
`email/example` are different entries,
and the `.gpg` ending is not part of
the name you type.

## "No secret key"

```text
$ GNUPGHOME=<empty keyring> pass show email/example-mail
gpg: public key decryption failed: No secret key
gpg: decryption failed: No secret key
exit: 2
```

Now GPG is speaking. The file names a
recipient your keyring cannot satisfy:
the secret key was never imported here,
or the keyring being consulted is the
wrong one (a fresh machine, a different
`GNUPGHOME`, a store copied without its
key — the classic half-finished move
from chapter 13's recipe). The store is
fine; the *key* is absent. `gpg
--list-secret-keys` shows what this
keyring actually holds.

## "Bad passphrase"

```text
gpg: public key decryption failed: Bad passphrase
gpg: decryption failed: Bad passphrase
exit: 2
```

The key is present; the passphrase you
typed is not its passphrase. (The lab
produced this by decrypting directly
with a deliberately wrong one.) Caps
lock, the wrong keyboard layout, or the
passphrase of a *different* key are the
usual causes. There is no counter and no
lockout — and no recovery either, if
the truth is that it is forgotten:
chapter 15 is the whole story.

## "Checksum error"

```text
gpg: ecdh failed in gcry_cipher_decrypt: Checksum error
gpg: public key decryption failed: Checksum error
gpg: decryption failed: Checksum error
exit: 2
```

The file itself is damaged — the lab
overwrote four bytes in the middle of a
copy and GPG's integrity check caught
it, which is the design working. The
original's other copies are the cure:
the Git history (chapter 10) if the
damage is recent and local, the remote,
or a backup. A store with one copy of a
file has exactly this failure in its
future, eventually.

## Git commits that never happened (exit 0)

Chapter 10's failure: `pass git init`
(or any automatic commit) reports
success while Git's own complaint —
"Author identity unknown" — scrolls
past, and nothing is committed. The
cause is a Git with no `user.name` /
`user.email`. The check is
`pass git status` and `pass git log`;
the cure is two `git config` commands
and one manual commit. Generalised:
when pass drives another tool, believe
the *state*, not the exit code.

## "There is no assurance this key belongs to the named user"

Chapter 11's failure:
`pass init` with a second key prints
"reencrypting" lines, exits 0 — and the
files gain no new recipient, because
GPG will not encrypt to a key whose
validity nobody has established, and its
attempt to ask interactively dies at
"cannot open '/dev/tty'". The check is
`gpg --list-packets` on any entry —
count the `pubkey enc packet` lines
against `.gpg-id`. The cure is to
verify the fingerprint out-of-band and
sign the key (`--quick-sign-key`, or
`lsign` inside `gpg --edit-key`), then
run `pass init` again.

## "No X11 or Wayland display detected"

Chapter 12's refusal: `pass show -c` on
a machine with no graphical session.
xclip being installed does not help;
there is no clipboard to copy to. Use
plain `pass show` there, or work from a
machine with a display.

## "the entered passwords do not match"

Chapter 5's refusal: the two typings at
`pass insert` differed; nothing was
stored, though the folder may have been
created. Type it again, or — for a
password you cannot reliably type —
let chapter 6 generate it instead of
transcribing it by hand.

## The agent will not ask

A decrypt that fails instantly, or
complains about pinentry, ioctls, or
devices, is the agent (chapter 2)
failing to collect the passphrase: no
terminal on an SSH session, a pinentry
variant that needs a display the
session lacks. The terminal-shaped
cure is a pinentry that matches the
session (`pinentry-curses` or
`pinentry-tty`, chosen in
`gpg-agent.conf`). The lab's scripting
shortcut — loopback mode with the
passphrase on the command line — is
named in chapter 4 as exactly what it
is: a lab convenience, not a pattern
for a real machine.
