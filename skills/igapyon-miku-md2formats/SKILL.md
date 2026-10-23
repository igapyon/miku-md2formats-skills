---
name: igapyon-miku-md2formats
description: Use only when the user explicitly names `igapyon-miku-md2formats`, `miku-md2formats`, `miku-md2formats-skills`, or a supported `miku-md2html`, `miku-md2docx`, `miku-md2xlsx`, `miku-md2pptx`, `miku-md2docx-java`, `miku-md2xlsx-java`, or `miku-md2pptx-java` tool for Markdown conversion.
---

# igapyon-miku-md2formats

Use this skill to select and run the appropriate miku-soft CLI for converting
Markdown into HTML, DOCX, XLSX, or PPTX. The main job is to route the requested
source and target formats to the upstream tool, keep output handling explicit,
and report the resulting file and diagnostics.

This package is an agent workflow adapter. Conversion behavior, output
structure, diagnostics, and format limitations belong to the upstream tools.
Conversion itself runs locally; it does not call an AI service, LLM API, or
network service.

## Model and Harness Dependency Reduction

- Keep deterministic document conversion in the selected bundled CLI rather
  than asking the model to recreate Office files.
- Reuse the same runtime across compatible agent environments while the agent
  handles routing, output-path decisions, and user-facing handoff.
- Treat this as reduced model and harness dependence, not complete
  independence: skill activation, filesystem permissions, process execution,
  and Node.js or Java availability still depend on the agent environment.

## Activation

Activate only when the user explicitly names `igapyon-miku-md2formats`,
`miku-md2formats`, `miku-md2formats-skills`, or one of the supported tools
`miku-md2html`, `miku-md2docx`, `miku-md2xlsx`, `miku-md2pptx`,
`miku-md2docx-java`, `miku-md2xlsx-java`, or `miku-md2pptx-java`, or when
continuing a workflow already invoked by one of those names. Do not activate
for generic Markdown conversion or generic Office editing without an explicit
miku product name.

If a focused skill for the named converter is also active, use one execution
path and do not run the same conversion twice.

## Required First Checks

1. Read [references/tool-map.md](references/tool-map.md) to select the explicit
   output format.
2. Read [references/runtime-policy.md](references/runtime-policy.md) before
   executing a converter.
3. Read [references/upstreams.md](references/upstreams.md) when release
   provenance or an upstream limitation matters.

## Conversion Workflow

1. Identify the source file and requested target format before selecting a
   runtime. Markdown input does not identify an output format; require the user
   to name HTML, DOCX, XLSX, or PPTX when it is unclear.
2. Use the requested output path. Otherwise write one output under
   `workplace/miku-md2formats-skills/` in the user's current project, with the
   matching extension.
3. Check that the output path does not already exist. Ask before replacing an
   existing output. Never overwrite the Markdown input unless the user
   explicitly asks for in-place behavior and the upstream tool can support it.
4. Select the backend under [references/runtime-policy.md](references/runtime-policy.md).
   Resolve the runtime path from the directory containing this `SKILL.md`, and
   run it with the user's project as the working directory. Confirm Node.js 22
   or newer for a Node.js CLI, or Java 8 or newer for a Java CLI. HTML is
   available only through Node.js.
5. Run only the selected converter with the input path and primary `--out`
   option. Do not add summaries, reports, manifests, debug files, or other
   secondary artifacts unless the user asks for them. Use format or template
   options only when needed for the requested output.
6. Preserve stdout, stderr, exit status, diagnostics, and the actual output
   path. Report success only when the command succeeds and the output exists.

If the user requests several output formats, run one converter per requested
format and check each output independently. Keep the Markdown input unchanged.

Use only the primary requested output during normal conversion. Optional
summary or report files remain opt-in even when an upstream CLI supports them.

## Product Limits

- HTML output is self-contained and works offline. Local supported images can
  be embedded; remote images are not downloaded. Use `--asset-root` only when
  the user needs local images from an additional directory.
- DOCX, XLSX, and PPTX generation preserves practical document structure; it
  does not promise pixel-perfect layout reconstruction.
- Java XLSX `0.10.0` can remove literal underscores from plain text. Use the
  Node.js XLSX CLI when underscores must be preserved; if Java was explicitly
  requested, explain this limitation before execution.
- The skill does not call AI, LLM APIs, OCR, or network services to convert a
  file. Do not reimplement conversion behavior in skill prose or helper code.
- If no compatible runtime is available under the backend-selection policy,
  stop and provide a handoff with the selected upstream, input, intended
  output, and missing requirement. Do not download a runtime or install
  dependencies during a normal conversion.

## References

- [references/INDEX.md](references/INDEX.md) for the reference map
- [references/tool-map.md](references/tool-map.md) for format routing
- [references/runtime-policy.md](references/runtime-policy.md) for execution
- [references/upstreams.md](references/upstreams.md) for release provenance

If local execution is not possible, follow the handoff shape in
`references/runtime-policy.md` and identify the selected upstream tool, input,
intended output, missing runtime requirement, and next action.
