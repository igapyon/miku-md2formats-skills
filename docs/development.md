# Development Notes

## Initial scaffold

- Date: 2026-10-23
- Repository: `miku-md2formats-skills`
- Installed skill: `igapyon-miku-md2formats`
- Delivery pattern: CLI-backed Agent Skill with four bundled Node.js
  single-file runtimes and four Java executable jars.
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
2026-10-24 and against the staged artifacts. The temporary source-built
`miku-md2html` v0.3.5 artifact described below has been superseded by official
v0.5.0 Node.js and Java Release assets.

The three office converter source checkouts had unrelated local edits to
`package.json` and `package-lock.json` while their tagged release artifacts
were copied from the existing `miku-ms-office-skills` runtime collection.
Those edits were left untouched. The `miku-md2html` checkout was clean at the
selected release tag.

## Initial Java runtime intake (2026-10-23)

This update added the Java DOCX, XLSX, and PPTX CLI companions to the same
installable skill. At that time `miku-md2html` had no Java companion. The Java
runtime pins matched the corresponding Node.js release versions:

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

## Temporary `miku-md2html` v0.3.5 intake (2026-10-23)

The upstream `v0.3.5` Release had no attached assets. Its release workflow
failed in `npm test`. The tag points to commit
`065c828dc6e987a39bdd7a4a15ea48a97ca2eccd`; at the user's direction, the CLI
was built locally from that tag and copied into the skill as
`miku-md2html-0.3.5.mjs`.

`npm ci`, `npm run build:bundle`, `npm run smoke:bundle`,
`npm run smoke:runtime`, `npm run release:stage`, and
`npm run smoke:release-assets` passed. The upstream `npm test` type-check passed
but 3 of 103 tests failed in `tests/cli-failures.test.ts`, all around writing
failure reports. The staged CLI reported `miku-md2html 0.3.5`, its `--help`
output was reviewed, and its SHA-256 is
`61d9f1569eba2a65147025665502f69987d2f05125c0cb9591a2b23adc87f192`. This is a
local build digest, not a digest published by the upstream Release. This
temporary artifact was superseded on 2026-10-24 by the official v0.5.0 Node.js
and Java Release assets documented below.

## `miku-md2html` Node.js and Java v0.5.0 intake (2026-10-24)

Both runtimes were downloaded from their GitHub Release asset URLs. Their
SHA-256 digests matched the Release metadata:

- Node.js CLI `miku-md2html-0.5.0.mjs`, source commit
  `c397569016383c622aae4b187234e56f08a753fd`, SHA-256
  `c5af81eccba1ecddc348533e40517a2dbcfa92af552b6d7a02616421fcbc9b78`.
- Java executable `miku-md2html-java-0.5.0.jar`, source commit
  `ba962e66e9243fa9a45ba95f85af06fca0eca4ec`, SHA-256
  `656de4e42c67afc6b8518c49b46ba6fcc4c6eeac8c99016d44e958bec7e6ebcd`.

The Java README identifies Node `miku-md2html` v0.4.0, commit
`b18a6e390425bd0e15387a2d608980a42a1809e9`, as its compatibility source and
does not claim complete feature parity with Node v0.5.0. Both `--help` outputs
were reviewed: local raster images are referenced by default, and
`--image-mode embed` embeds them. Each CLI passed `--version` and converted
`skills/igapyon-miku-md2formats/references/INDEX.md` to a non-empty HTML file
without diagnostics.

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
checks. After the v0.5.0 intake and package version increment,
`npm run build` passed with all eight runtime version checks and the 0.3.4 ZIP.
