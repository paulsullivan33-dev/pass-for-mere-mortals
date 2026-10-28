# The Clipboard

`pass show` prints the password on your
screen, where shoulder-surfers and
screenshots can keep it. The clipboard
form avoids the screen entirely:

```bash
pass show -c email/example-mail
```

From the manual: `-c` copies the **first
line** of the entry — the password, by
chapter 7's rule, never the notes — to
the clipboard using `xclip` on X11 or
`wl-clipboard` on Wayland, tells you it
has copied, and then, 45 seconds later,
**restores the clipboard to what it held
before**, so the password does not sit
in the clipboard all afternoon. The
interval is configurable
(`PASSWORD_STORE_CLIP_TIME`), and
`pass generate -c` and
`pass insert -c` can put a new password
straight onto the clipboard without it
ever being displayed. It is the right
default way to *use* a password at a
graphical machine.

## What the lab could verify: the failure

The lab machine has no display at all,
and on it the whole feature fails in one
line (transcript
`pass-lab/transcripts/t11-clipboard.txt`):

```text
$ pass show -c email/example-mail
Error: No X11 or Wayland display detected
exit: 1
$ echo "DISPLAY is [$DISPLAY]"
DISPLAY is []
$ which xclip
/usr/bin/xclip
```

Read that carefully, because it is the
honest state of the art for headless
machines, servers, and SSH sessions
without forwarding: the tool (`xclip`)
is installed, the entry exists, the key
works — and there is nobody to copy
*to*. The clipboard is a property of a
graphical session, and without one,
`-c` is a clean, immediate refusal
(exit 1, mercifully unlike chapter 10's
silent success). So this chapter's
success story — copy, paste into the
login form, watch the clipboard clear
itself — is **documented-only**: taken
from the manual, not run. The lab does
not print a staged success.

Two cautions belong to the feature
wherever it works. The clipboard is
shared space: any program running in
your session can read it during those
45 seconds, and clipboard managers may
keep a history the self-clearing cannot
reach into. And pasting is still
showing, in the end — the receiving
program gets the plain password, as it
must. The clipboard narrows the
exposure; it does not abolish it.

## Extensions, briefly

pass can learn new verbs from scripts
in an extensions folder
(`~/.password-store/.extensions` by
default, or `$PASSWORD_STORE_EXTENSIONS_DIR`),
each becoming a `pass <verb>` command.
The community maintains several: a
one-time-password generator
(`pass-otp`), a menu front-end, import
tools for other managers' exports (the
package itself ships converters for a
dozen rival formats). The lab installed
none and ran none — extensions change
the trust picture (each is code with
access to decrypted entries), so this
guide's verified ground is the stock
command set, and extensions are named
here from the manual so you know the
door exists before you open it.
