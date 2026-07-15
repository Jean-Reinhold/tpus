# ppt — slide deck layout rules (1280×720)

Brand foundation: `../../RULES.md` — read both before composing.
`pages/` is a working 14-slide showcase, one fragment per slide: every
component below exists as a copy-paste starting point (`00-defs.html` holds
the shared SVG icon symbols — keep it first). Delete fragment files you
don't need, renumber to reorder; never leave placeholder copy in a
deliverable. `index.html` is generated — edit fragments only.

## Slide anatomy

```
LIGHT CONTENT SLIDE                       DARK HERO SLIDE (title/divider/closing)
┌────────────────────────────────────┐   ┌────────────────────────────────────┐
│ KICKER                    ~flow~   │   │ [marca]                         │
│ Heading with blue span             │   │            (ufpel-gradient bg + veil)      │
│ ┌─ .body (vertically centered) ─┐  │   │ [chip]                             │
│ │  main payload                 │  │   │ Big title with blue span           │
│ │  (stats/table/chart/diagram)  │  │   │ subtitle                           │
│ └───────────────────────────────┘  │   │ meta (author · org · date)         │
│ [marca]   deck title   n / N    │   │                                    │
└────────────────────────────────────┘   └────────────────────────────────────┘
```

- `.content` = 60px top, 72px sides, 96px bottom padding. Everything lives
  inside it except full-bleed backgrounds.
- `.body` wraps the main payload of a content slide and vertically centers
  it between heading and footer. ALWAYS use it — top-hugging content with a
  dead bottom half is the #1 amateur tell.
- Footer (content slides only): colored marca left (30px tall), deck title
  centered muted, `n / N` right. Hero slides carry no footer.
- The light-slide flow-line anchor (top-right) is baked into
  `.slide:not(.dark)::after` — don't add competing decorations near it.

## Slide types and when to use them

| Type | Class/markup | Use |
|---|---|---|
| Title | `.slide.dark` + `.hero-img` + `.hero-veil` | first slide, always |
| Agenda | `.agenda` | decks ≥ 8 slides |
| Section divider | `.slide.dark` + `.divider-num` + dimmed hero JPG (`filter: brightness(.55)`) | one per major section, number 01/02/… |
| Light content | `.slide` + `.body` | the default |
| Dark content | `.slide.dark` + `data-theme="dark"` | 1–2 per deck max, for the money moment |
| Quote | `.slide.dark` + `.quote` | testimonial/citation |
| Closing | `.slide.dark` + ufpel-gradient | last slide, contact info |

## Composition rules

1. **One payload per slide.** A stat row OR a table OR a chart pair OR a
   diagram — never two heavyweight components stacked.
2. **Stat slides**: 2–4 `.stat` blocks, notes ≤ 2 lines. If you have more
   numbers, use the stat-table instead.
3. **Charts**: max two per slide (side by side in `.cols`). Every chart in a
   `.chart` figure with a caption. Dark slides: `data-theme="dark"` on the
   slide element (ufpel-charts.js walks ancestors).
4. **Diagrams**: give the svg an explicit width/height matching the content
   area (max 1136 wide). Copy the `<defs>` markers from the template.
5. **Gantt**: `grid-template-columns: <label-col> repeat(N, 1fr)` inline —
   set N to the real number of periods. Bars: `grid-column: <start> / span <len>`
   where start is period-index + 2 (col 1 = labels). Milestones get a label
   next to the diamond when space allows.
6. **Tables**: ≤ 6 rows on a slide; more belongs in a document.
7. **Titles**: every heading gets EXACTLY one `.em` span. Hero titles ≤ 2
   lines at 72px; shrink to 64px before wrapping to 3 lines (never).
8. **UFPel-gradient slides**: text always sits over the veiled (left/dark) side;
   nothing important over the bright bottom-right glow.

## data-asset tagging

Tag: title, subtitle, marca, every payload block (`stats`, `table`,
`chart-*`, `diagram`, `gantt`, `steps`, `features`, `agenda`, `quote`,
`insight`, `callout`, `bullets`), and meta/contact blocks. The debug render
labels exactly these.

## Validation (after the global checklist in ../../RULES.md)

- [ ] `.body` present on every light/dark CONTENT slide (machine check:
      layout.json overflow should be clean AND payload roughly vertically
      centered in the debug render).
- [ ] Footer page numbers sequential and `/ N` matches actual slide count.
- [ ] Hero slides: no footer, marca top-left, chip present on title.
- [ ] Charts drawn (a blank chart area = Chart.js failed — check console
      via the debug workflow, usually a bad JSON config or path).
- [ ] Steps/feature tiles aligned on a shared horizontal axis.
- [ ] Exactly one dark-content or quote slide per section at most — dark is
      seasoning, not the meal.
