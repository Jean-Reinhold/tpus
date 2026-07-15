---
name: ppt
description: Generate a UFPel branded 16:9 presentation (HTML → PDF) with a vision-validated render loop
user-invokable: true
argument-hint: "<topic> [num_slides]"
---

# /ppt — UFPel branded presentation

Generate a 16:9 slide deck using the official UFPel visual identity: deep
navy hero slides with the blue gradient, clean light content slides,
Montserrat typography, official blue (#00408F) and sky-blue (#B5DCF1)
accents, and the official marca (colored on light surfaces, white traço on
dark — never the colored marca on dark backgrounds).

This is a thin entry point. The full machinery lives in
`.agents/skills/document-generation/` — follow its SKILL.md workflow with
`type = ppt`:

1. Read `.agents/skills/document-generation/SKILL.md` and follow it
   end-to-end (rules → scaffold → compose → render `--debug` → machine
   check → vision check both images → fix loop → `--pdf`).
2. Scaffold with:
   ```bash
   node .agents/skills/document-generation/scripts/new-deliverable.mjs ppt <kebab-name>
   ```
   Output lands in `deliverable-assets/presentations/YYYY-MM-DD-<kebab-name>/`.
3. Derive `<kebab-name>` from the user's topic
   (`/ppt "Arquitetura de TPUs"` → `arquitetura-de-tpus`).
4. Structure the deck from the user's topic: title → agenda (if ≥ 8
   slides) → sections with dividers → content slides (stat trio, insight
   cards, charts, diagrams, gantt) → closing, per `templates/ppt/RULES.md`.
   Default ~8–12 slides unless the user says otherwise.
5. **MANDATORY on every iteration:** render with `--debug` and visually Read
   BOTH images for every slide you touched — the clean `page-NN.png` (brand
   feel, hierarchy, colors) AND the annotated `page-NN.debug.png` (exact
   geometry) — plus the `layout.json` machine checks. Never ship a slide you
   have not looked at in both views.
6. Deliver `out/deliverable.pdf` (also visually Read 2–3 PDF pages — the
   print path can diverge from the PNGs) and show the user the rendered key
   slides. Commit the deliverable in full, including `out/`.
