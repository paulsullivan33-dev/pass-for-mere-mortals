# Entries with More Inside

A password on its own ages badly. Six
months later you need the user name that
goes with it, the address of the login
page, the answer to which of your three
email addresses this account thinks you
are. pass has a convention for this,
stated in chapter 1 and worth repeating
as a rule:

> **The first line of an entry is the
> password. Every line after it is
> yours.**

Tools rely on the rule — the clipboard
in chapter 12 copies the first line
only — so keep line one sacred and put
everything else below it.

## Writing a whole entry at once

`pass insert --multiline` (short form
`-m`) reads the entry until you end it —
at a terminal, with Ctrl-D. The lab fed
its home-router entry from a script, so
the transcript shows the prompt but not
the typing (transcript
`pass-lab/transcripts/t06-multiline-edit.txt`):

```text
$ pass insert -m wifi/home-router
mkdir: created directory '~/.password-store/wifi'
Enter contents of wifi/home-router and press Ctrl+D when finished:

$ pass show wifi/home-router
basement-router-88
user: admin
url: http://192.168.0.1
```

The format of the extra lines is yours
to choose; the `name: value` shape above
is a common one because it greps well in
chapter 8. Note what this entry is: a
router's admin password, the account
name, and the address of its settings
page — three facts that travel together,
kept together, encrypted together.

## Editing an entry

`pass edit` decrypts the entry into a
temporary file, opens your editor on it,
and re-encrypts when you save and quit.
The editor is the one named by the
`EDITOR` environment variable. The lab
has no typist, so a small script stood
in for the editor — it appended one line
to whatever file it was handed — which
makes the mechanics fully visible:

```text
$ EDITOR=<append-a-line script> pass edit wifi/home-router
$ pass show wifi/home-router
basement-router-88
user: admin
url: http://192.168.0.1
note: spare ethernet cable lives in the linen cupboard
```

The edit produced no output of its own;
the proof is the before-and-after
`pass show`. With a human editor the
same silence applies: save and quit,
and the entry is re-encrypted in place —
with a Git commit, if chapter 10 is
turned on, saying the entry was updated.
Two safety properties ride along. The
temporary file lives in a private
temporary folder and is overwritten
after use, and if the editor exits
without changes, pass leaves the entry
untouched. (The wiping behaviour is
stated from pass's documentation; the
lab verified the edit itself, not the
overwrite.) One honest warning belongs
here: while the editor is open, your
entry exists in plain text in that
temporary file. On your own machine that
is a reasonable bargain; it is one more
reason pass belongs on machines you
control.
