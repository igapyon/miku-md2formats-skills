# Tool Map

Choose the converter from the source and explicitly requested target format.
Markdown input alone does not determine whether the user wants HTML, DOCX,
XLSX, or PPTX. Replace `/absolute/path/to/igapyon-miku-md2formats` in the
examples with the directory containing this `SKILL.md`. Run the command with
the user's project as the working directory; relative input and output paths
are based there. Use Node.js by default and Java when requested.

| Requested output | Node.js CLI (default) | Java CLI | Notes |
| --- | --- | --- | --- |
| `.html` | `node "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2html-0.5.0.mjs" input.md --out output.html` | `java -jar "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2html-java-0.5.0.jar" input.md --out output.html` | Local raster images are referenced by default; add `--image-mode embed` to embed them. Remote images are rejected. Check the selected CLI's `--help` for backend-specific details. |
| `.docx` | `node "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2docx-1.1.0.mjs" input.md --out output.docx --summary` | `java -jar "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2docx-java-1.1.0.jar" input.md --out output.docx --summary` | Editable Word structure; local images resolve relative to the Markdown file. `--summary` reports conversion diagnostics, including missing images. Check the selected CLI's `--help` for backend-specific options. |
| `.xlsx` | `node "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2xlsx-0.10.0.mjs" input.md --out output.xlsx` | `java -jar "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2xlsx-java-0.10.0.jar" input.md --out output.xlsx` | Practical workbook structure; successful conversion is silent. Values are written as text; formulas/charts/drawings are not generated. Java `0.10.0` may remove literal underscores; use Node.js when those must be preserved. |
| `.pptx` | `node "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2pptx-0.7.0.mjs" input.md --out output.pptx` | `java -jar "/absolute/path/to/igapyon-miku-md2formats/runtime/miku-md2pptx-java-0.7.0.jar" input.md --out output.pptx` | Editable slide structure; headings level 1 and 2 start slides. Local supported images may be embedded. |

## Selection Rules

1. Use the exact requested source-to-target direction; do not infer a format
   from Markdown input.
2. Require an explicit target extension or format name for Markdown input.
3. If the output extension and requested target disagree, confirm the target
   before running a converter.
4. Use Node.js by default. If the user requests Java, use the matching jar;
   explicitly naming a `*-java` CLI also selects Java. When no backend is
   specified, Java may be used if Node.js 22 or newer is not available and a
   compatible Java runtime is available.
5. Respect explicit backend requests. Do not silently switch from a requested
   Node.js-only or Java-only path to another backend.
6. Use the bundled HTML Java CLI when Java is explicitly requested; do not
   silently substitute the Node.js backend.
7. If the selected runtime or its prerequisite is unavailable, hand off the
   selected upstream and the intended input and output paths.
8. Do not use AI, LLM APIs, OCR, or network services while routing or
   executing a conversion.

For normal conversion, create only the primary requested output. Add options
such as `--template`, `--title`, `--sheet-mode`, or `--asset-root` only when
they are needed to satisfy the requested output. Do not create summary, report,
debug, manifest, or asset output files unless the user requests them. If
several formats are requested, run one command per format and verify each
output independently.

DOCX, XLSX, and PPTX output is intended to preserve useful document structure,
not to recreate a pixel-perfect Word, Excel, or PowerPoint layout. Review the
upstream notes in `upstreams.md` when a requested feature may be limited.
