---
title: IA no Desenvolvimento de Software
subtitle: Base comum de conceitos para o time de engenharia
author: Time de Adoção de IA
date: Junho 2026
---

# IA no Desenvolvimento de Software
## Base comum de conceitos para o time de engenharia

---

## Por que estamos aqui?

- Muitos termos novos nos últimos 2 anos — ruído nas conversas
- Objetivo: **vocabulário comum, zero ambiguidade**
- Foco: desenvolvedores, nível intermediário, 30-45 min

**Agenda**

| # | Tópico |
|---|--------|
| 1 | Modelos de LLM — o que são, quais existem |
| 2 | Tokens e custos — a economia da IA |
| 3 | Commands / Prompts — como falar com a IA |
| 4 | Protocolo MCP — conectando a IA ao mundo |
| 5 | Engenharia de contexto — o que a IA enxerga |
| 6 | Agentes de IA — autonomia em ação |
| 7 | Skills — módulos de conhecimento reutilizável |
| 8 | Harness — o ambiente completo de execução |
| 9 | RAG — busca + geração |
| 10 | Encerramento |

---

# 1. Modelos de LLM

---

## O que é um LLM?

**Large Language Model** — rede neural treinada sobre conjunto massivo de
texto e código. 

- Entrada: texto (prompt) → Saída: texto (completion)
- Funcionamento: predição de próximo token — **não é raciocínio consciente**

---

## Principais LLMs para código (Jun/2026)

| Modelo | Fornecedor | Destaque | Custo (input/output 1M tokens) |
|--------|-----------|----------|--------------------------------|
| Claude Opus 4.7 | Anthropic | SWE-bench 87,6% — líder em engenharia | $5 / $25 |
| GPT-5.5 | OpenAI | Terminal-Bench 75,1% — tarefas agentivas | $5 / $30 |
| Claude Sonnet 4.6 | Anthropic | Melhor custo-benefício frontier | $3 / $15 |
| Gemini 3.1 Pro | Google | 1M contexto, multimodal | $2 / $12 |
| DeepSeek V4 Pro | DeepSeek | MIT license, 75% mais barato | $0,43 / $0,87 |

**Modelos abertos (open-weight):**

| Modelo | Destaque | Licença |
|--------|----------|---------|
| Qwen3-32B | Melhor open-weight para código | Apache 2.0 |
| DeepSeek-Coder V3 | 236B MoE, frontier performance | MIT |
| StarCoder 3 | Totalmente aberto (dados + código + pesos) | OpenRAIL |
| Granite Code 34B | IBM, foco enterprise | Apache 2.0 |

> Gap aberto vs fechado no código bruto é pequeno. A diferença real está
> na qualidade do **agente** (SWE-bench: ~65% aberto vs ~82% fechado).

---

# 2. Tokens e custos

---

## O que é um token e por que importa?

Token é a **unidade atômica de processamento** de um LLM.
~1 token = ~4 caracteres em português (~0,75 palavras em inglês).

| Referência | Tokens |
|------------|--------|
| "IA" | 1 |
| "desenvolvimento" | 3 |
| Uma página A4 (~500 palavras) | ~650 |
| Arquivo de 1000 linhas de código | ~3.000-5.000 |

**Por que importa:**
- **Custo:** provedores cobram por token processado
- **Janela de contexto:** limite máximo por requisição (200K a 1M tokens em 2026)
- **Qualidade:** excesso de contexto degrada respostas (efeito "lost in the middle")

---

## Estrutura de custo

- **Input tokens:** o que você envia (prompt, contexto, ferramentas)
- **Output tokens:** o que o modelo gera — custa **3 a 5x mais** que input
- **Prompt caching:** ~90% de desconto no input quando o prefixo não muda

| Faixa | Exemplos | Input/1M tokens |
|-------|----------|-----------------|
| Ultra-baixo | GPT-5 Nano, Gemini Flash-Lite | $0,05 - $0,25 |
| Produção | Claude Sonnet 4.6, GPT-5.4 | $2,50 - $3,00 |
| Frontier | Claude Opus 4.7, GPT-5.5 | $5,00 - $10,00 |

**Cenário:** agente resolve 1 bug (10 turnos, 50K contexto, 2K output/turno):

| Modelo | Custo/tarefa | Tarefas/$1 |
|--------|-------------|------------|
| Gemini 2.5 Flash | $0,07 | 14 |
| Claude Sonnet 4.6 | $0,24 | 4 |
| Claude Opus 4.7 | $0,40 | 2,5 |

> **Regra prática:** modele o custo antes de escolher o modelo. O barato
> resolve 80% do dia a dia; reserve o frontier para tarefas críticas.

---

# 3. Commands / Prompts

---

## O que é um prompt?

**Prompt** é a instrução textual enviada ao LLM para guiar sua resposta.
Não é só "uma pergunta" — é um artefato que define papéis, regras e restrições.

Componentes:
- **System prompt:** papel, tom, comportamento esperado
- **User prompt:** a tarefa ou pergunta específica
- **Tool definitions:** esquema JSON das ferramentas disponíveis
- **Conversation history:** histórico de mensagens anteriores

**Prompt ≠ Command**

| Prompt | Command |
|--------|---------|
| Instrução livre em linguagem natural | Atalho predefinido, comportamento conhecido |
| "Explique o que esse código faz e sugira melhorias" | `/review` — dispara fluxo de code review |
| Cada uso pode ser diferente | Sempre faz a mesma coisa |

> Commands são prompts **empacotados e reutilizáveis** — economizam tempo
> e garantem consistência.

---

## Técnicas de prompting e evolução

**Técnicas fundamentais (ainda válidas em 2026):**

| Técnica | Quando usar |
|---------|-------------|
| Zero-shot | Tarefa direta: "Traduza este código para Python" |
| Few-shot | Forneça 2-3 exemplos de entrada/saída esperada |
| Chain of Thought | "Pense passo a passo antes de responder" |
| ReAct | "Raciocine, depois aja com uma ferramenta" — padrão agentivo |
| Formato estruturado | "Retorne JSON com campos: summary, risk, recommendation" |

**Evolução do foco da engenharia de IA:**

- **Prompt Engineering (2022-2024):** "O que devo dizer?"
- **Context Engineering (2025):** "Qual informação devo fornecer?"
- **Harness Engineering (2026):** "Qual sistema devo construir?"

> Prompt não morreu — foi **absorvido** como submódulo do harness.

---

# 4. Protocolo MCP

---

## O que é o Model Context Protocol?

Protocolo **aberto e padronizado** para conectar IAs a sistemas externos
(dados, APIs, ferramentas).

- Criado pela Anthropic (Nov/2024), doado à **Linux Foundation** (Dez/2025)
- +97M downloads/mês, +10.000 servidores públicos
- Suportado por Claude, ChatGPT, Gemini, VS Code, Cursor, Copilot

> **Analogia:** MCP está para IA como **USB-C** para dispositivos —
> um conector universal. Na metáfora do corpo: são os **braços, pernas
> e sentidos** que permitem à IA interagir com o mundo externo.

---

## O problema N×M e arquitetura

**Antes do MCP:** 5 modelos × 10 sistemas = **50 conectores customizados**

**Com MCP:** 1 servidor por sistema, compatível com todos os modelos = **10 integrações**

```
  MCP Client (Claude, ChatGPT, VS Code)
      ↕  JSON-RPC: resources, tools, prompts
  MCP Server (banco de dados, API, sistema de arquivos)
```

**O que mudou em 2026 (release candidate 2026-07-28):**
- Protocolo **stateless** (sem sessões persistentes)
- **MCP Apps:** interfaces HTML interativas dentro do chat
- **Tasks:** operações long-running com acompanhamento
- **OAuth 2.0 / OpenID Connect** como padrão de autorização

> MCP não é mais experimento — é o **padrão de integração** para IA agentiva.

---

## MCP na prática

**Exemplo: servidor MCP do PostgreSQL**

Expõe ferramentas: `query`, `list_tables`, `describe_table`

```
Você: "Quantos usuários ativos temos esse mês?"

Agente chama: query("SELECT COUNT(*) FROM users WHERE active = true")
      ↓
MCP Server executa no PostgreSQL
      ↓
Retorna: [{count: 15420}]
      ↓
Agente: "Temos 15.420 usuários ativos este mês."
```

O agente não "sabe SQL magicamente" — ele recebe a ferramenta, decide usá-la,
e o servidor MCP executa com segurança no ambiente controlado.

---

# 5. Engenharia de contexto

---

## O que é engenharia de contexto?

Disciplina de projetar **tudo que o modelo vê** em cada etapa da execução.

- Prompt engineering: otimiza **o que você diz**
- Context engineering: otimiza **o que o modelo enxerga**

**Os 5 componentes do contexto:**
1. System prompt (instruções de base)
2. User input (tarefa atual)
3. Conversation history (memória da sessão)
4. Tool results (resultados de chamadas anteriores)
5. Retrieved knowledge (documentos do RAG)

**As 4 operações do contexto (LangChain):**

| Operação | O que faz | Exemplo |
|----------|-----------|---------|
| Write | Salvar para depois | Sumário da conversa em memória |
| Select | Puxar na hora certa | Buscar docs relevantes via RAG |
| Compress | Reduzir mantendo essência | Resumir histórico antigo |
| Isolate | Separar entre agentes | Sub-agente recebe só o que precisa |

---

## Contexto vs Prompt na prática

**Por que contexto > prompt em 2026:**
- Janelas de **1M tokens** — carregar muito é fácil, carregar **bem** é o desafio
- Agentes de 50+ turnos: o prompt inicial é <5% do que o modelo vê
- **Lost in the middle:** informação no meio da janela longa é ignorada
- **Prompt caching** torna barato ter contexto estável grande

**Exemplo: agente analisa PR de 200 arquivos**

| Abordagem | Método | Resultado |
|-----------|--------|-----------|
| Só prompt | "Analise este PR" + dump dos 200 arquivos | Lost in the middle, análise superficial |
| Com contexto | Select (só diffs) → Compress (resume configs) → Isolate (sub-agente segurança, outro estilo) | Análise profunda e organizada |

> A qualidade do agente depende mais da **gestão do contexto** do que da
> redação do prompt inicial.

---

# 6. Agentes de IA

---

## O que é um agente?

Sistema onde um LLM decide autonomamente o que fazer em seguida —
chamando ferramentas — até atingir um objetivo.

```
Loop do agente (ReAct):
  Observar → Raciocinar → Agir (tool call) → Observar → ...
  Até: objetivo atingido ou condição de parada
```

**Ferramenta vs Agente vs Agente Customizado**

| Conceito | Definição | Exemplo |
|----------|-----------|---------|
| Ferramenta | Função que o LLM pode chamar | `web_search(query)`, `read_file(path)` |
| Agente | LLM + ferramentas + loop de decisão | Claude Code, ChatGPT com tools |
| Agente Customizado | Agente com ferramentas, regras e fluxos do seu domínio | Agente de code review com acesso ao seu repo e regras internas |

> **Ferramenta ≠ Agente.** Ferramenta é um bloco. Agente é o sistema que
> decide **quando e como** usar cada ferramenta.

---

## Tipos e modos de agentes

**Classificação por função (2026):**

| Tipo | O que faz | Exemplos |
|------|-----------|----------|
| Coding Agent | Escreve, depura, refatora, implanta | Claude Code, Cursor Agent, Codex CLI |
| Research Agent | Pesquisa multi-etapas, síntese | OpenAI Deep Research, Perplexity Pro |
| Computer-Use | Controla navegador, preenche formulários | Anthropic Computer Use |
| Multi-Agent | Coordena agentes especializados | CrewAI, AutoGen, LangGraph |

**Copilot vs Autopilot**

| Modo | Comportamento | Exemplos |
|------|---------------|----------|
| Copilot | Aumenta o dev — sugere, completa | GitHub Copilot, Cursor Tab |
| Autopilot | Age sozinho — planeja, executa, entrega PR | Claude Code, Devin, Codex Cloud |

> Maioria dos devs em 2026 usa **dois agentes**: copilot no IDE + autônomo
> no terminal para tarefas complexas.

---

# 7. Skills

---

## O que é uma Skill?

Unidade de **conhecimento + fluxo de trabalho reutilizável**, carregada
sob demanda pelo agente.

| Ferramenta | Skill |
|------------|-------|
| Função atômica: `search_db(query)` | Fluxo completo: pesquisar → sumarizar → apresentar |
| Sempre no contexto | Carregada só quando necessária |

> Ferramentas são **funções**. Skills são **módulos** compostos de múltiplas
> funções e conhecimento de domínio.

**Progressive Disclosure (divulgação progressiva):**
- Prefixo estável: catálogo (nome + 1 linha de descrição)
- Sufixo variável: definição completa, carregada só quando ativada

Isso mantém o KV-cache válido e reduz desperdício de tokens com skills
que não serão usadas na tarefa.

---

## Estrutura e exemplo

```
skills/
  code-review/
    SKILL.md          ← Instruções (carregado sob demanda)
    examples/
      good-review.md
      bad-review.md
```

**SKILL.md** contém: gatilhos de ativação, passo a passo, regras,
critérios de qualidade, ferramentas específicas, exemplos.

**Exemplo — Catálogo (prefixo estável):**
> `code-review` — Revisa PRs aplicando padrões da empresa: segurança,
> cobertura de testes, style guide.

**SKILL.md (carregado quando ativado):**
1. Leia o diff completo
2. Classifique: segurança, lógica, estilo, performance
3. Aplique checklist por categoria
4. Relatório com severity (CRITICAL/HIGH/MEDIUM/LOW)
5. Sugira correções com exemplos de código

---

# 8. Harness

---

## O que é um Harness?

> **Agente = Modelo + Harness**

O harness é o ambiente de execução que transforma um LLM puro em um agente
funcional. É **tudo que envolve o modelo.**

**As 5 peças:**

| Componente | Função | Exemplo |
|------------|--------|---------|
| **AGENTS.md** | Regras do projeto, convenções, comandos | "Use tabs. Testes: pytest. Não mexer em /infra." |
| **Skills** | Conhecimento reutilizável sob demanda | Code review, deploy, migração de DB |
| **MCP** | Conexão com sistemas externos | Servidor MCP do banco, da API interna |
| **Hooks** | Scripts que disparam em eventos | PreToolUse: validar antes de executar |
| **Sub-agentes** | Delegação para especialistas | Segurança, testes, documentação |

---

## Por que Harness é a camada certa?

- Mesmo modelo produz resultados **10x diferentes** conforme o harness
- Modelos são commodities — **a diferenciação está no ambiente**
- Resolve o que prompts sozinhos não resolvem:
  - **Segurança:** hooks bloqueiam ações perigosas (impossível burlar por prompt)
  - **Consistência:** skills garantem o mesmo padrão sempre
  - **Escala:** sub-agentes paralelizam o trabalho

**AGENTS.md — o ponto de partida:**
- Arquivo na raiz do repo, injetado no system prompt
- Máximo 300 linhas (ideal <60)
- O que o agente DEVE e NÃO DEVE fazer
- Comandos de build, teste, lint. Critérios de conclusão

**Hooks — controle obrigatório:**
- `PreToolUse` — bloqueia ação perigosa (exit code 2)
- `PostToolUse` — valida output
- Exemplo: hook que bloqueia `rm -rf /` ou `DROP TABLE` em produção

---

# 9. RAG

---

## RAG — Retrieval-Augmented Generation

Técnica que combina **busca em base de conhecimento + geração LLM.**
Fundamenta respostas em dados reais, reduzindo alucinações.

```
Usuário pergunta → Busca documentos relevantes → Injeta no contexto → LLM responde com fatos
```

**Evolução do RAG:**

| Geração | Período | Característica |
|---------|---------|----------------|
| Naive RAG | 2020-2023 | Busca simples (top-k) + geração |
| Advanced RAG | 2023-2025 | Query rewriting, hybrid search, re-ranking |
| Agentic RAG | 2025+ | O agente decide **se, quando e onde** buscar; auto-verifica resultado |

**Padrão típico em 2026:**
- Agente avalia se precisa de informação externa
- Decide entre: vector store, knowledge graph, web search, API
- Avalia qualidade do resultado e re-busca se necessário
- Auto-verifica a resposta gerada antes de entregar

> RAG é o componente mais importante de engenharia de contexto para
> sistemas que precisam de precisão factual.

---

## Obrigado! Perguntas?
