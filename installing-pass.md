# Installing pass

pass is distributed the way small Unix
tools usually are: as a package from
your operating system, with the program
itself being a single shell script plus
a manual page. The script is not the
system, though — chapter 2's cast comes
with it as dependencies, and the package
manager fetches them together:

- **gnupg** — the encryption itself.
  pass has no cryptography of its own.
- **git** — present for the day you turn
  on chapter 10's history (and harmless
  before).
- **tree** — draws the catalogue that
  `pass ls` prints.

On Debian and Ubuntu the whole install
is one command:

```bash
sudo apt install pass
```

That command is the documented route
and the one to use. This guide's lab did
something more cautious with the *same*
package: it downloaded the Ubuntu
`pass` 1.7.4 package and unpacked it
into a scratch folder, so the sandbox
machine's own setup stayed untouched,
and ran the script from there. The
program whose output fills the
transcripts is therefore the exact
program `apt install` would have given
the lab — only its address on disk
differs. The same package family serves
the other distributions (`dnf install
pass`, `pacman -S pass`, and friends),
and macOS readers get it from Homebrew
as `pass`; those routes are described
from the project's documentation, not
run in the lab.

## Check the install

Ask pass who it is. The lab's answer
(transcript
`pass-lab/transcripts/t00-version.txt`,
which also records the versions of the
companions the lab verified against):

```text
$ pass version
============================================
= pass: the standard unix password manager =
=                                          =
=                  v1.7.4                  =
=                                          =
=             Jason A. Donenfeld           =
=               Jason@zx2c4.com            =
=                                          =
=      http://www.passwordstore.org/       =
============================================
$ gpg --version | head -n 1
gpg (GnuPG) 2.4.4
$ git --version
git version 2.43.0
$ tree --version
tree v2.1.1 (c) 1996 - 2023 by Steve Baker, Thomas Moore, Francesc Rocher, Florian Sesser, Kyosuke Tokoro
```

The versions matter when you read other
people's advice. pass changes slowly —
1.7.4 has been current for years — but
GPG's prompts and Git's defaults move
underneath it, and a tip written against
an older GPG may name behaviour you
cannot reproduce. Every transcript in
this guide is from the versions above.

## The clipboard companions

If you plan to use chapter 12, one more
package decides whether it can work at
all: `xclip` on X11 desktops, or
`wl-clipboard` on Wayland ones. The lab
machine *has* xclip installed and the
clipboard still fails there, for the
more basic reason that the machine has
no display — hold that thought until
chapter 12, where the failure is run
and read properly.

## What an install leaves behind

Nothing, yet. Installing pass creates no
store, no key and no configuration; the
program simply waits. The store appears
in chapter 5, the key in chapter 4 —
key first, because the store cannot be
initialised without one.
