# Roteiro — TPUs (Tensor Processing Units)

Rascunho de estrutura para um deck de ~12 slides (`/ppt "TPUs" 12`).
Ajustar profundidade conforme o público (disciplina de arquitetura /
organização de computadores).

## 1. Título
Por que o Google construiu um chip próprio para redes neurais?

## 2. Agenda
Contexto → arquitetura → systolic array → memória → evolução → comparações.

## 3. Contexto (divider + slide)
- 2013: projeção de dobrar os datacenters se todo usuário usasse busca por voz 3 min/dia.
- CPU/GPU são de propósito geral; inferência de DNN é multiplicação de matrizes em massa.
- Stat trio: 15 meses do projeto ao datacenter · 92 TOPS (TPUv1, int8) · 10–30× perf/W vs CPU/GPU da época.

## 4. O que uma DNN pede do hardware
- Inferência = MAC (multiply-accumulate) em matrizes grandes; precisão reduzida (int8/bf16) basta.
- Localidade e reuso de dados dominam o custo energético — mover dados custa mais que computar.

## 5. Arquitetura da TPUv1 (diagrama)
- Matrix Multiply Unit 256×256 · acumuladores · unified buffer · weight FIFO · interface PCIe.
- Coprocessador comandado pela CPU host (instruções CISC tipo `MatrixMultiply`).

## 6. Systolic array (slide-chave, diagrama)
- Dados fluem ritmicamente entre ULAs vizinhas; pesos ficam estacionários.
- Elimina leituras/escritas repetidas em registradores e SRAM — a energia vai para o cálculo.

## 7. Hierarquia de memória e quantização
- int8: 4× a densidade de computação de fp32; bfloat16 no treino (TPUv2+).
- HBM a partir da v2; unified buffer de 24 MiB na v1.

## 8. Evolução (timeline/gantt)
- v1 (2015, inferência) → v2/v3 (treino, pods) → v4 → v5e/v5p → Trillium/v6 → Ironwood (2025, inferência em escala).
- Pods: milhares de chips com interconexão torus 3D / OCS óptico.

## 9. TPU vs GPU vs CPU (tabela comparativa)
- Especialização × flexibilidade; perf/W; modelo de programação (XLA/JAX vs CUDA).

## 10. Números atuais (dark chart)
- Escala de um pod (exaFLOPS), largura de banda HBM, perf/W entre gerações.

## 11. Limitações e críticas
- Só brilha com tensores grandes e regulares; lock-in de ecossistema; benchmark MLPerf com nuances.

## 12. Fechamento
- Lição de arquitetura: domain-specific architectures são a resposta ao fim do scaling de Dennard.
- Referências principais.

## Fontes a citar
- Jouppi et al., "In-Datacenter Performance Analysis of a Tensor Processing Unit", ISCA 2017.
- Jouppi et al., "Ten Lessons From Three Generations of TPUs", ISCA 2021.
- Hennessy & Patterson, "A New Golden Age for Computer Architecture", CACM 2019.
- Documentação Google Cloud TPU (gerações v4/v5/Trillium/Ironwood).
