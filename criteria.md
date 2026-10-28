# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's accounts belong to fictional services at a
   reserved example domain.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 15 names what the tool
   cannot hide or survive: entry names in plain text, one key
   and one passphrase as a single point of total loss, Git
   history retaining every old password, sharing that cannot
   be recalled, and a threat model that ends at the machine's
   edge.
8. **The rule is in the text, early.** Chapter 1 and the README
   state the rule plainly — *guard the key and its passphrase,
   and practise on invented passwords* — before any procedure
   depends on it, and chapters 11 and 15 return to it where
   the tooling would make crossing the line easy.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox.
Everything ran inside a throwaway scratch directory: a private
`HOME`, private GPG keyrings, and a store that never left the
scratch. The Ubuntu `pass` **1.7.4** package was downloaded and
*unpacked* into the scratch rather than installed system-wide;
the script the transcripts exercise is the package's own,
unmodified. Its companions were the sandbox's **GnuPG 2.4.4**,
**Git 2.43.0**, **tree 2.1.1** and **xclip**. Both lab keys were
generated in the scratch with an invented passphrase
(`lab-only-passphrase`, printed in the transcripts because it
protects nothing real), and every password in the store is an
invented value for a fictional service. The throwaway keys and
store remain in the scratch directory; only the transcripts
were copied into this repository. All transcripts live in
`pass-lab/transcripts/`. Two presentational notes: the lab's
absolute scratch paths were mechanically shortened in the
transcripts (`~` for the lab's home, `~/pass-lab` for the rest
of the scratch), and terminal colour escapes were stripped —
the words, values and exit codes are the programs' own.

Run for real, with transcripts as evidence:

- `pass version`, and the versions of GPG, Git and tree the
  lab ran against (t00).
- Batch key generation for both lab keys; public and secret
  listings, fingerprints read back (t01, t10).
- `pass init`; `.gpg-id` contents; the empty store's listing
  and permissions (t02).
- Interactive `pass insert` under a terminal, prompts
  included; `pass show`; the mismatched-typing refusal, with
  nothing stored (t03). Scripted inserts (two lines piped)
  throughout later transcripts.
- The store on disk: file listing, `file` reporting raw data,
  a `grep` for a known password finding nothing (exit 1), and
  the same file decrypted by plain `gpg` with no pass
  involved (t04).
- `pass generate` at two lengths, with and without symbols
  (t05).
- Multiline insert (`-m`) and `pass edit` with a script
  standing in for the editor, shown by before-and-after
  `pass show` (t06).
- `pass mv`, `pass cp`, `pass rm` with its confirmation
  prompt; the full tree listing between operations (t07).
- `pass find` on names, `pass grep` on contents, and the
  first-line idiom for password-only output (t08).
- `pass git init` failing to commit without a Git identity
  **while exiting 0**, verified by `pass git status`; the
  recovery commit; automatic commits on insert; `pass git
  log`; a bare `file://` remote; push; a clone read back
  through `PASSWORD_STORE_DIR` (t09).
- Sharing: import of a second public key; a two-key
  `pass init` whose re-encryption **silently failed** (GPG's
  "no assurance" warning per entry, exit 0), proven by a
  one-packet count and by the second key's failed decrypt;
  the ownertrust route tried and found insufficient on its
  own (recorded in chapter 11, not kept as a transcript);
  a local signature (`--quick-sign-key`) making the key
  `[ full ]`; the repeated `pass init` re-encrypting all six
  entries with a matching Git commit; a two-packet count;
  and the second keyring decrypting the entry for real (t10).
- The clipboard's headless failure: `pass show -c` refusing
  with "No X11 or Wayland display detected", exit 1, xclip
  present, `DISPLAY` empty (t11).
- A second store under `PASSWORD_STORE_DIR`, initialised,
  filled and listed, then the default store again (t13).
- The failure set, produced deliberately: unknown entry
  (exit 1), missing secret key, wrong passphrase, and a
  byte-corrupted entry file caught by GPG's checksum
  (exit 2 each) (t12).

Documented only — described from the project's manual and
documentation, not run, and the text does not claim otherwise:

- The `sudo apt install pass` route itself and other
  platforms' packages (the lab ran the same Ubuntu package
  unpacked, per above); interactive `gpg --full-generate-key`
  interviews (the lab used the batch form of the same
  operation).
- The clipboard's success path — copy, paste, and the
  45-second self-restore (`PASSWORD_STORE_CLIP_TIME`) — and
  the Wayland variant; only the no-display failure was
  runnable on a display-less machine.
- `pass generate --force` / `--in-place`, `pass insert
  --echo`, QR-code display, and per-folder `.gpg-id` files.
- Cross-folder moves that re-encrypt to a different folder's
  recipients (every lab folder shared one recipient list).
- Extensions, including `pass-otp` and the import converters
  the package ships.
- Hosted Git remotes over SSH/HTTPS and moving a secret key
  between machines (the store half of that move — clone and
  read — was run; the key half is plain GPG export/import,
  described in chapter 13).
- The comparison tools in chapter 14 are characterised, not
  benchmarked.

Fixtures are this guide's own and deliberately generic:
fictional services (`example-mail`, `example-bank`, a home
router) at the reserved `example.invalid` domain, two invented
people ("Lab Keeper", "Lab Teammate") who exist only as lab
keys, and passwords invented or generated inside the lab.
No example is modelled on any real person, service or
organisation, and no real credential of any kind appears
anywhere in the work.
