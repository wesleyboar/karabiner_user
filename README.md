# Karabiner-Elements config

## `_config.json` is the source of truth

`_config.json` is the version-controlled copy of the Karabiner-Elements
profile. The file Karabiner actually reads is:

    ~/.config/karabiner/karabiner.json

**Both are real files, and both must be updated together.** After editing
either one, copy it over the other:

```sh
# repo -> live (after editing _config.json here)
cp _config.json ~/.config/karabiner/karabiner.json

# live -> repo (after changing something in the Karabiner UI)
cp ~/.config/karabiner/karabiner.json _config.json
```

Karabiner reloads `karabiner.json` automatically on change.

### Why not a symlink

Both directions fail:

- **Live -> repo** (repo file is the symlink): git stores a symlink as a
  link, not as the file it points to. `_config.json` would be committed as
  a path string and the config would have no history. Commit `c614605`
  ("commit config as real file not symlink") was undoing exactly this.
- **Repo -> live** (live file is the symlink): Karabiner rewrites
  `karabiner.json` whenever anything changes in its UI, and can replace the
  symlink with a real file. The two then diverge silently.

A hard link doesn't help either. Hard links have no direction — both names
point at the same inode — so there is no "safe" way round. If Karabiner
writes atomically (temp file plus rename), the new content lands on a new
inode and the other name silently keeps the old one.

Copying is the only arrangement where divergence is visible: `git status`
and `diff` both show it.

## Rule order

See [order.md](order.md) — `(WES) CAPSLOCK` must be first.

## Rule fragments

The other `*.json` files are standalone copies of individual complex
modification rules, kept for reference and for pasting back into
`_config.json`. They are not read by Karabiner.
