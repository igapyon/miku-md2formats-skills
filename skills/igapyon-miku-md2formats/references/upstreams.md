# Upstream Runtime Artifacts

These are the self-contained Node.js and Java CLI release assets bundled by
this skill.
`miku-md2html` is the upstream HTML tool and its repository is the source for
the HTML behavior. The DOCX, XLSX, and PPTX CLIs are separate upstream
products; this Agent Skill routes to them and does not merge their converter
implementations into a new `miku-md2formats` CLI. SHA-256 values for all
artifacts were checked against GitHub Releases metadata on 2026-10-23 and
against the staged runtime files.

| Tool | Repository | Release / source commit | Release asset | SHA-256 |
| --- | --- | --- | --- | --- |
| `miku-md2html` | <https://github.com/igapyon/miku-md2html> | `v0.3.2` / `d11945b62a9cec0044fd5ebb9f33e8b27c2224b4` | [miku-md2html-0.3.2.mjs](https://github.com/igapyon/miku-md2html/releases/download/v0.3.2/miku-md2html-0.3.2.mjs) | `bad33db78dc31853bae45a900166bfd5a74b09459d193e233393ffc6f7c6f0e2` |
| `miku-md2docx` | <https://github.com/igapyon/miku-md2docx> | `v1.1.0` / `a25d302c742e6950183d60e0cda88f97bc65a265` | [miku-md2docx-1.1.0.mjs](https://github.com/igapyon/miku-md2docx/releases/download/v1.1.0/miku-md2docx-1.1.0.mjs) | `fc9557b737f8d156b08d28d989ce0c9608965fb7e8c7ae92d6c44205890b5fee` |
| `miku-md2docx-java` | <https://github.com/igapyon/miku-md2docx-java> | `v1.1.0` / `321e180979663e9e88d9d15975129bfdf39042ac` | [miku-md2docx-java-1.1.0.jar](https://github.com/igapyon/miku-md2docx-java/releases/download/v1.1.0/miku-md2docx-java-1.1.0.jar) | `9dfe021ea83af244046fe35545441157df9641db69ae4e4a9de33341a90a7f54` |
| `miku-md2xlsx` | <https://github.com/igapyon/miku-md2xlsx> | `v0.10.0` / `2337bd26226a73d2ada0d6f957d77044c8311b08` | [miku-md2xlsx-0.10.0.mjs](https://github.com/igapyon/miku-md2xlsx/releases/download/v0.10.0/miku-md2xlsx-0.10.0.mjs) | `64c1d7b4da4f05dd2a72176c1872c6ed0ba6dc219ea3020671edc60d11194b61` |
| `miku-md2xlsx-java` | <https://github.com/igapyon/miku-md2xlsx-java> | `v0.10.0` / `7dd2842648baf77f538bf7c4774adaa0b5d17479` | [miku-md2xlsx-java-0.10.0.jar](https://github.com/igapyon/miku-md2xlsx-java/releases/download/v0.10.0/miku-md2xlsx-java-0.10.0.jar) | `e7dc5238808a73abe6c38fdb835ed008d355e119e86f5d4345c659924c9b4cb7` |
| `miku-md2pptx` | <https://github.com/igapyon/miku-md2pptx> | `v0.7.0` / `dd59e38c2b608e2e521aa4b941b8ab05543b9716` | [miku-md2pptx-0.7.0.mjs](https://github.com/igapyon/miku-md2pptx/releases/download/v0.7.0/miku-md2pptx-0.7.0.mjs) | `e736cdd0801a1d2e273c0f31eef5f0012826832e9b7f30d541752c21b50318c9` |
| `miku-md2pptx-java` | <https://github.com/igapyon/miku-md2pptx-java> | `v0.7.0` / `f42d55354a50544343d0fde61ae2c7273a48e6c7` | [miku-md2pptx-java-0.7.0.jar](https://github.com/igapyon/miku-md2pptx-java/releases/download/v0.7.0/miku-md2pptx-java-0.7.0.jar) | `1a00f9bf484a12dd9920e935dd0a41957c770bb37b29690a81fe3870ee2e1b81` |

The release asset is the source of the bundled executable. The source commit
records the release tag's upstream checkout used for compatibility. When
refreshing a pin, verify the release asset and digest, read its `--help`
output, update the artifact filename and this table, and run the complete
bundle/conversion smoke checks.

The Java converter repositories declare Java 8 source/target compatibility.
`miku-md2html` currently has no Java companion; its Node.js runtime is the only
HTML backend in this skill.

## Relevant upstream limits

- HTML: output is a single offline-capable HTML file. Remote image URLs are
  rejected; supported local images can be embedded. Existing output is
  protected unless `--force` is supplied.
- DOCX: output is editable and structural rather than pixel-perfect. Remote
  images are not downloaded; SVG, table alignment, and merged cells are not
  converted. Existing `--out` files are overwritten.
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
