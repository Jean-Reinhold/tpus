# Automatização de Testes de Software (dá para usar IA?)

Seminário de Engenharia de Software 1: UFPel, julho de 2026.
Apresentadores: Jean Paul Reinhold · Pedro Ivo · Jonathas.

Deliverable: `deliverable-assets/presentations/2026-07-17-automatizacao-de-testes-com-ia/`
(30 slides, 16:9, identidade UFPel: PDF em `out/deliverable.pdf`).

## Estrutura (5 capítulos)

1. **Fundamentos da automação**: pirâmide de testes (Cohn/Vocke), testes
   unitários com JUnit 5 + Mockito, TDD (Beck) como protocolo para agentes,
   ambiente reprodutível (Docker + GitHub Actions + JaCoCo).
2. **IA nos testes unitários**: geração de casos (partições, valores-limite,
   property-based/jqwik); armadilhas (asserts fracos, teste acoplado à
   implementação, Lei de Goodhart); checagem adversarial periódica.
3. **Integração na era dos serviços**: mocks rasos mentem; Testcontainers
   (infra real por teste); Dev Containers (containers.dev): a IA derruba o
   overhead de simular a infra; driver de teste e ports & adapters.
4. **Além do funcional**: pentest assistido (OWASP), revisão automática de
   PR, computer use / testes de browser por intenção (Playwright), testes de
   mutação (PIT + mutantes semânticos), "testes com gosto", o problema do
   oráculo (Barr et al., 2015) e as garantias antes impossíveis.
5. **Estudo de caso**: esta apresentação: três abordagens de geração de
   slides; nosso harness (render → layout.json → checagem por visão em loop,
   com screenshot do debug do deck de TPUs); o harness é o produto;
   citação do Karpathy (vibe coding); conclusão: IA como workhorse com
   rédea humana; referências.

## Referências principais

- Karpathy: Software 2.0 (2017); Software 3.0 / AI Startup School (2025);
  "vibe coding" (X, fev. 2025).
- Steinberger: ensaios de engenharia agêntica em steipete.me (2025).
- Beck (TDD, 2003) · Vocke (Practical Test Pyramid, 2018) ·
  Barr et al. (Oracle Problem, IEEE TSE 2015) · docs: JUnit 5,
  Testcontainers, containers.dev, PIT, Playwright · Anthropic (Claude Code).
