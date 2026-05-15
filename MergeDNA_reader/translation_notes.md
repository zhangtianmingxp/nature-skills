# Translation Notes

- Source PDF had selectable text; no OCR was required.
- Hyphenated line breaks were normalized in the reader text.
- Display equations split across PDF text blocks were reconstructed as Markdown math blocks `E001`-`E008`; confidence is recorded for each equation in `paper.md` and `source_map.json`.
- Figures and tables were cropped from rendered PDF pages into `assets/` and placed near first substantive discussion.
- Dense numeric tables are preserved as image crops with bilingual captions rather than manually retyped, reducing transcription risk.
- `extracted_text.md` contains the exact selectable text by page/block for full-source traceability.
- References are preserved as a source-grounded reference block instead of translated line by line, because reference entries should keep formal citation wording.
- The root `nature-reader` skill in `/home/ztm/.codex/skills/nature-reader/SKILL.md` was re-read before regenerating this version.
