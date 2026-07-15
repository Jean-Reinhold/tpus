# UFPel brand rules (shared by all templates)

Derived from the official Manual de Identidade Visual da Universidade
Federal de Pelotas (CCS, AGO/2021) — the manual is the ground truth for
"looks like UFPel". Each template additionally has its own
`templates/<name>/RULES.md` with page anatomy and template-specific
validation — read both before composing.

| Template | Use for | Layout rules |
|---|---|---|
| `doc` | A4 documents: reports, proposals, analyses, summaries | `templates/doc/RULES.md` |
| `ppt` | 16:9 slide decks: lectures, seminars, defenses, workshops | `templates/ppt/RULES.md` |

## 1. Brand DNA (what makes it feel like UFPel)

UFPel is **light-first, institutional, and deep-blue**. Clean white
surfaces carry near-black Montserrat type; one phrase per headline is set
in the official blue (`#00408F`, PMS 287U); pale sky-blue fills
(`#B5DCF1`, PMS 2915U) provide calm secondary structure. Dark surfaces
(covers, dividers, closings) are deep navy blue — never black — with white
type and the white (negativo) marca. Small doses of the escudo's heraldic
accents (orange `#F7A600`, green `#BCCF00`) may appear in data series and
semantic callouts, never as decoration. If a page could pass as a generic
startup pitch deck, it is off-brand: this must read as a Brazilian federal
university — sober, credible, precise.

Signature moves (use them, they ARE the brand):
- **Blue emphasis span** — one key phrase inside a headline set in
  `#00408F` ("Como funciona a *arquitetura sistólica* de uma TPU?").
- **Stat trio** — small dark label, HUGE deep-blue numeral, small muted
  description. Use it whenever numbers matter.
- **Navy insight card** — very-rounded (28–40px) deep-navy card on a light
  surface, white text + sky-blue emphasis.
- **Sky icon tile** — rounded square filled `#B5DCF1`, holding line-art in
  deep blue/ink/white. Used for steps, features, benefits.
- **UFPel-gradient surface** — the deep-blue gradient as a full-bleed
  background for covers, section dividers, and closing pages. Never as a
  text background without a scrim keeping the text zone readable.
- **Pill buttons/chips** — fully rounded, deep-blue fill with white label
  (primary) or white fill with blue label (secondary).
- **Flow lines** — thin curved lines with small square/diamond/circle
  nodes, evoking the escudo's luzeiro rays; decoration on
  gradient/dark surfaces only.

## 2. Colors (Padrões Cromáticos — official values)

| Token | Value | Use |
|---|---|---|
| `--ufpel-blue` | `#00408F` (PMS 287U) | THE accent: emphasis spans, stat numerals, links, primary pills, section numbers, chart series 1 |
| `--ufpel-navy` | `#002A5C` | Dark surfaces (covers, insight cards, table headers) — a darkened 287U, never pure black |
| `--ufpel-ink` | `#14202E` | Body text on light surfaces |
| `--ufpel-muted` | `#54677D` | Secondary text, captions, footers, axis labels |
| `--ufpel-sky` | `#B5DCF1` (PMS 2915U) | Icon tiles, secondary accents, dividers on dark |
| `--ufpel-pale` | `#EAF4FB` | Pale fills: alternating table rows, callout backgrounds, chips on light |
| `--ufpel-white` | `#FFFFFF` | Light surfaces, text on dark |
| Hero surface | `static/ufpel-hero-bg.jpg` (or the CSS `.ufpel-gradient` recreation) | Cover/divider/closing backgrounds only — never UI chrome |

Official escudo accents — data and semantics only, never decoration:
orange `#F7A600` (021U, warning callouts / chart series 3), red `#C00D0D`
(179U, danger callouts), yellow `#F9D500` (123U, avoid in UI — poor
contrast), green `#BCCF00` (382U, chart series 5). Utility callout colors:
success `#0A9B76` / warning `#E58E1A` / danger `#D64545`, each with a pale
`--*-bg` variant. **No violet, magenta, or pink anywhere.** The core
palette is deep blue / white / sky blue. ALL dark hero surfaces use the
real JPG under a veil, or the blue-only CSS `.ufpel-gradient` on dividers
and quotes. Never build hero surfaces from stacked CSS radial gradients
alone in print — they band into visible rectangles in the PDF path; the
JPG has dithering baked in.

Rules:
- Blue is for **meaning** (emphasis, data, actions), not filler. If
  everything is blue, nothing is.
- Text on navy is white or sky blue. Text on white is ink or muted.
  Blue text works on both; never put blue text directly on the gradient.
- Sky blue is a *fill*, not a text color on light surfaces (fails
  contrast) — sky-blue text only on navy.
- No hues outside this palette except inside photography/screenshots and
  the official colored marca/escudo.

## 3. Typography — Montserrat only

The official marca fonts (Avenir Next LT Pro Bold CN in the escudo, Gotham
Bold in the UFPEL sigla) are proprietary and live only inside the logo
files. All composed text uses **Montserrat** (geometric, Gotham-adjacent,
OFL-licensed), shipped as a variable font in `static/fonts/`. NEVER let a
generic sans leak in (if a render shows system-ui shapes, the font path
broke — fix it, do not ship).

| Element | Weight / size (ppt @1280×720 · doc @A4) | Color |
|---|---|---|
| Hero/cover title | Bold 700 · 60–72px · 40–50pt equivalent | white (dark bg), one blue span |
| Slide/section heading | SemiBold 600 · 36–44px | ink, optional blue span |
| Subheading / kicker | Medium 500 UPPERCASE tracked +0.08em · 14–16px | muted (light) / sky (dark) |
| Body | Regular 400 · 18–20px (ppt) · 10.5–11pt (doc) | ink / white |
| Stat numeral | Bold 700 · 56–84px | `--ufpel-blue` |
| Stat label | SemiBold 600 · 15–17px | ink / white |
| Caption, footer | Regular 400 · 12–13px | muted |
| Table header | SemiBold 600 · 14–15px | white on navy |

Rules: generous line-height (1.15 headings, 1.5–1.65 body); sentence case
everywhere except kickers; no letter-spacing on body; never justify text;
max ~72ch line length on documents. Montserrat runs wider than most sans
faces — prefer tighter tracking (-0.01em) on 60px+ titles.

## 4. Layout principles

- **Whitespace is the credibility.** Min 64px slide padding, min 18mm
  document margins, and real space between blocks. Cramped = rejected.
- **One idea per slide / one message per section.** A slide with >5 bullets
  or a doc section with >2 consecutive full-text pages needs a visual
  (stat row, diagram, table, chart).
- **Grid discipline**: content aligns to a 12-col mental grid; two-column
  splits are 50/50 or 58/42; icon tiles align to a shared baseline.
- **Rounding scale**: tiles 20–24px, cards 28–40px, pills 999px, tables 12px
  (outer only), chips 999px. Sharp corners belong to nothing except full
  bleed surfaces.
- **z-order** (matches debug bands): background/gradient `0` · decorations
  (flow lines, glows) `10` · imagery/diagram bodies `20` · text `30` ·
  chips/pills/badges/logo `40`.

## 5. Components (inventory — both templates implement these)

Every meaningful block gets `data-asset="<id>"` so the render loop can
validate it. Image assets also get `data-profile` from `static/assets.json`.

| Component | Class | Contract |
|---|---|---|
| Stat trio/row | `.stats` > `.stat` | label + numeral + note; numeral ALWAYS blue; 2–4 per row, equal widths |
| Navy insight card | `.card-insight` | ≥28px radius, navy bg, white text, ≤3 sentences, optional sky span |
| Callout | `.callout.info/.success/.warning/.danger` | pale bg + 3px left bar + bold lead word; one per section max |
| Sky tile | `.tile` | 64–96px rounded square, inline SVG icon inside, label below/beside |
| Step flow | `.steps` > `.step` | numbered "Etapa 01" + tile + name + 1-line description; horizontal (ppt) or vertical (doc) |
| Timeline | `.timeline` | vertical spine with node dots, date + title + text per entry |
| Gantt | `.gantt` | CSS-grid; blue bars (primary work), sky (secondary/parallel), navy diamond milestones; month/week header row |
| Roadmap swimlanes | `.roadmap` | horizontal lanes, rounded bars with labels inside |
| Comparison table | `.table` | navy header row, white/pale alternating body, right-aligned numerics, first col left |
| Stat table | `.table.stat-table` | numeric cells in blue semibold |
| Pill button/chip | `.pill` / `.chip` | blue fill + white text (primary), white + blue (secondary), outline (tag) |
| Quote block | `.quote` | oversized blue quote glyph, 24–28px Montserrat, attribution muted |
| Code block | `.code` | near-navy bg `#0A1526`, white/sky mono, 12px radius |
| Chart figure | `.chart` + `canvas[data-chart]` | Chart.js with UFPel theme via `static/ufpel-charts.js` — see §6 |
| Diagram (custom) | inline `<svg class="diagram">` | brand SVG kit — see §7 |
| Hero surface | `.hero-ufpel` | `static/ufpel-hero-bg.jpg` and/or the CSS `.ufpel-gradient` |
| Flow lines | `.flow-lines` svg | dark/gradient surfaces only, ≤30% canvas coverage |

## 6. Charts (Chart.js, vendored)

- Load `static/vendor/chart.umd.min.js` + `static/ufpel-charts.js` (already
  wired in the templates). Author charts as JSON next to a
  `canvas[data-chart]`; the theme applies palette, Montserrat, no
  animations.
- Palette order (official Padrões Cromáticos): deep blue → sky blue →
  orange → ink → green → blue-gray. Don't hand-pick series colors unless
  semantically needed (e.g. danger red for a failure series).
- On dark slides put `data-theme="dark"` on the canvas ancestor.
- Keep charts HONEST: axis starts at 0 for bars; label units; max ~6 series.
- Every chart lives in a `.chart` figure with `data-asset` and a caption.

## 7. Custom diagrams (brand SVG kit)

Inline `<svg class="diagram">` blocks. The templates define reusable defs —
copy the `<defs>` block from the template and draw with these primitives:

- `marker#arrow-blue` / `marker#arrow-peri` — arrowheads for connectors
- `.d-node` — white box, 1.5px ink border, 12px radius
- `.d-node-black` — navy box, white label
- `.d-node-tile` — sky fill, ink label
- `.d-edge` — 1.5px ink connector; `.d-edge-dash` dashed variant
- `.d-flow` — thin sky curve with node dots (decorative rays)
- `.d-label` — 13px Montserrat muted

Rules: orthogonal or gently-curved connectors (no diagonal spaghetti);
uniform node sizes per rank; ≥24px between nodes; every node/edge label
legible at 100%; tag the whole diagram `data-asset` and validate in the
debug render (labels are the usual collision victims).

## 8. Assets (`static/assets.json` is the registry)

- `ufpel-marca.svg` / `ufpel-marca.png` — official colored marca (escudo +
  fio separador + UFPEL sigla): **light surfaces only**. The manual
  explicitly forbids the colored marca on dark or textured backgrounds.
- `ufpel-marca-white.svg` — white traço (negativo) marca: dark surfaces only.
- `ufpel-escudo.png` — colored escudo alone: official-document contexts,
  light surfaces only.
- `ufpel-hero-bg.jpg` — deep-blue hero gradient (1920×1080, dithered).
- Manual vetoes (Utilizações Vetadas): never rotate, distort, recolor,
  outline, shadow, frame, or place the marca over textures. Min height
  ~40px rendered (15mm print equivalent). Clearance around the marca ≥ the
  escudo-to-fio gap ("X" in the manual).
- Logo placement: **top-left** on covers/heroes and dark closing pages;
  small footer variant on content pages per template rules.

## 9. Global validation checklist (every render, every template)

- [ ] Montserrat rendering everywhere (no system-font shapes).
- [ ] Exactly one blue emphasis span in the main title (not zero, not three).
- [ ] Stat numerals blue; no black or gray hero numbers.
- [ ] Blue used with meaning; sky blue never as light-surface text.
- [ ] Marca: correct variant for the surface (colored on light, white traço
      on dark — NEVER colored on dark), unmodified, clear space.
- [ ] `layout.json`: zero unintended overlaps; zero overflow flags.
- [ ] Whitespace: nothing within 24px of another block unless grouped;
      nothing within page padding except full-bleed surfaces.
- [ ] Charts themed (no Chart.js default red/blue), axes honest, captioned.
- [ ] Diagrams: no label collisions, uniform nodes, brand node styles.
- [ ] Every meaningful block carries `data-asset`.
- [ ] Gradient/hero surfaces only on covers/dividers/closings — body pages
      stay calm and white.
- [ ] No violet, magenta, or pink anywhere.
