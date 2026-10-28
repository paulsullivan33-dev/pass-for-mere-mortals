# Sharing a Store

Everything so far encrypted to one key.
To share a store — with a partner, a
teammate, a second self with a second
key — you encrypt to *two*. pass makes
it sound like one command, and
eventually it is. But this chapter
contains the guide's second great
failure, worse than chapter 10's because
quieter, and the lab kept every line of
it. Read the failure first; the fix is
short, and the verification habit it
teaches is the real content.

## The setup: a second key arrives

The teammate makes their own key pair,
exactly as in chapter 4, in *their own*
keyring, and sends over only the public
half — public keys are for sending. The
keeper imports it (transcript
`pass-lab/transcripts/t10-sharing.txt`;
the lab kept the teammate's keyring in a
separate folder so one machine could
play both parts):

```text
$ gpg --import teammate-pub.asc
gpg: key 691A5E66A2C91052: public key "Lab Teammate <teammate@lab.example.invalid>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

## The attempt that silently did nothing

Now the obvious command: initialise the
store again, naming both fingerprints.
pass announces success, Git commits the
new `.gpg-id` — and then, entry by
entry, GPG declines to do the work:

```text
$ pass init 98E7C7561F88C7288694F63515837920394279EA 770996A6F5181FCDF5AA6CDF691A5E66A2C91052
Password store initialized for 98E7C7561F88C7288694F63515837920394279EA, 770996A6F5181FCDF5AA6CDF691A5E66A2C91052
[main d5ce42d] Set GPG id to 98E7C7561F88C7288694F63515837920394279EA, 770996A6F5181FCDF5AA6CDF691A5E66A2C91052.
 1 file changed, 1 insertion(+)
email/example-mail: reencrypting to 41763E1E0C6AD8EB CF67A51CDFE69FAF
gpg: CF67A51CDFE69FAF: There is no assurance this key belongs to the named user
gpg: cannot open '/dev/tty': No such device or address
```

— the same two warnings, for all six
entries. And still:

```text
pass init exit: 0
```

Check the files, not the announcement.
An OpenPGP file carries one **public-key
packet** per recipient; count them:

```text
$ gpg --batch --list-packets ~/.password-store/email/example-mail.gpg | grep "keyid"
:pubkey enc packet: version 3, algo 18, keyid 41763E1E0C6AD8EB
```

One packet — the keeper's own encryption
subkey. After a "successful" two-key
initialisation, every entry is still
encrypted to the keeper alone, while
`.gpg-id` — committed to Git, no less —
claims two recipients. A team that
stopped here would discover the truth
weeks later, at the worst moment, when
the teammate's first decrypt failed.

What went wrong is GPG working as
designed. Encrypting to a key means
asserting "this key really belongs to
the named person". GPG found no basis
for that assertion about the imported
key — nobody had vouched for it — so it
tried to *ask* ("cannot open '/dev/tty'"
is that question failing to reach a
human), and pass relayed neither the
question nor the refusal into its exit
code. Two distinctions matter here, and
GPG keeps them separate: **ownertrust**
is how much you trust a key's owner to
vouch for *other* keys, while **validity**
is whether this key itself is believed
to be its owner's. Encryption needs
validity. (The lab tested the ownertrust
route first, on the common advice; the
key stayed `[ unknown]` and encryption
kept refusing. The lab's own keyring is
the evidence: what was missing was a
signature.)

## The fix: vouch for the key yourself

You check the fingerprint with the
teammate through some channel you
already trust — read aloud, compared
against a printed card, anything
out-of-band — and then you **sign their
key** with yours. A *local* signature
(`lsign`, in the interactive editor)
never leaves your keyring; the batch
form the lab used signs the same way:

```text
$ gpg --batch --pinentry-mode loopback --passphrase 'lab-only-passphrase' --quick-sign-key 770996A6F5181FCDF5AA6CDF691A5E66A2C91052
pub  ed25519/691A5E66A2C91052
     trust: unknown       validity: unknown
 Primary key fingerprint: 7709 96A6 F518 1FCD F5AA  6CDF 691A 5E66 A2C9 1052
     Lab Teammate <teammate@lab.example.invalid>
$ gpg --list-keys teammate@lab.example.invalid
pub   ed25519 2026-10-28 [SC]
      770996A6F5181FCDF5AA6CDF691A5E66A2C91052
uid           [  full  ] Lab Teammate <teammate@lab.example.invalid>
sub   cv25519 2026-10-28 [E]
```

`[ unknown]` became `[  full  ]`: the
key is now believed. Run the same
`pass init` again and the re-encryption
runs clean — six quiet "reencrypting"
lines, no warnings, and a Git commit
that this time describes something true:

```text
[main 048b041] Reencrypt password store using new GPG id 98E7C7561F88C7288694F63515837920394279EA, 770996A6F5181FCDF5AA6CDF691A5E66A2C91052.
 6 files changed, 0 insertions(+), 0 deletions(-)
```

## The proof, twice over

First proof — count the packets again:

```text
$ gpg --batch --list-packets ~/.password-store/email/example-mail.gpg | grep "keyid"
:pubkey enc packet: version 3, algo 18, keyid 41763E1E0C6AD8EB
:pubkey enc packet: version 3, algo 18, keyid CF67A51CDFE69FAF
```

Two recipients now, the keeper's subkey
and the teammate's. Second proof — the
teammate themselves, from their own
keyring, against a copy of the store:

```text
$ GNUPGHOME=<teammate keyring> PASSWORD_STORE_DIR=~/pass-lab/store-copy pass show email/example-mail
sunrise-42-harbor
```

The password, read by the second key.
That is sharing, verified the way this
guide verifies everything: by the file's
own contents and by the other party's
real decrypt — never by the success
message.

Two closing truths. Sharing is per
store (or per `.gpg-id` folder): the
teammate can read *every* entry in it,
present and future, until removed. And
removal is not recall — chapter 15
explains why taking a key off `.gpg-id`
does not unteach anyone a password they
have already read.
