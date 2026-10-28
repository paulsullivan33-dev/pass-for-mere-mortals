# Moving Things Around

Entries are files, so organising a store
is file management with encryption-aware
manners. Three verbs do the work —
`mv`, `cp`, `rm` — and because the store
is a folder, they behave the way their
Unix namesakes taught you to expect,
with two additions: pass keeps the
recipients right when entries cross
folders, and it asks before deleting.

The lab's running example (transcript
`pass-lab/transcripts/t07-organise.txt`)
begins with a new entry, added by script
the way chapters 5 and 6 allowed:

```text
$ printf 'velvet-11-compass\nvelvet-11-compass\n' | pass insert shopping/example-shop
mkdir: created directory '~/.password-store/shopping'
```

## Rename: mv

```text
$ pass mv shopping/example-shop shopping/example-store
~/.password-store/shopping/example-shop.gpg
renamed '~/.password-store/shopping/example-shop.gpg' -> '~/.password-store/shopping/example-store.gpg'
```

The output gives the design away: this
is the filesystem's own rename, reported
in the `mv` tool's words. Renaming within
a folder is exactly that cheap. Moving an
entry *between* folders whose `.gpg-id`
files differ (chapter 2) is the one case
with more machinery: pass re-encrypts
the entry for its new home's recipients,
so an entry never ends up readable by
the wrong folder's keys. That cross-folder
case is described from the manual; every
folder in the lab shares one recipient
list.

## Copy: cp

```text
$ pass cp email/example-mail email/example-mail-old
~/.password-store/email/example-mail.gpg
'~/.password-store/email/example-mail.gpg' -> '~/.password-store/email/example-mail-old.gpg'
```

A copy is a second encrypted file with
the same contents — the standard trick
before changing a password on a service
you cannot afford to be locked out of:
keep `example-mail-old` until the new
password is proven, then delete it. The
store, mid-chapter, with the copy in
place:

```text
$ pass ls
Password Store
|-- banking
|   `-- example-bank
|-- email
|   |-- example-mail-old
|   `-- example-mail
|-- shopping
|   `-- example-store
|-- social
|   `-- example-social
|-- web
`-- wifi
    `-- home-router
```

(The empty `web` folder is chapter 5's
mismatch, still lingering — folders are
just directories, and pass does not tidy
them for you.)

## Delete: rm

Deletion is the one verb that argues:

```text
$ pass rm email/example-mail-old
Are you sure you would like to delete email/example-mail-old? [y/N] y
removed '~/.password-store/email/example-mail-old.gpg'
```

The default answer is No, and
`pass rm --force` exists for scripts
that have already decided. One warning
belongs here, and chapter 15 expands it:
if Git tracking is on (chapter 10),
deleting an entry removes the file but
not the history — the old password
remains readable in old commits to
anyone holding the key. Deletion tidies
the present; it does not rewrite the
past. Plan sensitive removals with that
in mind.
