# The Neighbours

pass sits in a crowded field. These are
the tools a reader is most likely to be
choosing between, characterised from
their designs and documentation — none
was benchmarked against pass for this
guide, and the honest differences are
about shape, not speed.

## KeePassXC — one encrypted file, with a window

The closest relative in spirit: your
passwords in a single database file
(KDBX) that you hold, opened by one
master password (optionally plus a key
file or a hardware key). Where pass is
one file per entry and no interface,
KeePassXC is one file for everything
and a polished graphical one — folders,
icons, a built-in generator, browser
integration, autofill. Its database is a
*format*, readable by a family of
compatible apps on every platform,
including phones. Choose it if the
people using the store want a window
and a phone app; choose pass if you
want every entry to be a file your
other tools can see. (A sibling guide
covers KeePassXC itself.)

## Bitwarden and Vaultwarden — the hosted vault

A different species: an account on a
server, clients on every device,
synchronisation and sharing as
first-class features, organisations
with roles and collections. Bitwarden
runs it as a service; Vaultwarden is a
self-hosted server speaking the same
protocol to the same apps. This is the
right shape when several non-technical
people must share logins and when
phones are the main computer. The price
is the thing pass never has: a service
that must run, be patched, and be
trusted with the *existence* of your
vault, and a company (or a server of
yours) in the loop. pass's sharing
(chapter 11) is a deliberate miniature
of this — a folder and two keys —
without accounts or infrastructure.

## gopass — pass, grown

A rewrite of pass's idea in Go, largely
compatible with its stores, adding
multi-store mounts, team features,
a proper configuration system, and
friendlier errors. If the pass model
delights you but the shell script's
edges chafe — chapter 11's silent
failure among them — gopass is the
upgrade path that keeps your files.
Existing pass stores open in it, which
says most of what matters about both.

## age — not a manager, the other half of one idea

age encrypts *files* for recipients,
beautifully and with almost no
configuration, and this series covers
it as the modern heir to GPG for that
one job. It has no store, no names, no
Git — people build pass-like habits on
top of it, and secret-management tools
for teams (SOPS and friends) use age
(and GPG) as their encryption layer.
Mentioned here because readers ask:
pass could not simply "use age instead"
without becoming a different, younger
project and leaving GPG's key world —
signatures, chapter 11's validity web —
behind.

## The browser's own manager

Chrome, Firefox and Safari will store
passwords, sync them through your
browser account, and autofill them with
zero setup — which is why most people
use it. For many people it is honestly
enough. What it is not: yours as files,
usable outside the browser, separable
from the browser account, or able to
hold the non-web secrets (routers,
keys, door codes) that chapter 7's
entries carry. The usual grown-up
arrangement is both: browser manager
for convenience on the web, a real
store — pass or a neighbour — as the
record of truth.

## The rule of thumb

One person, terminals, files, Git:
pass. One person, windows and phones:
KeePassXC. Several people, accounts,
support: Bitwarden or Vaultwarden.
The pass model with training wheels
removed and some added: gopass.
