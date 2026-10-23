# Handoff

## Current state

- Repository: `miku-md2formats-skills`, package version `0.3.1`.
- Installed skill: `igapyon-miku-md2formats`.
- Four output formats are covered by seven pinned self-contained runtimes:
  Node.js for HTML, DOCX, XLSX, and PPTX; Java for DOCX, XLSX, and PPTX.
- `miku-md2html` currently has no Java companion.
- Runtime provenance, source commits, release links, and SHA-256 values are in
  `skills/igapyon-miku-md2formats/references/upstreams.md`.

## Review remediation

- The routing guide explains that the runtime path comes from the directory
  containing `SKILL.md`, while the converter runs with the user's project as
  its working directory. The command examples use an absolute skill path.
- The release ZIP verifier requires `SKILL.md`, all three skill references,
  and all seven runtime artifacts. A regression test removes a required
  reference from a copied ZIP and checks that verification fails.
- Java XLSX `0.10.0` removes literal underscores in plain text in some cases;
  `REVIEW_MARKER` can become `REVIEWMARKER`. This is documented in the skill
  and upstream reference. No later upstream release was available on
  2026-10-23. Prefer the Node.js XLSX runtime when underscores must be kept.

## Verification record

- A representative Markdown conversion was manually run through all seven
  runtimes from a working directory outside the skill folder. Local-image
  handling was also checked for the formats that support it.
- That manual check used Node.js `26.12.0` and OpenJDK `17.0.21`; the minimum
  supported Node.js 22 and Java 8 versions were not tested locally.
- `npm run build` passed on 2026-10-23: both repository tests passed, the
  required-file regression check rejected the altered ZIP, and the staged ZIP
  passed `--version` checks for all seven runtimes. This build does not replace
  representative conversion checks.

## Release preparation

- The release will use the currently pinned Java XLSX `0.10.0` jar. Its
  underscore-loss limitation remains disclosed and tracked by upstream
  [Issue #11](https://github.com/igapyon/miku-md2xlsx-java/issues/11).
- `npm run build` regenerated `bundle/igapyon-miku-md2formats-skills-0.3.1.zip`
  from the current working tree. Both tests passed and all seven staged runtimes
  passed their `--version` checks.
- Local version is `0.3.1`; no local or remote Git tags were found at the last
  check. The expected matching tag is `v0.3.1`. Ensure the tag points to the
  merged release commit containing this bundle. GitHub Release publication
  remains a human UI step.

## Follow-up

When upstream publishes a Java XLSX release that preserves literal
underscores, review and pin it, verify the behavior, then remove or revise the
limitation in the skill references and `TODO.md`.
