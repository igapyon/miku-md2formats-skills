# Goal

Provide an installable, explicit-activation miku-soft Agent Skill that routes
Markdown conversion to four local upstream tools with seven pinned runtimes:

- `miku-md2html` for self-contained HTML
- `miku-md2docx` for editable Word documents
- `miku-md2xlsx` for practical Excel workbooks
- `miku-md2pptx` for editable PowerPoint decks

Preserve upstream CLI behavior and diagnostics, keep the Markdown source
unchanged, make output paths reviewable, and distribute the skill with its
version-pinned self-contained CLI artifacts. Bundle Node.js for all four
formats; bundle Java for DOCX, XLSX, and PPTX. HTML remains Node.js-only.
