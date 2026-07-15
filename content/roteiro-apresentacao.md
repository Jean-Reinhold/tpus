# Roteiro de apresentação: Arquitetura de TPUs

**Apresentador:** Jean Reinhold · Matrícula 21101175
**Deck:** `deliverable-assets/presentations/2026-07-15-arquitetura-de-tpus/out/deliverable.pdf` (17 slides)
**Duração alvo:** 13 minutos (dá para fazer em 11 acelerando as transições; 15 com perguntas)

## O que assistir e ler antes (em ordem)

1. **[CPU vs GPU vs TPU](https://www.youtube.com/watch?v=MUWAbpg1xLo)** (vídeo curto). Dá a intuição da diferença entre os três chips. É a base dos slides 4 e 6.
2. **[An in-depth look at Google's first TPU](https://cloud.google.com/blog/products/ai-machine-learning/an-in-depth-look-at-googles-first-tensor-processing-unit-tpu)** (artigo, Google Cloud Blog). A fonte principal do deck: diagrama de blocos, array sistólico, INT8, CISC e os números de desempenho. Slides 8, 9, 10 e 15 saem daqui.
3. **[All about AI Accelerators, com Adi Fuchs](https://www.youtube.com/watch?v=VQoyypYTz2U)** (entrevista longa). Contexto geral de aceleradores. Não precisa ver inteiro: foque nos trechos sobre TPU e dataflow, que ajudam a defender o slide 12 (Flynn).
4. **[How to Think About TPUs](https://jax-ml.github.io/scaling-book/tpus/)** (capítulo do JAX Scaling Book). A melhor referência para a TPU moderna: TensorCore, VMEM, HBM, ICI e a topologia em toro. Slides 13 e 14.
5. **[Google's TPU clusters explained, Lex Fridman](https://www.youtube.com/watch?v=aV0bTDKXBP8)** (clipe). Visão de datacenter e pods, bom para responder perguntas sobre escala.
6. Opcional, se quiser ir a fundo: Jouppi et al., *In-Datacenter Performance Analysis of a Tensor Processing Unit*, ISCA 2017. É o paper por trás de tudo.

## Linha do tempo

| Tempo | Slides | Bloco |
|---|---|---|
| 0:00 a 1:00 | 1 e 2 | Abertura e agenda |
| 1:00 a 4:30 | 3 a 6 | Fundamentos: TPU, redes neurais, CPU × GPU × TPU |
| 4:30 a 8:00 | 7 a 10 | Por dentro do chip: blocos, array sistólico, eficiência |
| 8:00 a 11:30 | 11 a 14 | Flynn, TPU moderna e conexão em toro |
| 11:30 a 13:00 | 15 a 17 | Números, síntese e encerramento |

Regra de bolso: uns 45 segundos por slide de conteúdo. Os divisores (3, 7, 11) são respiros de 10 segundos: anuncie a seção e siga.

## Script slide a slide

### 1 · Capa (30 s)
> "Boa tarde. Eu sou o Jean Reinhold e hoje vou falar de um processador que faz uma escolha incomum: em vez de servir para tudo, ele aposta tudo em uma única operação, que é multiplicar matrizes. É a TPU do Google, e a gente vai olhar para ela do ponto de vista de arquitetura de hardware."

### 2 · Agenda (30 s)
> "Três blocos: primeiro o que é uma TPU, o que uma rede neural calcula e a comparação com CPU e GPU. Depois a gente abre o chip. E no final, a classificação de Flynn e como milhares de chips se conectam em toro."

### 3 · Divisor: Fundamentos (10 s)
> "Começando pelo básico: por que o Google resolveu fazer um chip próprio?"

### 4 · O que é uma TPU (60 s)
- Defina ASIC: chip de aplicação específica, o oposto do processador de uso geral.
- Conte a história: em 2013 o Google calculou que, se cada usuário usasse busca por voz 3 minutos por dia, precisaria dobrar os datacenters. A resposta foi um chip próprio, pronto em 15 meses e em produção desde 2015 (Busca, Tradutor, Fotos, AlphaGo).
- Feche com o card: abre mão de rodar qualquer programa em troca de 30 a 80 vezes mais desempenho por watt.

### 5 · O que uma rede neural calcula (75 s)
- Aponte a equação: cada camada faz y = f(W·x + b). Multiplica, soma, aplica uma função simples.
- Mostre no desenho: cada seta é um peso; as setas azuis são uma linha inteira da matriz W.
- A conclusão que prepara o resto da apresentação: redes reais repetem essa conta bilhões de vezes, então quase todo o tempo é multiplicação de matrizes. Quem acelera essa conta, acelera tudo.

### 6 · CPU × GPU × TPU (75 s)
- Percorra a tabela por filosofia, não linha por linha: "a CPU gasta 30 a 40% do chip decidindo o que executar; a TPU gasta menos de 2%, o resto é conta".
- Destaque a latência determinística (sem cache nem especulação) e a linha Flynn: "guardem essa, ela volta no slide 12".
- Resumo do rodapé: CPU quer terminar rápido uma tarefa, GPU quer fazer milhares ao mesmo tempo, TPU quer o máximo de contas de matriz por watt.

### 7 · Divisor: Por dentro do chip (10 s)
> "Agora vamos abrir o chip."

### 8 · Diagrama de blocos da TPU v1 (75 s)
- Siga o fluxo com a mão: pesos descem da DRAM pela FIFO; ativações saem do host, ficam no Unified Buffer (24 MB) e entram na MXU.
- MXU: 256 por 256, ou seja 65.536 multiplicadores de 8 bits, 92 TeraOps por segundo a 700 MHz.
- Resultados passam pelos acumuladores e pela ativação (ReLU, sigmoide) e voltam ao buffer. O ponto é o loop: a memória externa quase não é usada.

### 9 · Array sistólico (90 s), o slide mais importante
- Explique o nome: "sistólico" vem de sístole. Os dados pulsam pelo chip como o sangue bombeado pelo coração.
- Mecânica: os pesos são carregados antes e ficam parados nas células; as ativações entram pela esquerda a cada ciclo; cada célula multiplica, soma e passa adiante; as somas parciais descem para os acumuladores.
- A sacada: durante a multiplicação inteira não há acesso à memória. Cada dado é lido uma vez e reaproveitado por centenas de células. É daí que vem a eficiência energética.

### 10 · Três escolhas que explicam a eficiência (60 s)
- INT8: sair de float de 32 bits para inteiro de 8 bits dá uns 25 vezes mais multiplicadores no mesmo espaço.
- Menos de 2% de controle: sem cache, sem previsão de desvio, sem execução fora de ordem.
- ISA CISC de 5 instruções: Read_Weights, MatrixMultiply, Activate e as de memória.
- Callout: o determinismo permite garantir resposta em menos de 7 ms com o datacenter cheio.

### 11 · Divisor: Classificação e escala (10 s)
> "Agora a pergunta da disciplina: onde essa máquina cai na taxonomia de Flynn?"

### 12 · Taxonomia de Flynn (90 s)
- Situe os conhecidos: CPU de 1 núcleo é SISD; multicore é MIMD; GPU é SIMD, na variante SIMT.
- A TPU está no quadrante SIMD: uma instrução (MatrixMultiply) sobre 65.536 dados.
- A seta tracejada é a parte interessante: dentro do array sistólico o dado não fica parado, ele é transformado em cadeia, célula após célula. Isso lembra MISD, e é por isso que livros citam arrays sistólicos como o exemplo (contestado) de MISD.
- Conclusão honesta: trate a TPU como SIMD; o fluxo sistólico é o detalhe que a taxonomia de 1966 não previu.

### 13 · TensorCore moderno (60 s)
- Atualize a foto: a v1 só fazia inferência; as atuais também treinam.
- Blocos: HBM (memória rápida, até 2,8 TB/s) conversa com a VMEM (rascunho 22 vezes mais rápido que a HBM), que alimenta a MXU e a VPU.
- Aponte a VPU: "olha o SIMD clássico aqui dentro, para ReLU e reduções".
- O bloco ICI liga aos vizinhos e leva ao próximo slide.

### 14 · Conexão em toro (75 s)
- Comece pelo topo: cada bolinha é um chip, cada linha é um enlace ICI.
- Malha: para ir do chip A ao chip B são 6 saltos, e cada salto custa tempo.
- Toro: as bordas se ligam de volta (as linhas tracejadas continuam do outro lado). O mesmo caminho cai para 2 saltos: a volta da linha e a volta da coluna.
- Escala real: v5e e v6e usam toro 2D 16 por 16 (256 chips); v4 e v5p usam toro 3D, com até 8.960 chips num pod.
- Conecte com a disciplina: são as mesmas topologias de redes de interconexão (malha, toro, hipercubo).

### 15 · Desempenho (45 s)
- Esquerda: por watt, a TPU v1 entregou 83 vezes a CPU e 29 vezes a GPU da época, em inferência.
- Direita: o pico por chip praticamente dobra a cada geração, da v3 (123 TFLOPs) à v6e (918).
> "Especializar compensou. Esse é o retorno de abrir mão da generalidade."

### 16 · Síntese e fontes (45 s)
- Releia as 4 mensagens: chip de matrizes; SIMD com fluxo sistólico em Flynn; CPU, GPU e TPU como escolhas diferentes; toros 2D e 3D em escala.
- Aponte as fontes para quem quiser se aprofundar.

### 17 · Encerramento (15 s)
> "Muito obrigado. Perguntas?"

## Perguntas prováveis (e respostas de bolso)

1. **"TPU substitui GPU?"** Não. A GPU segue mais flexível (gráficos, HPC, modelos com operações incomuns). A TPU ganha quando o trabalho é dominado por multiplicação de matrizes densas.
2. **"INT8 não estraga o resultado?"** A inferência tolera quantização: a rede é treinada em ponto flutuante e convertida depois; a perda de acurácia costuma ser menor que 1%. Para treino, as TPUs modernas usam bfloat16.
3. **"MISD existe de verdade?"** É o quadrante quase vazio. Arrays sistólicos são o exemplo clássico citado, mas contestado. Bom gancho para discutir os limites da taxonomia.
4. **"O PCIe não vira gargalo?"** Na v1 sim (12,5 GB/s). Por isso as TPUs modernas usam ICI dedicado entre chips e HBM dentro do pacote.

## Fontes

- Google Cloud Blog: *An in-depth look at Google's first Tensor Processing Unit* (2017). <https://cloud.google.com/blog/products/ai-machine-learning/an-in-depth-look-at-googles-first-tensor-processing-unit-tpu>
- JAX Scaling Book: *How to Think About TPUs*. <https://jax-ml.github.io/scaling-book/tpus/>
- Vídeo: *All about AI Accelerators* (com Adi Fuchs). <https://www.youtube.com/watch?v=VQoyypYTz2U>
- Vídeo: *CPU vs GPU vs TPU*. <https://www.youtube.com/watch?v=MUWAbpg1xLo>
- Vídeo: *Google's TPU clusters explained* (Lex Fridman Podcast). <https://www.youtube.com/watch?v=aV0bTDKXBP8>
- Jouppi et al., *In-Datacenter Performance Analysis of a Tensor Processing Unit*, ISCA 2017 (o paper por trás dos números).
