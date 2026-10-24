# Handoff

## Current state

- Repository: `miku-md2formats-skills`, package version `0.3.6`.
- Installed skill: `igapyon-miku-md2formats`.
- Four output formats are covered by eight pinned self-contained runtimes:
  Node.js and Java for HTML, DOCX, XLSX, and PPTX.
- HTML uses `miku-md2html` and `miku-md2html-java` `v0.5.0`. The Java release
  is based on Node v0.4.0 and does not claim full feature parity with Node
  v0.5.0. Both backends reference local raster images by default; use
  `--image-mode embed` to embed them.
- Runtime provenance, source commits, release links, and SHA-256 values are in
  `skills/igapyon-miku-md2formats/references/upstreams.md`.

## Review remediation

- The routing guide explains that the runtime path comes from the directory
  containing `SKILL.md`, while the converter runs with the user's project as
  its working directory. The command examples use an absolute skill path.
- The release ZIP verifier requires `SKILL.md`, all three skill references,
  and all eight runtime artifacts. A regression test removes a required
  reference from a copied ZIP and checks that verification fails.
- Java XLSX `0.10.0` removes literal underscores in plain text in some cases;
  `REVIEW_MARKER` can become `REVIEWMARKER`. This is documented in the skill
  and upstream reference. No later upstream release was available on
  2026-10-23. Prefer the Node.js XLSX runtime when underscores must be kept.

## Verification record

- A representative Markdown conversion was manually run through all seven
  original runtimes on 2026-10-23 from a working directory outside the skill
  folder. Local-image handling was also checked for the supported formats.
- That manual check used Node.js `26.12.0` and OpenJDK `17.0.21`; the minimum
  supported Node.js 22 and Java 8 versions were not tested locally.
- `npm run build` passed on 2026-10-23 for the original seven-runtime package:
  both repository tests passed, the required-file regression check rejected
  the altered ZIP, and the staged ZIP passed every `--version` check.

## Release preparation

- The release will use the currently pinned Java XLSX `0.10.0` jar. Its
  underscore-loss limitation remains disclosed and tracked by upstream
  [Issue #11](https://github.com/igapyon/miku-md2xlsx-java/issues/11).
- At package version `0.3.4`, `npm run build` regenerated the matching ZIP;
  both tests passed and all eight runtimes passed their `--version` checks.
- At package version `0.3.5`, `npm run build` passed with both tests and
  generated the ZIP after the DOCX diagnostic and agent-metadata changes.
- At package version `0.3.6`, `npm run build:bundle:zip` regenerated
  `bundle/igapyon-miku-md2formats-skills-0.3.6.zip` from the current working
  tree.
- Local version is `0.3.6`; the expected matching tag is `v0.3.6`. Ensure the
  tag points to the merged release commit containing this bundle. GitHub
  Release publication remains a human UI step.

## Follow-up

When upstream publishes a Java XLSX release that preserves literal
underscores, review and pin it, verify the behavior, then remove or revise the
limitation in the skill references and `TODO.md`.
