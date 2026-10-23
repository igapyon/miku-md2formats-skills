# Development Notes

## Initial scaffold

- Date: 2026-10-23
- Repository: `miku-md2formats-skills`
- Installed skill: `igapyon-miku-md2formats`
- Delivery pattern: CLI-backed Agent Skill with four bundled Node.js
  single-file runtimes and three Java executable jars.
- Sister-project reference: local `miku-ms-office-skills` checkout at commit
  `1b2dbd8ea7e7d7681664a0f12a954b61d8130e17`. It provided the multi-tool
  bundle, runtime pin, installable ZIP, and release workflow shape. The new
  package omits office-to-Markdown converters.
- Shared miku-soft workflow: `igapyon-miku-soft-developer` Agent Skills
  workflow was checked on 2026-10-23. The installed skill checkout has no Git
  metadata, so no skill commit ID is available.
- Generated `index.json` is not adopted: routing is handled by a small
  hand-authored format map and no consumer requires an inventory.
- Following the `miku-ms-office-skills` activation pattern, explicit names of
  any supported converter activate this aggregate skill. Generic Markdown or
  Office conversion requests remain outside its activation scope.

## Runtime compatibility anchors

See `skills/igapyon-miku-md2formats/references/upstreams.md` for upstream
repositories, release tags, source commit IDs, release asset links, and
SHA-256 digests. Digests were checked against GitHub Releases metadata on
2026-10-23 and against the staged artifacts.

The three office converter source checkouts had unrelated local edits to
`package.json` and `package-lock.json` while their tagged release artifacts
were copied from the existing `miku-ms-office-skills` runtime collection.
Those edits were left untouched. The `miku-md2html` checkout was clean at the
selected release tag.

## Java runtime intake (2026-10-23)

This update adds the Java DOCX, XLSX, and PPTX CLI companions to the same
installable skill. `miku-md2html` remains Node.js-only because it has no Java
companion. The Java runtime pins match the corresponding Node.js release
versions:

- `miku-md2docx-java` `v1.1.0`, source commit
  `321e180979663e9e88d9d15975129bfdf39042ac`
- `miku-md2xlsx-java` `v0.10.0`, source commit
  `7dd2842648baf77f538bf7c4774adaa0b5d17479`
- `miku-md2pptx-java` `v0.7.0`, source commit
  `f42d55354a50544343d0fde61ae2c7273a48e6c7`

The executable jars were reused from the local `miku-ms-office-skills`
runtime collection. Their SHA-256 values matched the corresponding GitHub
Release asset digests queried on 2026-10-23. The upstream Java projects declare
Java 8 source/target compatibility. Node.js remains the default backend; the
skill uses Java when requested, or when Node.js 22 or newer is unavailable and
Java is available. CI and release workflows now set up Java 8 before checking
the bundle.

## Verification

The upstream CLIs' `--help` output was inspected while writing the per-format
route and runtime policy. `npm run build` runs the repository tests, stages the
installable ZIP, and checks each bundled CLI's `--version` output from the
staged archive. It does not run the full conversion matrix.

On 2026-10-23, all seven runtimes were manually exercised with a representative
Markdown conversion from a working directory outside the skill directory.
Local-image handling was also checked for the formats that support it. The
check used Node.js 26.12.0 and OpenJDK 17.0.21; the minimum supported Node.js
22 and Java 8 versions were not tested locally. The Java XLSX 0.10.0
underscore-loss limitation is documented in the skill references; no later
upstream Java XLSX release was available at the time of review.

The release ZIP verifier requires `SKILL.md` and all three task references in
addition to every runtime declared in `package.json`. Its test removes a
required reference from a copied ZIP and checks that verification fails. After
the review fixes, `npm run build` passed on 2026-10-23: both repository tests
passed, and the staged ZIP passed required-file and seven-runtime version
checks.
