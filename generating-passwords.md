# Generating Passwords

Typed passwords have an author, and the
author has habits: birthdays, keyboard
walks, the same root word on every site.
Chapter 5's `sunrise-42-harbor` is fine
for a lab and mediocre for a life.
`pass generate` removes the author. It
invents a password from the machine's
randomness, stores it encrypted, and
shows it to you exactly once — on the
way in (transcript
`pass-lab/transcripts/t05-generate.txt`):

```text
$ pass generate social/example-social 24
mkdir: created directory '~/.password-store/social'
The generated password for social/example-social is:
syha3.{V^"U(.N&~%bO`\?J_
$ pass generate -n banking/example-bank 20
mkdir: created directory '~/.password-store/banking'
The generated password for banking/example-bank is:
8pt22UhtiKTFeVBzY1vw
```

The number is the length — 24 characters
for the social account, 20 for the bank.
Length is the cheapest strength there
is: every added character multiplies the
guessing work, and since pass remembers
the password for you, nothing except a
website's own rules argues for short.

The first password is drawn from letters,
digits and symbols; read it and you will
find quotes, braces and a backslash in
there. Some websites reject symbols, and
some reject particular ones, so the
second command's `-n` (`--no-symbols`)
restricts the alphabet to letters and
digits. That smaller alphabet is why the
lab kept the no-symbols password at a
respectable length too — with fewer
possible characters per position, length
does more of the work.

## The one showing, and the overwrite

"The generated password ... is:" is the
only time pass volunteers a new password
in plain text. Afterwards it behaves like
any other entry: `pass show` prints it,
the clipboard (chapter 12) can carry it,
and the file on disk is the same opaque
`.gpg` as chapter 5 inspected. If your
terminal scrolls away, nothing is lost —
show it again.

Generating onto an entry that already
exists asks before overwriting, and
`--force` skips the asking; `--in-place`
rewrites just the first line of an entry
and keeps its notes, which is the right
tool for a password change on an entry
chapter 7 has filled with notes. Those
two behaviours are described from the
manual — the lab generated only fresh
entries — and the manual's other note
is worth copying: where the program
`pwgen` is installed, pass uses it as
its random source; the lab machine has
no `pwgen`, so the passwords above came
from pass reading the system's
randomness directly.

## A workable habit

Generate long, generate per-site, and
let the store remember. When a site
forces a smaller alphabet or a shorter
ceiling, `-n` and a number are the whole
negotiation. The password you will never
do this for is the key's own passphrase
from chapter 4 — pass cannot store the
thing that unlocks pass, which is why
that one stays human-memorised.
