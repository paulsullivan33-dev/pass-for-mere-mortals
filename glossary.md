# Glossary

**Agent (gpg-agent)** — GPG's background
helper, which holds your secret key
unlocked in memory for a while after you
type its passphrase, so you are not asked
for every entry.

**Entry** — one password in the store:
an encrypted file, named by its path
(`email/example-mail`), whose first line
is the password and whose remaining
lines are notes.

**Fingerprint** — a key's public name:
forty hexadecimal digits identifying
the key pair. Safe to share and print;
it is what `pass init` records.

**GPG (GnuPG)** — the encryption program
pass drives. It makes key pairs, encrypts
the entry files, and decrypts them; pass
contains no cryptography of its own.

**`.gpg-id`** — the plain-text file at
the top of a store (or of a folder)
listing the fingerprints every entry
there is encrypted for.

**Key pair** — the matched public and
secret halves of one GPG identity. The
public half encrypts and may be shared;
the secret half decrypts and must not
be.

**Keyring** — GPG's own store of keys,
by default `~/.gnupg`. Your secret key
lives here, never in the password store.

**Ownertrust** — how much you trust a
key's owner to vouch for *other* keys.
Not the same as validity — chapter 11's
failed sharing attempt is the
difference, demonstrated.

**Passphrase** — the password that locks
your secret key itself. The one secret
pass cannot store for you.

**PASSWORD_STORE_DIR** — the environment
variable that points pass at a store
other than `~/.password-store`; the
switch behind second stores and clones.

**pinentry** — the small program that
draws the passphrase prompt on behalf of
the agent. Its failures wear several
disguises in chapter 16.

**Recipient** — a key an entry is
encrypted for. An entry file carries one
public-key packet per recipient;
counting them is how chapter 11 verifies
sharing.

**Re-encryption** — decrypting an entry
and encrypting it again for a changed
recipient list, as `pass init` does when
keys are added or removed.

**Store** — the folder holding the
entries, by default `~/.password-store`.
The folder is the database.

**Validity** — GPG's judgement that a
key really belongs to its named owner,
established by signatures — yours, after
you check the fingerprint. Encryption to
a key requires it.
