# Goal

Provide an installable, explicit-activation miku-soft Agent Skill that routes
Markdown conversion to four local upstream tools with eight pinned runtimes:

- `miku-md2html` for HTML with optional embedded images
- `miku-md2docx` for editable Word documents
- `miku-md2xlsx` for practical Excel workbooks
- `miku-md2pptx` for editable PowerPoint decks

Preserve upstream CLI behavior and diagnostics, keep the Markdown source
unchanged, make output paths reviewable, and distribute the skill with its
version-pinned self-contained CLI artifacts. Bundle both Node.js and Java
CLIs for all four formats.
