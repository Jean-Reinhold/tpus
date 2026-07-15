# Roteiro de apresentação — Arquitetura de TPUs

**Apresentador:** Jean Reinhold · Matrícula 21101175
**Deck:** `deliverable-assets/presentations/2026-07-15-arquitetura-de-tpus/out/deliverable.pdf` (16 slides)
**Duração alvo:** 12 minutos (cabe em 10 acelerando as transições; 15 com perguntas)

## Linha do tempo

| Tempo | Slides | Bloco |
|---|---|---|
| 0:00 – 1:00 | 1–2 | Abertura e agenda |
| 1:00 – 3:30 | 3–5 | Fundamentos: o que é uma TPU, CPU × GPU × TPU |
| 3:30 – 7:00 | 6–9 | Por dentro do chip: blocos, array sistólico, eficiência |
| 7:00 – 10:30 | 10–13 | Flynn, TPU moderna e topologia |
| 10:30 – 12:00 | 14–16 | Desempenho, síntese e encerramento |

**Regra de bolso:** ~45 s por slide de conteúdo; os divisores (3, 6, 10) são respiros de ~10 s — anuncie a seção e siga.

## Script slide a slide

### 1 · Capa (30 s)
> "Boa tarde. Eu sou o Jean Reinhold e hoje vou falar de um processador que faz uma aposta radical: em vez de servir para tudo, ele aposta tudo em uma única operação — multiplicar matrizes. É a TPU do Google, e a gente vai olhar para ela do ponto de vista de arquitetura de hardware."

### 2 · Agenda (30 s)
> "Três blocos: primeiro o que é uma TPU e como ela se compara com CPU e GPU; depois a gente abre o chip — diagrama de blocos e o array sistólico; e por fim classificamos ela na taxonomia de Flynn e vemos como milhares de chips se conectam em topologia de toro."

### 3 · Divisor — Fundamentos (10 s)
> "Começando pelos fundamentos: por que o Google resolveu construir um chip próprio?"

### 4 · O que é uma TPU (60 s)
- Defina ASIC: circuito de aplicação específica — o oposto do processador de uso geral.
- Conte a história: em 2013 o Google projetou que, se cada usuário usasse busca por voz 3 min/dia, precisaria **dobrar os datacenters**. A resposta foi um chip próprio, do projeto à produção em 15 meses (2015: Busca, Tradutor, Fotos, AlphaGo).
- Feche com o card: troca-se generalidade por 30–80× mais desempenho por watt.

### 5 · CPU × GPU × TPU (75 s)
- Percorra a tabela por **filosofia**, não linha a linha: "a CPU gasta 30–40% do silício decidindo *o que* executar; a TPU gasta menos de 2% — quase tudo é conta".
- Destaque a linha de latência (determinística — sem caches nem especulação) e a linha Flynn: "guardem essa, ela volta no slide 11".
- Regra de bolso do rodapé: CPU minimiza latência, GPU maximiza vazão, TPU maximiza matriz por joule.

### 6 · Divisor — Por dentro do chip (10 s)
> "Vamos abrir o chip."

### 7 · Diagrama de blocos da TPU v1 (75 s)
- Siga o fluxo com a mão: pesos descem da DRAM pela FIFO; ativações saem do host, ficam no Unified Buffer (24 MB) e entram na MXU.
- MXU: 256×256 = 65.536 multiplicadores-acumuladores de 8 bits, 92 TeraOps/s a 700 MHz.
- Resultados passam pelos acumuladores e pela unidade de ativação (ReLU, sigmoide) e **voltam ao buffer** — o loop é o ponto: a memória externa quase não é tocada.

### 8 · Array sistólico (90 s) — o slide mais importante
- Explique o nome: "sistólico" vem de sístole — os dados **pulsam** pelo chip como sangue bombeado pelo coração.
- Mecânica: pesos são pré-carregados e ficam **estacionários**; ativações entram pela esquerda a cada ciclo; cada célula multiplica, soma e passa adiante; somas parciais descem para os acumuladores.
- A sacada: durante toda a multiplicação **não há acesso à memória** — o dado é lido uma vez e reutilizado por centenas de células. É daí que vem a eficiência energética.

### 9 · Três decisões radicais (60 s)
- INT8: quantizar de float32 para int8 dá ~25× mais multiplicadores no mesmo silício.
- < 2% de controle: sem cache, sem previsão de desvio, sem execução fora de ordem.
- ISA CISC de ~5 instruções: `Read_Weights`, `MatrixMultiply`, `Activate`…
- Callout: o determinismo permite garantir resposta < 7 ms sob carga — crucial para servir usuários.

### 10 · Divisor — Classificação e escala (10 s)
> "Agora a pergunta da disciplina: onde essa máquina cai na taxonomia de Flynn?"

### 11 · Taxonomia de Flynn (90 s)
- Situe os conhecidos: CPU de 1 núcleo = SISD; multicore = MIMD; GPU = SIMD (na variante SIMT).
- A TPU: **uma** instrução (`MatrixMultiply`) sobre **65.536** dados → comportamento SIMD.
- A polêmica (a seta tracejada): no array sistólico os dados **se transformam** ao fluir de célula em célula — um traço que lembra MISD, e é por isso que livros citam arrays sistólicos como o exemplo (contestado) de MISD. Conclusão honesta: Flynn (1966) não captura arquiteturas de dataflow; a classificação mais defensável é "SIMD com dataflow sistólico".

### 12 · TensorCore moderno (60 s)
- Atualize a foto: a TPU v1 era só inferência; as atuais treinam também.
- Blocos: HBM (memória de alta largura de banda, até 2,8 TB/s) ↔ VMEM (scratchpad ~22× mais rápido que a HBM) → MXU (128×128) + VPU (vetorial — "olha o SIMD clássico aqui dentro").
- ICI: enlaces diretos chip a chip, que levam ao próximo slide.

### 13 · Topologia em toro (75 s)
- Malha × toro: o wraparound corta a distância máxima pela metade — compare com as topologias de redes de interconexão vistas em aula (malha, toro, hipercubo).
- Números de escala: v5e/v6e usam toro 2D 16×16 (256 chips); v4/v5p usam toro 3D — cada chip com 6 vizinhos, até 8.960 chips num pod v5p.

### 14 · Desempenho (45 s)
- Gráfico da esquerda: por watt, a TPU v1 entregou 83× a CPU e 29× a GPU contemporâneas (em inferência).
- Direita: o pico por chip dobra a cada geração — v3 123 → v6e 918 TFLOPs bf16.
> "Especialização paga: esse é o retorno de abrir mão da generalidade."

### 15 · Síntese e fontes (45 s)
- Releia as 4 mensagens: ASIC de matrizes · SIMD-sistólico em Flynn · CPU/GPU/TPU são pontos numa troca generalidade × eficiência · toros 2D/3D em escala.
- Aponte as fontes para quem quiser se aprofundar.

### 16 · Encerramento (15 s)
> "Muito obrigado. Perguntas?"

## Perguntas prováveis (e respostas de bolso)

1. **"TPU substitui GPU?"** Não — GPU continua mais flexível (gráficos, HPC, modelos com operações exóticas). TPU vence quando o workload é dominado por multiplicação de matrizes densas.
2. **"Por que INT8 não estraga o resultado?"** Inferência tolera quantização: a rede é treinada em float e convertida; a perda de acurácia costuma ser < 1% (a v1 só fazia inferência; para treino as TPUs modernas usam bfloat16).
3. **"MISD existe de verdade?"** É o quadrante quase vazio; arrays sistólicos são o exemplo clássico citado, mas contestado — bom gancho para discutir os limites da taxonomia.
4. **"E o barramento/PCIe não vira gargalo?"** Na v1 sim (12,5 GB/s); por isso as TPUs modernas usam ICI dedicado entre chips e HBM no pacote.

## Fontes

- Google Cloud Blog — *An in-depth look at Google's first Tensor Processing Unit* (2017): <https://cloud.google.com/blog/products/ai-machine-learning/an-in-depth-look-at-googles-first-tensor-processing-unit-tpu>
- JAX Scaling Book — *How to Think About TPUs*: <https://jax-ml.github.io/scaling-book/tpus/>
- Vídeo — *All about AI Accelerators: GPU, TPU, Dataflow…* (com Adi Fuchs): <https://www.youtube.com/watch?v=VQoyypYTz2U>
- Vídeo — *CPU vs GPU vs TPU*: <https://www.youtube.com/watch?v=MUWAbpg1xLo>
- Vídeo — *Google's TPU clusters explained* (Lex Fridman Podcast): <https://www.youtube.com/watch?v=aV0bTDKXBP8>
- Jouppi et al., *In-Datacenter Performance Analysis of a Tensor Processing Unit*, ISCA 2017 (paper por trás dos números de desempenho).
