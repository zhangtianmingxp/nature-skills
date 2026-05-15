# Output Spec

## Standard bundle

Produce these files when possible:

- `paper.md`
- `source_map.json`
- `translation_notes.md`
- `assets/` for extracted images, crops, or figure snippets
- `reader.html` only when the user explicitly asks for a browser preview

## Full-text mode

When the source is a full paper, include all pages or all extractable sections in the reader. Do not limit the bundle to selected pages, a teaser excerpt, or the abstract unless the user explicitly requests a preview.

`paper.md` is the primary deliverable. It must expose paragraph-level bilingual alignment:

```markdown
<a id="S001"></a>
**Source:** p.1 S001

**Original:** ...

**中文:** ...
```

For source material that cannot be extracted or translated confidently, keep the source anchor and write a visible uncertainty note instead of dropping the block.

Equations must be readable Markdown math, not flattened PDF text. Use inline `$...$` for short expressions and display `$$...$$` for standalone equations. If an equation is split across PDF blocks, reconstruct one equation group with its own stable `E###` ID and record the contributing source blocks.

## `source_map.json`

Keep a stable source map so follow-up questions can cite the same anchors.

```json
{
  "paper": {
    "title": "",
    "venue": "",
    "source_type": "pdf|html|doi|arxiv|text",
    "language": "en",
    "source_path": ""
  },
  "blocks": [
    {
      "id": "S001",
      "page": 1,
      "type": "heading|paragraph|caption|table|table_row|note",
      "order": 1,
      "original_text": "",
      "translation": "",
      "bbox": [0, 0, 0, 0],
      "confidence": "high|medium|low",
      "refs": ["F001", "T001"],
      "insert_after": "S001"
    }
  ],
  "equations": [
    {
      "id": "E001",
      "page": 4,
      "order": 12,
      "latex": "L_{\\mathrm{MLM}}(\\theta) = -\\frac{1}{L}\\sum_{i=1}^{L}\\log P(x_i \\mid X * M; \\theta)",
      "equation_number": "(1)",
      "source_block_ids": ["S017", "S018", "S019", "S020"],
      "bbox": [0, 0, 0, 0],
      "confidence": "high|medium|low",
      "notes": ""
    }
  ],
  "pages": [
    {
      "page": 1,
      "block_ids": ["S001", "S002", "S003", "C001"]
    }
  ],
  "figures": [
    {
      "id": "F001",
      "page": 3,
      "caption_id": "C001",
      "image_path": "",
      "bbox": [0, 0, 0, 0],
      "placement_hint": "near_first_mention",
      "placed_after": "S012",
      "alt_text": ""
    }
  ],
  "glossary": [
    {
      "term": "",
      "translation": "",
      "note": ""
    }
  ]
}
```

## `paper.md`

The Markdown reader should support:

- stable headings in paper order
- paragraph-level original/Chinese pairs
- readable inline and display equations using Markdown math
- stable equation anchors for display equations
- source IDs on every substantive block
- figure/table cards near the relevant prose
- English captions and Chinese caption translations
- page navigation for full papers
- terminology notes and uncertainty notes

## `reader.html`

The page should support:

- desktop side-by-side original and translation
- mobile stacked layout
- clickable source IDs on every block
- figure cards near the relevant text
- section navigation
- page navigation for full papers

Do not add a question area unless explicitly requested.

## Layout rules

- Keep paragraph alignment stable.
- Preserve formulas as formulas. Do not leave standalone equations as fragments such as separate numerator, denominator, sum symbol, and index blocks when they can be reconstructed.
- Use `$...$` for inline math and `$$...$$` for display math.
- Keep equation numbers such as `(1)` visible and tied to the equation anchor.
- If the equation reconstruction is uncertain, include a short uncertainty note and optionally a crop of the original equation in `assets/`.
- Keep captions attached to their figures.
- Show tables where they are explained, not only where they appear in the PDF.
- Prefer semantic proximity over exact visual reconstruction.
- If a figure is referenced across multiple sections, anchor it at the first substantive discussion and link later mentions back to it.
- Crop figures and tables with the tightest valid bounding box.
- Do not use a full-page screenshot when the actual content occupies a smaller region.
- If the exact crop box cannot be verified, label the crop as approximate.
- For full papers, preserve page order and include a page index.

## Figure/table card format

```markdown
<a id="F001"></a>
### Fig. 1. 中文短标题

**Placed near:** p.3 S012
**Source:** p.4 C001

![Fig. 1](assets/fig1.png)

**Original caption:** ...

**中文图注:** ...

**Reading note:** ...
```

Every image/table asset must have a corresponding card in `paper.md`. Every card must identify the source caption and placement block.

## Equation block format

Use this format for standalone equations:

```markdown
<a id="E001"></a>
**Source:** p.4 E001; derived from p.4 S017-S020

**Original equation:**
$$
L_{\mathrm{MLM}}(\theta) = -\frac{1}{L}\sum_{i=1}^{L}\log P(x_i \mid X * M; \theta)
$$

**中文说明:** 该损失鼓励模型根据上下文预测被 mask 的 token。
```

If exact LaTeX cannot be recovered:

```markdown
**Formula confidence:** low. See `assets/equation_E001.png` for the original crop.
```

## Citation format in the page

Use short, stable source pointers:

- `p.7 S021`
- `p.4 E001`
- `p.8 C003`
- `Fig. 2`
- `Table 1`

For follow-up answers, combine page and block ID when available.
