# TPUs — Apresentação UFPel

Apresentação sobre **TPUs (Tensor Processing Units)** com a identidade
visual oficial da Universidade Federal de Pelotas, gerada como HTML →
PNG → PDF por um pipeline de renderização com validação visual.

## Estrutura

| Caminho | O que é |
|---|---|
| `.agents/skills/document-generation/` | Motor de geração: templates (ppt 16:9 e doc A4), identidade UFPel, scripts de render (Playwright) |
| `.claude/skills/ppt/` | Ponto de entrada `/ppt` para o Claude Code |
| `content/` | Roteiro e material de pesquisa da apresentação |
| `deliverable-assets/presentations/` | Decks gerados (HTML + PNGs + PDF) |

## Identidade visual

Baseada no [Manual de Identidade Visual da UFPel (CCS, 2021)](https://ccs2.ufpel.edu.br/wp/identidade-visual-ufpel/):

- **Azul oficial** `#00408F` (PMS 287U) · **azul-claro** `#B5DCF1` (PMS 2915U)
- Acentos do escudo: laranja `#F7A600`, vermelho `#C00D0D`, amarelo `#F9D500`, verde `#BCCF00`
- Marca colorida em superfícies claras; versão a traço branca em superfícies escuras (o manual veta a marca colorida sobre fundo escuro)
- Tipografia: **Montserrat** (substituta livre da Gotham Bold oficial)

## Como gerar um deck

Em uma sessão do Claude Code neste repositório:

```
/ppt "Arquitetura de TPUs" 12
```

Ou manualmente:

```bash
cd .agents/skills/document-generation && pnpm install   # uma vez
node .agents/skills/document-generation/scripts/new-deliverable.mjs ppt meu-deck
# editar deliverable-assets/presentations/<data>-meu-deck/pages/*.html
node .agents/skills/document-generation/scripts/render.mjs deliverable-assets/presentations/<data>-meu-deck --debug --pdf
```

O showcase renderizado do template está em
`deliverable-assets/presentations/2026-07-15-ufpel-template/out/`.
