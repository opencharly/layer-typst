# layer-typst

The [Typst](https://typst.app) document processor for OpenCharly images — a
markup-based typesetting system that compiles `.typ` source into PDF.

The `typst` candy fetches the pinned Typst release for the build arch (the
`${BUILD_ARCH}-unknown-linux-musl` static tarball) from GitHub releases and
installs the single `typst` binary at `/usr/local/bin/typst`. The version is
pinned to a concrete tag (not `latest`) because the `download:` verb
content-addresses its cache by the sha256 of the URL — with `latest` the URL is
constant while the artifact behind it changes.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `typst` |
| Binary | `/usr/local/bin/typst` |
| Version | pinned `v0.15.1` (`TYPST_VERSION` var) |
| Install | `download:` the GitHub release tarball, `strip_components: 1` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-docs-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-typst:v2026.240.1854'
```

Then, inside the built image:

```bash
typst --version                  # typst 0.15.1
typst compile doc.typ doc.pdf    # compile markup into a PDF
```

The candy's `plan:` asserts the binary at the fixed path, `typst --version`
printing a parseable version, and `typst compile` turning a minimal `.typ` source
into a real PDF whose bytes start with the `%PDF` magic.

## Layout

- `charly.yml` — the `typst:` candy entity (the `TYPST_VERSION` var, the
  `download:`/extract `plan:` step, the `check:` assertions) and the embedded
  `typst-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:typst`
- `/charly-tools:vscode` — editor sibling
- `/charly-image:layer` — candy authoring reference
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
