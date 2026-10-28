# Your First Store

With a key in hand, one command creates
the store. `pass init` takes the key's
fingerprint and does two things: it makes
the folder `~/.password-store`, and it
writes the fingerprint into a file inside
it called `.gpg-id` — the store's answer
to "encrypt everything for whom?"
(transcript `pass-lab/transcripts/t02-init.txt`):

```text
$ pass init 98E7C7561F88C7288694F63515837920394279EA
mkdir: created directory '~/.password-store/'
Password store initialized for 98E7C7561F88C7288694F63515837920394279EA
$ cat ~/.password-store/.gpg-id
98E7C7561F88C7288694F63515837920394279EA
$ ls -la ~/.password-store/
total 4
drwx------ 1 root root 14 Oct 28 18:47 .
drwxrwx--- 1 root root 30 Oct 28 18:47 ..
-rw------- 1 root root 41 Oct 28 18:47 .gpg-id
$ pass ls
Password Store
```

Three details in that short transcript
carry weight. The folder's permissions
are `drwx------` — readable by you alone.
The `.gpg-id` file is plain text and not
a secret: fingerprints are public names.
And `pass ls` on an empty store prints
just its title; the tree grows as entries
arrive.

## Your first password

`pass insert` names an entry and asks
for its password twice, with the typing
hidden (transcript
`pass-lab/transcripts/t03-insert-show.txt`):

```text
$ pass insert email/example-mail
mkdir: created directory '~/.password-store/email'
Enter password for email/example-mail: 
Retype password for email/example-mail: 
$ pass ls
Password Store
`-- email
    `-- example-mail
$ pass show email/example-mail
sunrise-42-harbor
```

The name before the slash became a
folder; the name after it became a file.
`pass show` is the whole retrieval story:
pass finds the file, GPG decrypts it with
your secret key (the agent may first ask
for the passphrase), and the password is
printed. Check — if `pass show` prints
the password you typed, your store works,
and every later chapter is refinement.

The double typing earns its keep
immediately. When the two copies
disagree, pass refuses and stores
nothing:

```text
$ pass insert web/mismatch-demo
mkdir: created directory '~/.password-store/web'
Enter password for web/mismatch-demo: 
Retype password for web/mismatch-demo: 
Error: the entered passwords do not match.
$ pass ls web
web
```

An empty folder remains — harmless; a
later insert into `web/` will use it. The
lab also fed passwords to `pass insert`
from scripts (two identical lines on
standard input do the same job, with no
prompts shown), which is how chapters 9
and 10 add entries without a typist;
`pass insert --echo` exists for the
one-line form. At a terminal, type.

## What actually landed on disk

Before trusting the folder with anything,
look at it the way an attacker with a
stolen backup would (transcript
`pass-lab/transcripts/t04-on-disk.txt`):

```text
$ find ~/.password-store -type f | sort
~/.password-store/.gpg-id
~/.password-store/banking/example-bank.gpg
~/.password-store/email/example-mail.gpg
~/.password-store/social/example-social.gpg
~/.password-store/wifi/home-router.gpg
$ file ~/.password-store/email/example-mail.gpg
~/.password-store/email/example-mail.gpg: data
$ grep -r 'sunrise-42-harbor' ~/.password-store/
grep exit: 1
```

(Two of these entries arrive in
chapter 6 and one in chapter 7; the lab
re-ran its listing once the store was
fuller.) `file` sees
undifferentiated *data* — an OpenPGP
message, binary, no text to read. And
`grep` for the known password across the
whole store finds **nothing**: exit 1,
grep's "no match". The password is not
in the store in any form a text search
can find.

Then the demystifying step. pass is a
wrapper, so plain GPG opens the file
without it:

```text
$ gpg --batch --decrypt ~/.password-store/email/example-mail.gpg
sunrise-42-harbor
```

Same password, no pass involved. Whoever
holds your secret key and its passphrase
can do this to every file in the store —
which is the rule from chapter 1, now
with evidence. It also means your store
can never be locked inside pass: the
program is a convenience, the format is
GPG's, and the exit door is always open.
