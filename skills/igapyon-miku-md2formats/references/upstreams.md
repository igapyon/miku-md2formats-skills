# Upstream Runtime Artifacts

These are the self-contained Node.js and Java CLI artifacts bundled by this
skill.
`miku-md2html` is the upstream HTML tool and its repository is the source for
the HTML behavior. The DOCX, XLSX, and PPTX CLIs are separate upstream
products; this Agent Skill routes to them and does not merge their converter
implementations into a new `miku-md2formats` CLI. The six existing Office
runtime hashes were checked against GitHub Releases metadata on 2026-10-23.
The Node.js and Java HTML v0.5.0 hashes were checked against Release metadata
and the bundled files on 2026-10-24.

| Tool | Repository | Release / source commit | Artifact source | SHA-256 |
| --- | --- | --- | --- | --- |
| `miku-md2html` | <https://github.com/igapyon/miku-md2html> | `v0.5.0` / `c397569016383c622aae4b187234e56f08a753fd` | [miku-md2html-0.5.0.mjs](https://github.com/igapyon/miku-md2html/releases/download/v0.5.0/miku-md2html-0.5.0.mjs) | `c5af81eccba1ecddc348533e40517a2dbcfa92af552b6d7a02616421fcbc9b78` |
| `miku-md2html-java` | <https://github.com/igapyon/miku-md2html-java> | `v0.5.0` / `ba962e66e9243fa9a45ba95f85af06fca0eca4ec` | [miku-md2html-java-0.5.0.jar](https://github.com/igapyon/miku-md2html-java/releases/download/v0.5.0/miku-md2html-java-0.5.0.jar) | `656de4e42c67afc6b8518c49b46ba6fcc4c6eeac8c99016d44e958bec7e6ebcd` |
| `miku-md2docx` | <https://github.com/igapyon/miku-md2docx> | `v1.1.0` / `a25d302c742e6950183d60e0cda88f97bc65a265` | [miku-md2docx-1.1.0.mjs](https://github.com/igapyon/miku-md2docx/releases/download/v1.1.0/miku-md2docx-1.1.0.mjs) | `fc9557b737f8d156b08d28d989ce0c9608965fb7e8c7ae92d6c44205890b5fee` |
| `miku-md2docx-java` | <https://github.com/igapyon/miku-md2docx-java> | `v1.1.0` / `321e180979663e9e88d9d15975129bfdf39042ac` | [miku-md2docx-java-1.1.0.jar](https://github.com/igapyon/miku-md2docx-java/releases/download/v1.1.0/miku-md2docx-java-1.1.0.jar) | `9dfe021ea83af244046fe35545441157df9641db69ae4e4a9de33341a90a7f54` |
| `miku-md2xlsx` | <https://github.com/igapyon/miku-md2xlsx> | `v0.10.0` / `2337bd26226a73d2ada0d6f957d77044c8311b08` | [miku-md2xlsx-0.10.0.mjs](https://github.com/igapyon/miku-md2xlsx/releases/download/v0.10.0/miku-md2xlsx-0.10.0.mjs) | `64c1d7b4da4f05dd2a72176c1872c6ed0ba6dc219ea3020671edc60d11194b61` |
| `miku-md2xlsx-java` | <https://github.com/igapyon/miku-md2xlsx-java> | `v0.10.0` / `7dd2842648baf77f538bf7c4774adaa0b5d17479` | [miku-md2xlsx-java-0.10.0.jar](https://github.com/igapyon/miku-md2xlsx-java/releases/download/v0.10.0/miku-md2xlsx-java-0.10.0.jar) | `e7dc5238808a73abe6c38fdb835ed008d355e119e86f5d4345c659924c9b4cb7` |
| `miku-md2pptx` | <https://github.com/igapyon/miku-md2pptx> | `v0.7.0` / `dd59e38c2b608e2e521aa4b941b8ab05543b9716` | [miku-md2pptx-0.7.0.mjs](https://github.com/igapyon/miku-md2pptx/releases/download/v0.7.0/miku-md2pptx-0.7.0.mjs) | `e736cdd0801a1d2e273c0f31eef5f0012826832e9b7f30d541752c21b50318c9` |
| `miku-md2pptx-java` | <https://github.com/igapyon/miku-md2pptx-java> | `v0.7.0` / `f42d55354a50544343d0fde61ae2c7273a48e6c7` | [miku-md2pptx-java-0.7.0.jar](https://github.com/igapyon/miku-md2pptx-java/releases/download/v0.7.0/miku-md2pptx-java-0.7.0.jar) | `1a00f9bf484a12dd9920e935dd0a41957c770bb37b29690a81fe3870ee2e1b81` |

The release asset is the source of each bundled executable. The source commit
records the upstream checkout used for compatibility. The Java HTML v0.5.0
implementation is based on Node `miku-md2html` v0.4.0, commit
`b18a6e390425bd0e15387a2d608980a42a1809e9`; the matching version numbers do
not assert full feature parity with Node v0.5.0. Read each selected CLI's
`--help`, verify its release digest, and run the bundle/conversion smoke checks
when refreshing a pin.

The Java converter repositories declare Java 8 source/target compatibility.
Both HTML backends reference local raster images by default and support
`--image-mode embed` for embedding them in the generated HTML. Their CLI
details and implementation behavior may differ; check the selected backend's
`--help` output.

## Relevant upstream limits

- HTML: output embeds CSS, viewer JavaScript, and SVG. Local raster images are
  referenced by default; use `--image-mode embed` for a portable HTML file
  containing them. Remote image URLs are rejected. Existing output is
  protected unless `--force` is supplied.
- DOCX: output is editable and structural rather than pixel-perfect. Remote
  images are not downloaded; SVG, table alignment, and merged cells are not
  converted. Existing `--out` files are overwritten. Pass `--summary` to
  report missing images and other conversion diagnostics on stdout; the summary
  is human-readable rather than a stable machine-readable API.
- XLSX: output focuses on practical structure. Cell values remain strings;
  formulas, charts, drawings, and conditional formatting are not generated.
  Existing `--out` files are overwritten and success writes no stdout.
- PPTX: headings level 1 and 2 start slides; text blocks, simple tables, links,
  notes, and supported local images are supported. Remote/absolute/outside
  image paths are skipped with warnings. Layout is structural, not
  pixel-perfect. Existing `--out` files are overwritten.
- Java DOCX/XLSX/PPTX converters are separate implementations. Their supported
  Markdown syntax and generated package details may differ from the matching
  Node.js versions; consult the selected Java upstream's README when those
  details matter.
- Java XLSX `0.10.0` currently treats a lone `_` as an italic delimiter even
  when it appears in plain text. A value such as `REVIEW_MARKER` is emitted as
  `REVIEWMARKER`. If literal underscores must be preserved, use the Node.js
  `miku-md2xlsx` runtime. No newer Java release was published as of
  2026-10-23 to replace this behavior.
