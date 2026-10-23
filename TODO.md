# TODO

- [ ] When an upstream Node.js or Java converter publishes a release, review
  its CLI contract, update the matching bundled artifact and SHA-256 pin, then
  run the full bundle and representative conversion checks before the next
  skill release.
- [ ] Track upstream [Issue #11](https://github.com/igapyon/miku-md2xlsx-java/issues/11).
  The current release retains Java XLSX `0.10.0` with its limitation disclosed.
  When upstream fixes the behavior that removes literal underscores from
  plain text, refresh the bundled JAR, verify the replacement, and remove the
  documented limitation.
