# doc — A4 document layout rules (210mm × 297mm)

Brand foundation: `../../RULES.md` — read both before composing.
`pages/` is a working 6-page showcase, one fragment per page: dark ufpel-gradient
cover, contents, three body-page patterns, dark back cover. Every component
exists as a copy-paste starting point. Delete/renumber fragment files to
restructure; `index.html` is generated — edit fragments only.

## Page anatomy

```
COVER (dark)                       BODY PAGE (light)
┌──────────────────────┐          ┌──────────────────────────┐
│ [marca]           │          │ short title    [marca]│ ← .pg-header (hairline below)
│                      │          │                          │
│   (blue ufpel-gradient glow  │          │ N  Section heading       │
│    from the bottom)  │          │ lead paragraph           │
│ [chip]               │          │ body blocks: stats /     │
│ Title + blue span    │          │ table / chart / diagram /│
│ ▬▬▬▬ blue rule       │          │ callouts / steps / gantt │
│ ▬▬ periwinkle rule   │          │                          │
│ subtitle             │          │ ▎date   short title  n/N │ ← .pg-footer
│ meta (author · date) │          └──────────────────────────┘
└──────────────────────┘
```

- Body pages: `.pg-header` (top 10mm) + `.pg-footer` (bottom 10mm) on EVERY
  light page; `.body-content` starts at `padding-top: 30mm` to clear the
  header. Dark cover/back pages carry neither.
- **Pages are fixed boxes — there is no auto-flow.** You decide what goes on
  each page; the render loop's `overflow` diagnostics (layout.json) tell you
  when a page is overfull. Move blocks to the next page, don't shrink type.
- A body page comfortably holds: 1 section heading + lead + ~3 content
  blocks (stat band, table, chart, callout, bullets…). The showcase page 3
  is at capacity — treat it as the upper bound.

## Page types

| Type | When |
|---|---|
| Cover (dark, blue ufpel-gradient) | always page 1 |
| Contents | documents ≥ 5 pages |
| Body (light) | the default |
| Section-divider (dark, `.divider`-style like ppt) | long documents (≥ 12 pages) only |
| Back cover (dark) | always last |

## Composition rules

1. **Sections**: `h2.section` with `<span class="sec-num">N</span>`; blue
   number, ink title. Subsections `h3.sub` numbered `N.M`.
2. **Stat band**: right after the section lead when the section is
   number-driven. 26pt numerals — noticeably smaller than slides; if you
   want slide-scale drama, the content belongs in a ppt.
3. **Tables**: any length that fits the page; repeat the header row if a
   table continues on the next page (copy the `<thead>`).
4. **Charts**: width ≤ 640px, height ≤ 300px per figure; always numbered
   captions ("Figure N — …").
5. **Callouts**: max one per page. Insight card: max one per section.
6. **Gantt/timeline/steps**: doc-scaled versions in the template (mm units).
   Same grid mechanics as the ppt variant.
7. **Diagrams**: viewBox width 640 (the text column). Copy `<defs>` from
   the template; keep node text ≥ 10px.
8. **Text discipline**: paragraphs ≤ 5 lines, `max-width` is set to 170mm
   by the stylesheet. Use `ul.bullets` after at most two paragraphs.
9. **Covers**: title ≤ 3 lines at 34pt. Chip needs
   `align-self: flex-start` (it sits in a flex column). Covers use the real
   ufpel-gradient JPG with `object-position: 30%` so the A4 crop stays on the
   blue/violet side — the warm corner is fine on slides, but pure pink must
   never sit behind cover text.

## Print/PDF

`--pdf` produces `out/deliverable.pdf` by re-shooting every page at
`--pdf-scale` (default 2×) and embedding those renders one per page — the
PDF is pixel-identical to the validated PNGs in every viewer. One fragment
file = one page = one PDF page; page breaks can never fall mid-content.

## data-asset tagging

Tag: marca, title, subtitle, meta, toc, every heading (`heading`),
lead, and every content block (`stats`, `table`, `chart`, `diagram`,
`gantt`, `steps`, `callout`, `insight`, `definition`, `quote`, `bullets`,
`contact`). Suffix repeated blocks (`callout-2`) — ids must be unique per
page for the overlap report to be readable.

## Validation (after the global checklist in ../../RULES.md)

- [ ] layout.json: `overflow.pageScroll` null and `escapes` empty on EVERY
      page — an overfull page is a hard fail, redistribute content.
- [ ] Header/footer present on every light page; page numbers sequential;
      `/ N` matches the real page count; short title consistent.
- [ ] Section numbers sequential; TOC page numbers match reality.
- [ ] Cover: chip, one blue span in title, both divider rules, meta block.
- [ ] Stat numerals blue; charts captioned with "Figure N".
- [ ] Nothing pink/magenta anywhere (blue-only ufpel-gradient, blue-family charts).
