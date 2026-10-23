# Runtime Policy

## Backend selection

- Respect an explicit backend request. Use Node.js for Node.js, JavaScript, or
  `.mjs` requests; use Java for Java, jar, or Java-only requests.
- Naming a `miku-md2docx-java`, `miku-md2xlsx-java`, or
  `miku-md2pptx-java` CLI is an explicit Java request.
- If the user does not choose a backend, prefer Node.js when Node.js 22 or newer
  is available. If it is unavailable, Java 8 or newer may be used for DOCX,
  XLSX, or PPTX when the matching jar is present and Java is available.
- Do not silently switch backends when the user explicitly requests a
  Node.js-only or Java-only path.
- HTML has no Java runtime. If Java is explicitly requested for HTML, hand off
  instead of selecting Node.js without the user's consent.
- Node.js and Java converters are separate upstream implementations. Use the
  selected CLI's `--help` for its exact options; do not assume all optional
  flags or generated package details are identical.
- Java XLSX `0.10.0` treats a lone underscore as an italic delimiter and may
  remove it from plain text. For example, `REVIEW_MARKER` can become
  `REVIEWMARKER`. Use the Node.js backend when preserving literal underscores
  matters. If Java was explicitly requested, explain the limitation before
  running it and do not switch backends silently.

## Runtime lookup

Locate the directory containing this `SKILL.md` and resolve the selected
runtime to an absolute path beneath its `runtime/` directory. Keep the current
working directory in the user's project; relative input and output paths are
resolved there. Run the `.mjs` artifact with Node.js, or the jar with
`java -jar`. The versioned artifacts are self-contained and do not need a
source checkout, Maven, `npm install`, or a network connection for normal
conversion.

| Output | Node.js runtime | Java runtime |
| --- | --- | --- |
| HTML | `miku-md2html-0.3.2.mjs` | unavailable |
| DOCX | `miku-md2docx-1.1.0.mjs` | `miku-md2docx-java-1.1.0.jar` |
| XLSX | `miku-md2xlsx-0.10.0.mjs` | `miku-md2xlsx-java-0.10.0.jar` |
| PPTX | `miku-md2pptx-0.7.0.mjs` | `miku-md2pptx-java-0.7.0.jar` |

Runtime requirements:

- Node.js 22 or newer for `.mjs` artifacts.
- Java 8 or newer for the bundled `.jar` artifacts.

Do not substitute an unrelated converter, download runtimes, install
dependencies, or build a replacement converter during normal conversion. The
current pins and SHA-256 values are recorded in [upstreams.md](upstreams.md).

## Output safety

1. Resolve the intended output path and check whether it already exists.
2. If it exists, do not invoke the converter until the user explicitly asks to
   replace it. DOCX, XLSX, and PPTX CLIs overwrite existing output files
   without prompting. HTML protects existing files by default and needs
   `--force` to replace them.
3. Keep the Markdown input unchanged. Do not choose an output path that
   collides with the input or with another requested format.
4. After running, confirm exit status 0 and that the requested output exists
   and is non-empty. For multiple formats, verify each output independently.

## Diagnostics and reporting

- Preserve each CLI's stdout, stderr, exit status, warnings, and output path.
- XLSX reports normal success silently; confirm the exit status and output
  file instead of interpreting empty stdout as failure.
- Do not hide warnings or infer success from partial output after a non-zero
  exit status.
- Report the selected tool, backend, version, created file path, and any
  diagnostics that affect how the output should be reviewed.

## Missing prerequisites

If the selected runtime is missing or its required Node.js or Java version is
unavailable, stop and provide a handoff with:

- selected upstream tool and repository URL
- backend requested or selected
- required input path and intended output path
- missing runtime artifact or runtime version requirement
- next action needed from the user or upstream project

If the user did not request a specific backend, another bundled backend may be
selected only according to the backend-selection rules above.
