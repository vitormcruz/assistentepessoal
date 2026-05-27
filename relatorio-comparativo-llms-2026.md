# Relatorio Comparativo de Modelos LLM

**Data de geracao:** 27 de maio de 2026
**Fontes primarias:** [BenchLM.ai](https://benchlm.ai/), [LiveBench](https://livebench.ai/), [LMSYS Chatbot Arena / LMArena](https://arena.ai/), [Artificial Analysis](https://artificialanalysis.ai/)

---

## Fontes consultadas

| Fonte | URL | Tipo |
|-------|-----|------|
| BenchLM.ai | https://benchlm.ai/ | Leaderboard unificado (222 benchmarks, 237 modelos) |
| LiveBench | https://livebench.ai/ | Benchmark anti-contaminacao (atualizado a cada 6 meses) |
| LMSYS / LMArena | https://arena.ai/ | Elo baseado em preferencia humana (milhoes de votos) |
| Artificial Analysis | https://artificialanalysis.ai/ | Intelligence Index, Coding Index, precos e velocidade |
| Swfte AI (Arena tracker) | https://www.swfte.com/lmarena | Tracker do LMArena com precos e velocidades |
| Presenc AI (Arena tracker) | https://presenc.ai/research/lmsys-chatbot-arena-elo-rankings-may-2026 | Snapshot do Arena com intervalos de confianca |

---

## Tabela comparativa de scores

### Scores gerais e por categoria (BenchLM.ai - escala 0-100)

| Modelo | Geral | Agentic | Coding | Reasoning | Knowledge | Multilingual | Inst. Following | Math | Contexto |
|--------|------:|--------:|-------:|----------:|----------:|-------------:|----------------:|-----:|---------:|
| **Qwen3.7 Max** | **92** | **88** | **92** | **96** | N/D | 87 | 88 | **94** | 1M |
| **MiMo-V2.5-Pro** | **81** | 68 | 57 | N/D | N/D | N/D | N/D | N/D | 1M |
| **DeepSeek V4 Pro** | **87** | **89** | **90** | N/D | N/D | 78 | N/D | N/D | 1M |
| **Kimi K2.6** | **85** | **87** | **89** | N/D | 72 | 76 | N/D | N/D | 256K |
| **GLM-5.1** | **82** | 80 | 84 | 64 | **85** | **84** | **92** | **90** | 203K |
| **GLM-5 (Reasoning)** | **80** | 79 | 70 | **88** | 82 | 82 | 81 | **92** | 200K |
| **MiMo-V2.5** | **74** | 66 | 56 | N/D | N/D | N/D | N/D | N/D | 1M |
| **DeepSeek V4 Flash** | **76** | N/D | N/D | N/D | N/D | N/D | N/D | N/D | 1M |
| **Kimi K2.5** | **64** | N/D | N/D | N/D | N/D | N/D | N/D | N/D | 256K |
| **Kimi K2.5 (Reasoning)** | **76** | N/D | N/D | N/D | N/D | N/D | N/D | N/D | 256K |
| **Qwen3.6 Plus** | **73** | 62 | 65 | 62 | 66 | **85** | 88 | N/D | 1M |
| **GLM-5** | **67** | N/D | 77 | N/D | **83** | 73 | 84 | **91** | 200K |
| **MiniMax M2.7** | **62** | N/D | N/D | N/D | N/D | N/D | N/D | N/D | 200K |
| **MiniMax M2.5** | N/D | N/D | N/D | N/D | N/D | N/D | N/D | N/D | 200K |
| **Qwen3.5 Plus** | N/D | N/D | N/D | N/D | N/D | N/D | N/D | N/D | 1M |

> **Nota:** Scores em **negrito** sao do ranking provisional do BenchLM. "N/D" indica dados nao disponiveis na fonte consultada. DeepSeek V4 Pro refere-se a variante "Max". Qwen3.5 Plus e MiniMax M2.5 nao possuem coverage suficiente no BenchLM para score geral.

### LiveBench 2026-01-08 (escala 0-100, anti-contaminacao)

| Modelo | Geral | Coding | Reasoning | Math | Language | Analysis |
|--------|------:|-------:|----------:|-----:|---------:|---------:|
| **Qwen3.7 Max** | **74.3** | 83.3 | 74.2 | 51.7 | **85.3** | 71.8 |
| **DeepSeek V4 Pro** | **73.6** | **82.7** | 70.0 | 56.7 | **90.7** | **74.5** |
| **Kimi K2.6 Thinking** | **72.2** | 79.4 | **78.6** | **58.3** | 84.3 | 65.1 |
| **Qwen3.6 Plus** | **70.9** | 75.8 | **78.2** | 55.0 | 83.7 | 69.9 |
| **GLM-5.1** | **70.2** | 72.5 | 75.4 | 55.0 | **84.9** | 63.2 |
| **Kimi K2.5 Thinking** | **69.1** | 76.0 | **77.9** | 48.3 | **84.9** | 61.4 |
| **GLM-5** | **68.9** | 69.1 | 73.6 | 55.0 | 83.5 | 67.9 |
| **DeepSeek V4 Flash** | **67.3** | 70.6 | 69.2 | 50.0 | 79.7 | 68.0 |
| **MiniMax M2.7** | **63.5** | **74.8** | 54.9 | 50.0 | 80.5 | 56.3 |
| **MiniMax M2.5** | **60.1** | 59.3 | 70.7 | 51.7 | 77.4 | 49.6 |
| **MiMo-V2-Pro** | **58.1** | 69.7 | 68.9 | 30.0 | 77.0 | 49.2 |

> **Nota:** LiveBench usa questoes renovadas a cada 6 meses para reduzir contaminacao. Os scores tendem a ser mais baixos que BenchLM. "MiMo-V2-Pro" refere-se a versao anterior (V2, nao V2.5).

### LMSYS Chatbot Arena - Elo (preferencia humana, maio 2026)

| Modelo | Elo (Text) | IC 95% | Votos |
|--------|-----------:|-------:|------:|
| **GLM-5.1** | **1472** | +/-6.0 | 12.295 |
| **Kimi K2.6** | **1462** | +/-6.1 | 10.281 |
| **MiMo-V2.5-Pro** | **1465** | +/-7.0 | 7.476 |
| **Qwen3.6 Plus** | **1447** | N/D | N/D |
| **DeepSeek V4 Pro** | **1462** | N/D | N/D |
| **DeepSeek V4 Flash** | **1432** | N/D | N/D |
| **GLM-5** | **1457** | +/-4.7 | 20.816 |
| **MiMo-V2.5** | **1429** | +/-6.3 | 9.729 |

> **Nota:** Elo do Arena mede preferencia humana direta em comparacoes cegas. Intervalos de confianca (IC) que se sobrepoem indicam empate tecnico.

### Precos (por 1M tokens, USD)

| Modelo | Input | Output | Blended | Licenca |
|--------|------:|-------:|--------:|---------|
| DeepSeek V4 Flash | $0.14 | $0.28 | $0.21 | Apache 2.0 |
| MiniMax M2.7 | $0.30 | $1.20 | $0.75 | Open Weight |
| MiniMax M2.5 | $0.15 | $1.20 | $0.68 | Open Weight |
| Qwen3.6 Plus | $0.33 | $1.95 | $1.14 | Proprietario (API) |
| Kimi K2.6 | $0.95 | $4.00 | $2.48 | Apache 2.0 |
| GLM-5 | $1.00 | $3.20 | $2.10 | MIT |
| GLM-5.1 | $1.40 | $4.40 | $2.90 | MIT |
| Qwen3.5 Plus | $0.80 | $2.00 | $1.40 | Proprietario (API) |
| DeepSeek V4 Pro | $1.74 | $3.48 | $2.61 | MIT |
| MiMo-V2.5-Pro | $1.00 | $3.00 | $2.00 | MIT |
| MiMo-V2.5 | $0.50 | $1.50 | $1.00 | MIT |
| Qwen3.7 Max | N/D | N/D | N/D | Proprietario (API) |
| Kimi K2.5 | $1.50 | $3.00 | $2.25 | MIT |

---

## Analise por categoria

### Agentic (peso 22% no BenchLM)

A categoria agentic mede a capacidade do modelo em tarefas autonomas multi-etapas: uso de ferramentas, navegacao web, automacao de terminal e resolucao de problemas complexos sem intervencao humana. Benchmarks incluem Terminal-Bench 2.0, OSWorld, BrowseComp, GAIA, Tau-Bench e WebArena.

**Qwen3.7 Max** lidera com score 88, seguido de perto por **DeepSeek V4 Pro** (89) e **Kimi K2.6** (87). O destaque do Kimi K2.6 e sua arquitetura de "swarm sampling" com ate 300 sub-agentes paralelos, que resolve tickets mais dificeis por exploracao estocastica. O **MiMo-V2.5-Pro** chama atencao pela eficiencia de tokens: atinge resultados comparaveis usando 40-60% menos tokens que concorrentes. O **MiniMax M2.7** surpreende com Elo 1495 no GDPval-AA (o mais alto entre open-weight), demonstrando forte capacidade em tarefas agentivas do mundo real.

### Coding (peso 20% no BenchLM)

Codificacao avalia geracao, correcao e refatoracao de codigo em repositorios reais. Inclui SWE-bench Verified, SWE-bench Pro, LiveCodeBench, SWE-Rebench e SciCode.

**Qwen3.7 Max** (92) e **DeepSeek V4 Pro** (90) dominam esta categoria. No SWE-bench Pro (o mais dificil), **Kimi K2.6** lidera entre open-weight com 58.6%, seguido de **MiMo-V2.5-Pro** (57.2%) e **GLM-5.1** (58.4%). O **DeepSeek V4 Pro** atinge 80.6% no SWE-bench Verified, proximo ao Claude Opus 4.6 (80.8%). O **MiniMax M2.7** marca 56.22% no SWE-Pro, impressionante para seu preco. O **GLM-5** (standard) fica para tras com 76.5 no LiveBench Coding, enquanto seu irmao GLM-5.1 chega a 84.

### Reasoning (peso 17% no BenchLM)

Raciocinio mede capacidade logica, deducao e resolucao de problemas complexos. Inclui LongBench v2, ARC-AGI-2, MRCRv2 e MuSR.

**Qwen3.7 Max** domina com score 96, muito a frente dos demais. O **GLM-5 (Reasoning)** se destaca com 88, superando ate o GLM-5.1 (64) que nao usa modo reasoning. No LiveBench Reasoning, **Kimi K2.6 Thinking** (78.6) e **Qwen3.6 Plus** (78.2) lideram entre os modelos avaliados. O **DeepSeek V4 Pro** brilha em matematica com 99.4% no AIME 2026 e 92.8% no MMLU-Pro, sugerindo forte raciocinio quantitativo.

### Knowledge (peso 12% no BenchLM)

Conhecimento avalia compreensao factual, ciencia e cultura geral. Inclui HLE, MMLU-Pro, FrontierScience, SimpleQA, GPQA e SuperGPQA.

**GLM-5.1** (85) e **GLM-5** (83) lideram entre os modelos da lista, refletindo o foco da Z.AI em treinamento com dados enciclopedicos. O GLM-5 atinge MMLU 96 e GPQA 94, os maiores scores entre open-weight. **Qwen3.7 Max** ainda nao possui score publicado nesta categoria. O **DeepSeek V4 Pro** mostra MMLU-Pro 89.7%, competitivo com os melhores proprietarios.

### Multilingual (peso 7% no BenchLM)

Avalia desempenho em multiplos idiomas. Inclui MMLU-ProX e MGSM.

**Qwen3.6 Plus** (85) e **GLM-5.1** (84) lideram esta categoria. A familia Qwen historicamente se destaca em tarefas multilingues, especialmente chines-ingles-portugues. O **Qwen3.7 Max** marca 87, o mais alto da lista. O **GLM-5** (73) fica atras, sugerindo que a versao 5.1 trouxe melhorias significativas em idiomas.

### Instruction Following (peso 5% no BenchLM)

 Mede capacidade de seguir instrucoes complexas e formatos especificos. Inclui IFEval e IFBench.

**GLM-5.1** (92) lidera com folga, seguido por **Qwen3.6 Plus** (88) e **Qwen3.7 Max** (88). O Qwen3.5 Plus era conhecido por IFBench 76.5 (superando GPT-5.2), e o 3.6 Plus melhorou ainda mais essa capacidade. Para uso em agentes que dependem de prompts estruturados e tool calling, estes modelos sao os mais confiaveis.

### Math (peso 5% no BenchLM)

Avalia matematica avancada. Inclui FrontierMath, AIME 2025, BRUMO 2025 e MATH-500.

**Qwen3.7 Max** (94) e **GLM-5 (Reasoning)** (92) lideram. O DeepSeek V4 atinge 99.4% no AIME 2026, o maior score absoluto em qualquer benchmark de matematica. O GLM-5.1 marca 95.3% no AIME, e o GLM-5 atinge 91.3%. Modelos com modo reasoning (chain-of-thought) consistentemente superam variantes standard nesta categoria.

---

## Recomendacoes por caso de uso

### 1. Busca e coleta de conteudo (pesquisa web, extracao e sintese de informacoes)

| Tipo | Modelo | Justificativa |
|------|--------|---------------|
| **Premium** | **Qwen3.7 Max** | Score geral mais alto (92), lider em reasoning (96) e multilingual (87). Contexto de 1M tokens permite processar grandes volumes de conteudo coletado. Excelente em sintetizar informacoes de multiplas fontes. |
| **Economico** | **Qwen3.6 Plus** | Score 73 a ~$0.33/$1.95 por 1M tokens (12x mais barato que Claude Opus). Contexto de 1M tokens. 78.8% no SWE-bench Verified demonstra capacidade de analisar codigo coletado. Elo Arena 1447 confirma qualidade em dialogos. |
| **Budget** | **DeepSeek V4 Flash** | Score 76 a apenas $0.14/$0.28 por 1M tokens (o mais barato da lista). Contexto de 1M tokens. 70.6 no LiveBench Coding e 69.2 em reasoning. Lider absoluto em custo-beneficio (271.43 score/$ no BenchLM). Ideal para buscas em alto volume. |

### 2. Discussao sobre temas variados (dialogo e analise, possivelmente com pesquisa web)

| Tipo | Modelo | Justificativa |
|------|--------|---------------|
| **Premium** | **Qwen3.7 Max** | Lider absoluto no BenchLM (92), com scores altos em knowledge, reasoning e multilingual. Elo Arena 1475. Melhor modelo da lista para analise profunda e discussao fundamentada sobre qualquer tema. |
| **Economico** | **GLM-5.1** | Elo Arena 1472 (mais alto entre open-weight), knowledge 85, instruction following 92. A $1.40/$4.40 por 1M tokens com licenca MIT. Excelente para discussoes que exigem precisao factual e seguimento de instrucoes complexas. |
| **Budget** | **MiMo-V2.5** | Elo Arena 1429, contexto de 1M tokens, multimodal nativo. A $0.50/$1.50 por 1M tokens com licenca MIT. 40-60% mais eficiente em tokens que concorrentes. Bom para discussoes longas com imagens ou documentos anexados. |

### 3. Planejamento de codificacao/alteracao de um repo (analise de codebase, decomposicao de tarefas, criacao de planos de implementacao)

Modelos nesta categoria se destacam por capacidade de compreender repositorios complexos, analisar requisitos, identificar dependencias e criar planos de implementacao estruturados. Importam: reasoning, knowledge, instruction following e contexto grande para absorver o repo inteiro.

| Tipo | Modelo | Justificativa |
|------|--------|---------------|
| **Premium** | **Qwen3.7 Max** | Reasoning 96 (o mais alto da lista), score geral 92, contexto de 1M tokens. Ideal para analisar codebases inteiras, entender arquitetura, identificar impactos de mudancas e decompor tarefas complexas em planos acionaveis. |
| **Economico** | **GLM-5.1** | Knowledge 85, instruction following 92, contexto de 203K. A $1.40/$4.40 por 1M tokens com MIT license. Excelente para planejamento que exige precisao factual sobre o repo e seguimento rigoroso de requisitos e constraints. |
| **Budget** | **Qwen3.6 Plus** | Reasoning 62, contexto de 1M tokens. A $0.33/$1.95 por 1M tokens. Bom para analise de codebases grandes onde o contexto extenso e mais critico que reasoning de ponta. 78.8% SWE-bench Verified demonstra capacidade de entender codigo real. |

### 4. Orquestracao de agentes (modelo que coordena, delega e gerencia multiplos sub-agentes)

Modelos nesta categoria se destacam por recursos nativos de coordenacao multi-agente: swarm sampling, Agent Teams, ou arquitetura projetada para delegar tarefas a sub-agentes e consolidar resultados.

| Tipo | Modelo | Justificativa |
|------|--------|---------------|
| **Premium** | **Kimi K2.6** | Score 85 geral, coding 89, agentic 87. Arquitetura unica com "swarm sampling" de ate 300 sub-agentes paralelos. 58.6% no SWE-Bench Pro (lider open-weight). Projetado para orquestracao multi-agente nativa. |
| **Economico** | **MiniMax M2.7** | Score 62 geral mas com SWE-Pro 56.22%, Terminal-Bench 57.0% e GDPval-AA Elo 1495 (mais alto entre open-weight). A apenas $0.30/$1.20 por 1M tokens. Suporta Agent Teams nativos para colaboracao multi-agente com identidade estavel. |
| **Budget** | **MiniMax M2.5** | SWE-bench Verified 80.2%, Multi-SWE-Bench 51.3%, suporta Agent Teams. A $0.15/$1.20 por 1M tokens (o segundo mais barato da lista). 37% mais rapido que versoes anteriores. Ideal para fluxos multi-agente de alto volume onde custo e critico. |

### 5. Agente codificador em workflow (modelo que executa tarefas de codigo em pipeline serial)

Modelos nesta categoria sao os melhores "executores" em um fluxo de trabalho serial: recebem instrucoes de um orquestrador (humano ou agente), executam tarefas de codificacao com alta qualidade e retornam resultados. Importam: coding, instruction following e eficiencia de tokens (pois workflows seriais acumulam contexto).

| Tipo | Modelo | Justificativa |
|------|--------|---------------|
| **Premium** | **DeepSeek V4 Pro** | Score 87 geral, coding 90, agentic 89. 80.6% SWE-bench Verified, 68.4% Terminal-Bench 2.0. Contexto de 1M tokens permite receber codebases inteiras como contexto do orquestrador. MIT license. O melhor executor de codigo open-weight disponivel. |
| **Economico** | **MiMo-V2.5-Pro** | 57.2% SWE-bench Pro, 78.9% SWE-bench Verified, 68.4 Terminal-Bench. Diferencial critico para workflows seriais: usa 40-60% menos tokens por trajetoria que concorrentes (70K vs 140K+). A $1/$3 por 1M tokens com MIT license. Menos tokens acumulados = menor custo em pipelines longos. |
| **Budget** | **GLM-5 (Reasoning)** | Score 80, coding 70, instruction following 84, math 92. A $1/$3.20 por 1M tokens com MIT license. Modo reasoning habilitado melhora resolucao de problemas complexos. Bom executor para tarefas de codigo que exigem raciocinio encadeado em workflows seriais. |

### 6. Codificacao local com terminal (codificacao, debug e resolucao de problemas na maquina do usuario, com operacoes em terminal)

| Tipo | Modelo | Justificativa |
|------|--------|---------------|
| **Premium** | **DeepSeek V4 Pro** | Score 87 geral, coding 90, agentic 89. 80.6% SWE-bench Verified, 68.4% Terminal-Bench 2.0 (o mais alto da lista). Contexto de 1M tokens permite analisar codebases inteiras. MIT license para self-hosting. |
| **Economico** | **DeepSeek V4 Flash** | Score 76 a apenas $0.14/$0.28 por 1M tokens (o mais barato da lista). 70.6 no LiveBench Coding. Melhor custo-beneficio absoluto: 271.43 no ranking de value do BenchLM (score/$). Roda em hardware modesto (13B params ativos). |
| **Budget** | **Qwen3.6-35B-A3B** | Self-host com custo zero (Apache 2.0). Apenas 3B params ativos, roda em GPU consumer de 24GB. 73.4% SWE-bench Verified, 49.5% SWE-bench Pro. Contexto de 262K (extensivel a 1M). Ideal para quem quer controle total e custo operacional minimo. |

---

## Tabela-resumo de recomendacoes

| Cenario | Premium | Economico | Budget |
|---------|---------|-----------|--------|
| Busca e coleta de conteudo | Qwen3.7 Max | Qwen3.6 Plus | DeepSeek V4 Flash |
| Discussao sobre temas variados | Qwen3.7 Max | GLM-5.1 | MiMo-V2.5 |
| Planejamento de codificacao | Qwen3.7 Max | GLM-5.1 | Qwen3.6 Plus |
| Orquestracao de agentes | Kimi K2.6 | MiniMax M2.7 | MiniMax M2.5 |
| Agente codificador em workflow | DeepSeek V4 Pro | MiMo-V2.5-Pro | GLM-5 (Reasoning) |
| Codificacao local com terminal | DeepSeek V4 Pro | DeepSeek V4 Flash | Qwen3.6-35B-A3B (self-host) |

---

## Disponibilidade e licenciamento

### Modelos open-weight (baixaveis para self-hosting)

| Modelo | Licenca | Parametros | Download | Observacoes |
|--------|---------|-----------|----------|-------------|
| **DeepSeek V4 Pro** | MIT | 1.6T total / 49B ativos | [Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro) \| [ModelScope](https://modelscope.cn/models/deepseek-ai/DeepSeek-V4-Pro) | Melhor open-weight geral. Requer 8x H100 para FP8. |
| **DeepSeek V4 Flash** | MIT | 284B total / 13B ativos | [Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash) \| [ModelScope](https://modelscope.cn/models/deepseek-ai/DeepSeek-V4-Flash) | Roda em 2x H100. Mais barato da lista. |
| **GLM-5.1** | MIT | 744B total / 40B ativos | [Hugging Face](https://huggingface.co/zai-org/GLM-5.1) \| [ModelScope](https://modelscope.cn/models/ZhipuAI/GLM-5.1) \| [GitHub](https://github.com/zai-org/GLM-5) | Suporta vLLM, SGLang, KTransformers. |
| **GLM-5** | MIT | 744B total / 40B ativos | [Hugging Face](https://huggingface.co/zai-org/GLM-5) \| [ModelScope](https://modelscope.cn/models/ZhipuAI/GLM-5) | Versao FP8 disponivel. |
| **Kimi K2.6** | Apache 2.0 | ~1T total / ~32B ativos | [Hugging Face](https://huggingface.co/moonshotai/Kimi-K2.6) | Swarm sampling nativo. |
| **Kimi K2.5** | Apache 2.0 | ~1T total / ~32B ativos | [Hugging Face](https://huggingface.co/moonshotai/Kimi-K2.5) | Versao anterior, ainda competitiva. |
| **MiMo-V2.5-Pro** | MIT | 1.02T total | [Hugging Face](https://huggingface.co/XiaomiMiMo/MiMo-V2.5-Pro) | 40-60% mais eficiente em tokens. |
| **MiMo-V2.5** | MIT | 1.02T total | [Hugging Face](https://huggingface.co/XiaomiMiMo/MiMo-V2.5) | Versao base, multimodal nativo. |
| **MiniMax M2.7** | Open Weight | ~10B ativos | [Hugging Face](https://huggingface.co/MiniMaxAI/MiniMax-M2.7) \| [GitHub](https://github.com/MiniMax-AI/MiniMax-M2.7) | Agent Teams nativos. |
| **MiniMax M2.5** | Open Weight | ~10B ativos | [Hugging Face](https://huggingface.co/MiniMaxAI/MiniMax-M2.5) \| [GitHub](https://github.com/MiniMax-AI/MiniMax-M2.5) | 80.2% SWE-bench Verified. |
| **Qwen3.6-35B-A3B** | Apache 2.0 | 35B total / 3B ativos | [Hugging Face](https://huggingface.co/Qwen/Qwen3.6-35B-A3B) | Roda em GPU consumer de 24GB. |
| **Qwen3.6-27B** | Apache 2.0 | 27B (dense) | [Hugging Face](https://huggingface.co/Qwen/Qwen3.6-27B) | Single-GPU inference. |

### Modelos proprietarios (API only)

| Modelo | Provedor | API | Observacoes |
|--------|----------|-----|-------------|
| **Qwen3.7 Max** | Alibaba Cloud | [Alibaba Cloud Model Studio](https://bailian.console.aliyun.com/) | $2.50/$7.50 por 1M tokens. Nao disponivel para download. |
| **Qwen3.6 Plus** | Alibaba Cloud (DashScope) | [DashScope API](https://dashscope.aliyun.com/) | $0.33/$1.95 por 1M tokens. Preview gratuito no OpenRouter. |
| **Qwen3.5 Plus** | Alibaba Cloud (DashScope) | [DashScope API](https://dashscope.aliyun.com/) | $0.80/$2.00 por 1M tokens. |

### Ferramentas para self-hosting

Para rodar modelos open-weight localmente:

| Ferramenta | Uso | Link |
|------------|-----|------|
| **vLLM** | Inferencia de alta performance | [github.com/vllm-project/vllm](https://github.com/vllm-project/vllm) |
| **SGLang** | Inferencia otimizada | [github.com/sgl-project/sglang](https://github.com/sgl-project/sglang) |
| **Ollama** | Execucao local simplificada | [ollama.com](https://ollama.com/) |
| **llama.cpp** | Execucao em CPU/GPU consumer | [github.com/ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) |
| **KTransformers** | Execucao de modelos grandes | [github.com/kvcache-ai/ktransformers](https://github.com/kvcache-ai/ktransformers) |

### Requisitos de hardware (estimativa)

| Modelo | VRAM minima (FP8) | GPUs recomendadas |
|--------|-------------------|-------------------|
| DeepSeek V4 Pro | ~1.6TB | 8x H100 80GB |
| DeepSeek V4 Flash | ~284GB | 2x H100 80GB |
| GLM-5.1 / GLM-5 | ~744GB | 8x H100 80GB |
| Kimi K2.6 / K2.5 | ~1TB | 8x H100 80GB |
| MiMo-V2.5-Pro / V2.5 | ~1TB | 8x H100 80GB |
| MiniMax M2.7 / M2.5 | ~200GB | 2x H100 80GB |
| Qwen3.6-35B-A3B | ~35GB | 1x RTX 4090 24GB (com quantizacao) |
| Qwen3.6-27B | ~27GB | 1x RTX 4090 24GB (com quantizacao) |

> **Nota:** Modelos com arquitetura MoE (Mixture-of-Experts) como DeepSeek V4, GLM-5, Kimi K2.6 e MiMo-V2.5 possuem muito mais parametros totais do que ativos. O VRAM necessario depende da implementacao e quantizacao. Versoes FP8 ou quantizadas (GGUF, AWQ) reduzem significativamente os requisitos.

---

## Observacoes finais

- **Qwen3.7 Max** e o modelo mais forte da lista em termos absolutos, mas e proprietario e ainda nao possui precos publicados nem cobertura completa de benchmarks.
- **DeepSeek V4 Pro** e o melhor open-weight geral (MIT license), liderando em coding e agentic com contexto de 1M tokens. E o executor ideal em workflows seriais de codificacao.
- **Kimi K2.6** e a escolha para orquestracao de agentes gracas a arquitetura de swarm sampling com 300 sub-agentes.
- **GLM-5.1** e o modelo mais confiavel para conhecimento e instrucoes (MIT license, Elo Arena mais alto entre open-weight).
- **MiniMax M2.7** e **DeepSeek V4 Flash** sao os campeoes de custo-beneficio, entregando capacidade competitiva a fracoes do preco dos concorrentes.
- **MiMo-V2.5-Pro** destaca-se pela eficiencia de tokens (40-60% menos tokens que concorrentes em tarefas agentivas), sendo o economico ideal para workflows seriais onde custo acumula.
- Modelos marcados como "N/D" em benchmarks nao possuem dados publicos suficientes. Suas capacidades podem ser subestimadas.

---

*Este relatorio e autocontido e nao depende de links externos para compreensao. Os dados refletem o estado dos benchmarks em maio de 2026 e podem estar desatualizados. Consulte as fontes originais para dados em tempo real.*
