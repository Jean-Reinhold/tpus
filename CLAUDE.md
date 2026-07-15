# tpus — apresentação UFPel sobre TPUs

Repositório de uma apresentação sobre TPUs (Tensor Processing Units) com a
identidade visual oficial da UFPel.

- Para gerar/editar slides use a skill `/ppt` (`.claude/skills/ppt/SKILL.md`),
  que delega para `.agents/skills/document-generation/SKILL.md` — siga o
  workflow completo (rules → scaffold → compose → render `--debug` →
  machine check → vision check → `--pdf`).
- A identidade UFPel (cores oficiais, marca, vetos do manual) está em
  `.agents/skills/document-generation/RULES.md` — é o critério de aceitação
  visual; nunca use a marca colorida sobre fundo escuro.
- Conteúdo da apresentação (roteiro, fontes) vive em `content/`.
- Idioma padrão dos slides: português (pt-BR).
- Deliverables são commitados por completo, incluindo `out/` (PNGs + PDF).
