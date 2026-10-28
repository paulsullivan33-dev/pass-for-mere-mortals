# The Store in Git

A folder of files can be copied. A folder
of files under **Git** can be *history*:
every change committed, every old version
retrievable, every push a backup and
every clone a working copy. pass wires
Git in with one command and then drives
it for you — and this chapter contains
the guide's most useful failure, because
the first attempt fails while reporting
success.

## Turning it on — and the failure

`pass git` is a pass-through: everything
after it runs as a Git command inside the
store. `pass git init` creates the
repository, and pass then tries to commit
the store's current contents as the first
commit. On the lab machine, Git had never
been told a name or email address to sign
commits with. The transcript
(`pass-lab/transcripts/t09-git.txt`,
which also records a line of housekeeping
first — naming the default branch `main`)
shows what happened:

```text
$ pass git init
Initialized empty Git repository in ~/.password-store/.git/
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'root@htch-runtime.(none)')
```

Git prints that complaint **twice** (two
commits were attempted — the contents,
and a `.gitattributes` file pass adds so
Git knows `.gpg` files have a special
diff driver) — and then the line that
makes this failure dangerous:

```text
pass git init exit: 0
```

Exit zero. Success, as far as any script
checking the result can tell. But
`pass git status` tells the truth: every
file is staged and **uncommitted**. The
repository exists; the history does not.
The lesson generalises beyond pass: with
this integration, *verify by looking*,
not by trusting the exit code — a habit
this guide has applied to itself
throughout.

## The recovery

Tell Git who you are, then make the
commit pass attempted, using its own
words for the message:

```text
$ git config --global user.name "Lab Keeper"
$ git config --global user.email "keeper@lab.example.invalid"
$ pass git add -A
$ pass git commit -m "Add current contents of password store."
[main (root-commit) a002b45] Add current contents of password store.
 7 files changed, 2 insertions(+)
 create mode 100644 .gitattributes
 create mode 100644 .gpg-id
 create mode 100644 banking/example-bank.gpg
 create mode 100644 email/example-mail.gpg
 create mode 100644 shopping/example-store.gpg
 create mode 100644 social/example-social.gpg
 create mode 100644 wifi/home-router.gpg
```

## Life with history on

From now on pass commits for you. An
ordinary insert, done by script as
before:

```text
$ printf 'harbor-19-willow\nharbor-19-willow\n' | pass insert web/example-forum


[main f5337ac] Add given password for web/example-forum to store.
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 web/example-forum.gpg
$ pass git log --oneline
f5337ac Add given password for web/example-forum to store.
a002b45 Add current contents of password store.
```

The commit messages are pass's own —
"Add given password for ...", "Update
password for ...", "Remove ... from
store" — plain enough that
`pass git log` reads as the store's
diary. Every password you have ever
changed is in there, encrypted as
always; chapter 15 draws the conclusion
about old passwords never truly leaving.

## A remote, and a second machine

A Git remote for a password store is
just a place the encrypted folder can be
pushed to; the encryption means the
remote learns names and shapes but no
passwords (chapter 15 qualifies even
that). The lab used the simplest remote
there is — a bare repository on the same
machine, reached by a `file://` address —
which proves the mechanics without an
account anywhere:

```text
$ git init --bare ~/pass-lab/store-remote.git
Initialized empty Git repository in ~/pass-lab/store-remote.git/
$ pass git remote add origin file://~/pass-lab/store-remote.git
$ pass git push -u origin main
To file://~/pass-lab/store-remote.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

And the payoff — the store, cloned to a
fresh folder and read there through the
`PASSWORD_STORE_DIR` switch from chapter
2, with the same keyring doing the
decrypting:

```text
$ git clone file://~/pass-lab/store-remote.git ~/pass-lab/clone2
Cloning into '~/pass-lab/clone2'...
$ PASSWORD_STORE_DIR=~/pass-lab/clone2 pass show email/example-mail
sunrise-42-harbor
```

That is pass's whole synchronisation
story: Git moves the folder, your key —
carried to the second machine by you,
separately — opens it. A hosted remote
over SSH (a private repository with a
hosting service) works identically;
setting one up is ordinary Git, described
in that guide's terms rather than run
here, because the lab does not hand its
store, even a fictional one, to outside
services.
