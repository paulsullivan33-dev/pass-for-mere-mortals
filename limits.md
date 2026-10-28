# Limits

pass's design is small enough that its
limits can be stated completely. Several
were visible in the lab's own
transcripts; none of them is a reason to
avoid the tool, and all of them are
reasons to run it with open eyes.

## The names are not encrypted

Chapter 5's own listing is the evidence:
`email/example-mail.gpg`,
`banking/example-bank.gpg`. The folder
tree is plain text. Anyone who can list
your store — a stolen backup, a nosy
sync provider, a shared machine — learns
where you have accounts, how you
organise them, and roughly how many
secrets you keep, though not one
password. If that metadata matters to
you, the defences are outside pass:
encrypt the disk or the store's folder
too, keep names oblique (`mail/main`
instead of `banking/example-bank`), or
choose a database-style manager whose
whole vault, names included, is one
encrypted blob. What you cannot do is
ask pass to hide them; the names *are*
the interface.

## One key, one passphrase, total consequences

The concentration that makes pass
pleasant makes its failures absolute:

- **Lose the secret key** — disk dead,
  keyring never backed up — and the
  store is noise. Every entry, forever.
  GPG working correctly *is* the loss.
- **Forget the passphrase** and the key
  you still hold is equally useless;
  there is no reset, no support desk,
  no recovery email. This is not an
  oversight in pass; it is the property
  you bought.
- **Lose both to an attacker** — a
  copied keyring and a watched or weak
  passphrase — and every password you
  own is theirs, in bulk, at leisure.

The mitigations are boring and total:
back up the keyring (encrypted, in a
different place from the store's
backup), choose the passphrase like the
master key it is, and keep the
revocation certificate from chapter 4
findable. A password manager whose
backup plan is unfinished is a loss
waiting for a date.

## Git never forgets

Chapter 10's history is a diary: every
password you ever stored remains in old
commits, encrypted, readable by the key,
for as long as the history exists.
Deleting an entry (chapter 9) tidies
the present only. Consequences: changing
a password does not erase the old one
from your own records (fine — it is
yours, and the *service* no longer
accepts it), but handing someone the
store's Git repository hands them every
password it has ever held, including
ones you "removed" before sharing.
Share clones made from a fresh export
when the past is not part of the gift,
and treat old backups of the store as
still containing old passwords —
because they do.

## Sharing is wide, and removal is not recall

Chapter 11's teammate can read every
entry in the store, including entries
added later, until their key leaves
`.gpg-id`. Removing it re-encrypts the
files — to everyone except the person
who already decrypted and remembered
them. You cannot unteach a password.
Real revocation after a departure means
*changing the passwords themselves* at
every service in the store, which is
why shared stores should hold only what
genuinely needs sharing.

## The platform edges

- **No display, no clipboard** —
  chapter 12's verified refusal. On
  servers and in plain SSH sessions,
  `pass show` printing to the terminal
  is the only route, with the exposure
  that implies.
- **Plain text exists, briefly** —
  while editing (chapter 7) and while
  any tool displays or pastes an entry.
  pass minimises these moments; it
  cannot abolish them — a password no
  program can ever show is a password
  nothing can use.
- **The agent's cache is a window** —
  while gpg-agent holds your key
  unlocked, anything running as you
  can ask it to decrypt. That is the
  usability bargain, chosen knowingly;
  shorter cache times narrow the window
  at the price of more prompts.
- **No autofill, no phone in the box** —
  pass is a command-line store. Apps
  that carry it to browsers and phones
  exist and are other people's code,
  with their own trust questions.

## What pass is not defending against

If your machine itself is compromised —
malware running as you, watching the
terminal, reading the agent's cache,
keylogging the passphrase — pass falls
with everything else on it. Its threat
model ends at the machine's edge: lost
backups, stolen folders, curious
services, bulk breaches of *other*
companies' databases. Inside that
model it is exact. Outside it, no
password manager on the same machine
saves you, and chapter 1's rule — your
machines, kept worthy of the trust —
is the whole of the law.
