# Decisions

- This repository is the `miku-md2formats-skills` Agent Skills companion
  package. Its installable skill is named `igapyon-miku-md2formats`.
- The package adds no standalone `miku-md2formats` converter. It routes to
  existing upstream products; `miku-md2html` is the HTML upstream tool.
- Bundle Node.js and Java CLI release assets for HTML, DOCX, XLSX, and PPTX.
  Reverse converters are outside this package's scope.
- Prefer Node.js when the backend is not specified. Use Java when requested,
  or as the default-path fallback when Node.js 22 or newer is unavailable and
  the matching Java runtime is available. Honor explicit backend-only requests
  without fallback.
- Require explicit activation by the aggregate skill/package name or one of
  the supported converter CLI names, following the activation pattern in
  `miku-ms-office-skills`.
- The old focused `igapyon-miku-md2html` skill is retired. This aggregate is
  the maintained entry point for both HTML backends and all Office outputs.
- Keep the skill as a local CLI workflow adapter. Do not duplicate conversion
  logic or add network/AI services.
- Pin the initial artifacts to the current upstream releases documented in
  `skills/igapyon-miku-md2formats/references/upstreams.md`.
- Do not generate `index.json`: the skill has one short routing map and three
  focused references, with no demonstrated consumer that benefits from a
  generated inventory.
- Use Node.js 22 as the minimum runtime and CI versions 22/24. Build and
  release on Node.js 24, then smoke-check the ZIP with Node.js 22.
- Use `miku-ms-office-skills` as the shape reference for a mature, multi-tool
  CLI-backed Agent Skills aggregate. Adapt its bundled-runtime and release
  bundle patterns; omit office-to-Markdown tools.
