# bork-runtime

The Gecko runtime for [bork](https://github.com/100mountains/bork) — a Firefox
source tree carrying bork's branding and a small set of patches, built into the
platform binaries the browser ships on.

This repo is the *engine*. The browser itself — workspaces, tab stacks, zen
mode, the side panels, everything you actually see — lives in
[100mountains/bork](https://github.com/100mountains/bork) and is layered on top
of what gets built here.

## What is ours

| Path | What |
|------|------|
| `browser/branding/bork/` | icons, wordmarks, `brand.ftl`, `configure.sh` |
| `.github/assets/config/moz.configure` | `MOZ_APP_VENDOR` / `MOZ_APP_PROFILE` |
| `.github/workflows/mozconfigs/` | per-platform build configuration |
| `.github/patches/upstream/` | patches applied over the Firefox source |

Everything else is Firefox.

## Upstream syncs overwrite almost everything

When the pull-upstream action runs, the entire tree **except**

```js
[".git/**/*", ".github/**/*", ".gitmodules", "noraneko/**/*"]
```

is replaced by current Firefox source. Anything added outside those paths is
lost on the next sync. Put additions under `.github/`, or express them as a
patch in `.github/patches/upstream/` so they are reapplied rather than
overwritten.

## Building

Local macOS builds source the environment first, then use mach as normal:

```bash
source /path/to/macbuildenv.sh
./mach build
```

The multi-platform release chain is driven from the bork repo by
`tools/release/build-all-runtimes.sh`, which publishes tagged runtime artifacts
that `build-bork-packages.sh` then overlays the feature layer onto.

## Lineage

Forked from [Noraneko Runtime](https://github.com/nyanrus/noraneko-runtime) by
[NyanRus](https://github.com/nyanrus), itself a build of the Mozilla Firefox
source.

Not affiliated with, endorsed by, or connected to Mozilla, Ablaze, the Floorp
project or Noraneko.

## License

[Mozilla Public License 2.0](./.github/LICENSE) for the non-Firefox sources.
[Firefox's own license](./LICENSE) for everything upstream.
